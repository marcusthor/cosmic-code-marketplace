# Migration from postgres_changes to broadcast

`postgres_changes` should be avoided due to scalability limitations (single-threaded, doesn't scale well). Use `broadcast` with database triggers for all database change notifications.

## Migration Steps

### Step 1: Replace Client Code

#### Before (using postgres_changes)
```javascript
const oldChannel = supabase
  .channel('changes')
  .on('postgres_changes', {
    event: '*',
    schema: 'public',
    table: 'messages'
  }, callback)
  .subscribe()
```

#### After (using broadcast)
```javascript
const room_id = "room_id" // or any other identifier that you use in the trigger function
const newChannel = supabase
  .channel(`messages:${room_id}:changes`, {
    config: { private: true }
  })
  .on('broadcast', { event: 'INSERT' }, callback)
  .on('broadcast', { event: 'DELETE' }, callback)
  .on('broadcast', { event: 'UPDATE' }, callback)
  .subscribe()
```

### Step 2: Add Database Trigger

Create a trigger function that broadcasts changes:

```sql
CREATE OR REPLACE FUNCTION notify_table_changes()
RETURNS TRIGGER AS $$
SECURITY DEFINER
LANGUAGE plpgsql
AS $$
BEGIN
  PERFORM realtime.broadcast_changes(
    TG_TABLE_NAME ||':' || COALESCE(NEW.id, OLD.id)::text,
    TG_OP,
    TG_OP,
    TG_TABLE_NAME,
    TG_TABLE_SCHEMA,
    NEW,
    OLD
  );
  RETURN COALESCE(NEW, OLD);
END;
$$;

-- Attach trigger to table
CREATE TRIGGER messages_broadcast_trigger
  AFTER INSERT OR UPDATE OR DELETE ON messages
  FOR EACH ROW
  EXECUTE FUNCTION notify_table_changes();
```

### Step 3: Setup Authorization

Set up RLS policies on `realtime.messages` table:

```sql
-- Enable RLS
ALTER TABLE realtime.messages ENABLE ROW LEVEL SECURITY;

-- Allow authenticated users to receive broadcasts
CREATE POLICY "users_can_receive_broadcasts"
ON realtime.messages
FOR SELECT
TO authenticated
USING (true);

-- Or more restrictive, for specific topics:
CREATE POLICY "users_can_receive_room_broadcasts"
ON realtime.messages
FOR SELECT
TO authenticated
USING (
  topic LIKE 'messages:%' AND
  -- Add your custom authorization logic here
  EXISTS (
    SELECT 1 FROM room_members
    WHERE user_id = auth.uid()
    AND room_id = SPLIT_PART(topic, ':', 2)::uuid
  )
);
```

## Benefits of Migration

### Scalability
- **postgres_changes**: Single-threaded, limited scalability
- **broadcast**: Multi-threaded, scales horizontally

### Flexibility
- **postgres_changes**: Limited to raw database changes
- **broadcast**: Custom payloads, business logic in triggers

### Performance
- **postgres_changes**: All clients receive all changes (inefficient)
- **broadcast**: Dedicated topics, targeted delivery (efficient)

### Security
- **postgres_changes**: Limited RLS control
- **broadcast**: Full RLS on `realtime.messages` table

## Complete Migration Example

### Original Implementation (postgres_changes)

```javascript
// Client code
const channel = supabase
  .channel('public:messages')
  .on('postgres_changes', {
    event: '*',
    schema: 'public',
    table: 'messages'
  }, (payload) => {
    console.log('Change received:', payload)
  })
  .subscribe()
```

### Migrated Implementation (broadcast)

#### Database Migration

```sql
-- Create trigger function
CREATE OR REPLACE FUNCTION messages_broadcast_trigger()
RETURNS TRIGGER AS $$
SECURITY DEFINER
LANGUAGE plpgsql
AS $$
BEGIN
  PERFORM realtime.broadcast_changes(
    'messages:' || COALESCE(NEW.room_id, OLD.room_id)::text,
    TG_OP,
    TG_OP,
    TG_TABLE_NAME,
    TG_TABLE_SCHEMA,
    NEW,
    OLD
  );
  RETURN COALESCE(NEW, OLD);
END;
$$;

-- Attach trigger
CREATE TRIGGER messages_broadcast_trigger
  AFTER INSERT OR UPDATE OR DELETE ON messages
  FOR EACH ROW
  EXECUTE FUNCTION messages_broadcast_trigger();

-- Enable RLS on realtime.messages
ALTER TABLE realtime.messages ENABLE ROW LEVEL SECURITY;

-- Create RLS policy
CREATE POLICY "room_members_can_receive_messages"
ON realtime.messages
FOR SELECT
TO authenticated
USING (
  topic LIKE 'messages:%' AND
  EXISTS (
    SELECT 1 FROM room_members
    WHERE user_id = auth.uid()
    AND room_id = SPLIT_PART(topic, ':', 2)::uuid
  )
);

-- Add index for performance
CREATE INDEX idx_room_members_user_room
ON room_members(user_id, room_id);
```

#### Client Code

```javascript
// Subscribe to specific room's messages
async function subscribeToMessages(roomId) {
  const channel = supabase.channel(`messages:${roomId}`, {
    config: { private: true }
  })
    .on('broadcast', { event: 'INSERT' }, (payload) => {
      console.log('New message:', payload)
    })
    .on('broadcast', { event: 'UPDATE' }, (payload) => {
      console.log('Message updated:', payload)
    })
    .on('broadcast', { event: 'DELETE' }, (payload) => {
      console.log('Message deleted:', payload)
    })

  // Set auth before subscribing
  await supabase.realtime.setAuth()

  // Subscribe
  await channel.subscribe()

  return channel
}

// Usage
const channel = await subscribeToMessages('room-123')

// Cleanup
channel.unsubscribe()
```

## Checklist

- [ ] Create database trigger function
- [ ] Attach trigger to table(s)
- [ ] Enable RLS on `realtime.messages`
- [ ] Create RLS policies for topic-based access
- [ ] Add indexes on columns used in RLS policies
- [ ] Update client code to use `broadcast`
- [ ] Set `private: true` in channel config
- [ ] Test authorization (correct users receive messages)
- [ ] Test performance (dedicated topics, no unnecessary messages)
- [ ] Remove old `postgres_changes` code
