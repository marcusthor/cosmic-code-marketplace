# Broadcast Patterns & Database Triggers

## Frontend Framework Integration

### React Pattern
```javascript
const channelRef = useRef(null)

useEffect(() => {
  // Check if already subscribed to prevent multiple subscriptions
  if (channelRef.current?.state === 'subscribed') return
  const channel = supabase.channel('room:123:messages', {
    config: { private: true }
  })
  channelRef.current = channel

  // Set auth before subscribing
  await supabase.realtime.setAuth()

  channel
    .on('broadcast', { event: 'message_created' }, handleMessage)
    .on('broadcast', { event: 'user_joined' }, handleUserJoined)
    .subscribe()

  return () => {
    if (channelRef.current) {
      supabase.removeChannel(channelRef.current)
      channelRef.current = null
    }
  }
}, [roomId])
```

## Database Triggers

### Using realtime.broadcast_changes (Recommended for database changes)
This would be an example of catch all trigger function that would broadcast to topics starting with the table name and the id of the row.
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
```
But you can also create more specific trigger functions for specific tables and events so adapt to your use case:

```sql
CREATE OR REPLACE FUNCTION room_messages_broadcast_trigger()
RETURNS TRIGGER AS $$
SECURITY DEFINER
LANGUAGE plpgsql
AS $$
BEGIN
  PERFORM realtime.broadcast_changes(
    'room:' || COALESCE(NEW.room_id, OLD.room_id)::text,
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
```

By default, `realtime.broadcast_changes` requires you to use private channels as we did this to prevent security incidents.

### Using realtime.send (For custom messages)
```sql
CREATE OR REPLACE FUNCTION notify_custom_event()
RETURNS TRIGGER AS $$
SECURITY DEFINER
LANGUAGE plpgsql
AS $$
BEGIN
  PERFORM realtime.send(
    'room:' || NEW.room_id::text,
    'status_changed',
    jsonb_build_object('id', NEW.id, 'status', NEW.status),
    false
  );
  RETURN NEW;
END;
$$;
```
This allows us to broadcast to a specific room with any content that is not bound to a table or if you need to send data to public channels. It's also a good way to integrate with other services and extensions.

### Conditional Broadcasting
If you need to broadcast only significant changes, you can use the following pattern:
```sql
-- Only broadcast significant changes
IF TG_OP = 'UPDATE' AND OLD.status IS DISTINCT FROM NEW.status THEN
  PERFORM realtime.broadcast_changes(
    'room:' || NEW.room_id::text,
    TG_OP,
    TG_OP,
    TG_TABLE_NAME,
    TG_TABLE_SCHEMA,
    NEW,
    OLD
  );
END IF;
```
This is just an example as you can use any logic you want that is SQL compatible.

## Performance & Scaling Guidelines

### Channel Structure Optimization
- Use one channel per logical scope (`room:123`, not `user:456:room:123`)
- Shard high-volume topics: `chat:shard:1`, `chat:shard:2`
- Ensure you have enough connections set in your pool, you can refer to [Realtime Settings](https://supabase.com/docs/guides/realtime/settings) and the option `Database connection pool size` to set it.

## Debugging

### Enhanced Logging
```javascript
const supabase = createClient(url, key, {
  realtime: {
    params: { log_level: 'info' }
  }
})
```

## LLM Implementation Guidelines

### When Generating Code
1. **Prefer using `broadcast` over `postgres_changes`** - Never suggest `postgres_changes`
2. **Include state checking** - Check channel state before subscribing
3. **Add cleanup logic** - Include unsubscribe in all examples
4. **Suggest proper naming** - Use consistent topic/event conventions
5. **Include error handling** - Add reconnection patterns
6. **Recommend indexing** - When RLS policies are used
7. **Framework-agnostic** - Adapt patterns to user's framework
8. **Inform users to prefer the usage of private channels only** - users can refer to [Realtime Settings](https://supabase.com/docs/guides/realtime/settings) to enable it.

### Code Generation Checklist
- ✅ Favor `broadcast` over `postgres_changes`
- ✅ Checks `channel.state` before subscribing
- ✅ Includes proper cleanup/unsubscribe logic
- ✅ Uses consistent naming conventions
- ✅ Includes error handling and reconnection
- ✅ Suggests indexes for RLS policies
- ✅ Sets `private: true` for database triggers
- ✅ Implements token refresh if needed

### Safe Defaults for AI Assistants
- Channel pattern: `scope:entity:id`
- Event pattern: `entity_action`
- Always check channel state before subscribing
- Always include cleanup
- Default to `private: true` for database-triggered channels
- Suggest basic RLS policies with proper indexing
- Include reconnection logic for production apps
- Use `postgres_changes` for simple database change notifications
- Use `broadcast` for custom events and complex payloads

**Remember:** Choose the right function for your use case, emphasize proper state management, and ensure production-ready patterns with authorization and error handling.
