# Authorization & RLS Setup

## Basic RLS Setup
To access a private channel you need to set RLS policies against `realtime.messages` table for SELECT operations.
```sql
-- Simple policy with indexed columns
CREATE POLICY "room_members_can_read" ON realtime.messages
FOR SELECT TO authenticated
USING (
  topic LIKE 'room:%' AND
  EXISTS (
    SELECT 1 FROM room_members
    WHERE user_id = auth.uid()
    AND room_id = SPLIT_PART(topic, ':', 2)::uuid
  )
);

-- Required index for performance
CREATE INDEX idx_room_members_user_room
ON room_members(user_id, room_id);
```

To write to a private channel you need to set RLS policies against `realtime.messages` table for INSERT operations.

```sql
-- Simple policy with indexed columns
CREATE POLICY "room_members_can_write" ON realtime.messages
FOR INSERT TO authenticated
USING (
  topic LIKE 'room:%' AND
  EXISTS (
    SELECT 1 FROM room_members
    WHERE user_id = auth.uid()
    AND room_id = SPLIT_PART(topic, ':', 2)::uuid
  )
);
```

## Client Authorization
```javascript
const channel = supabase.channel('room:123:messages', {
  config: { private: true }
})
  .on('broadcast', { event: 'message_created' }, handleMessage)
  .on('broadcast', { event: 'user_joined' }, handleUserJoined)

// Set auth before subscribing
await supabase.realtime.setAuth()

// Subscribe after auth is set
await channel.subscribe()
```

## Enhanced Security: Private-Only Channels
**Enable private-only channels** in Realtime Settings (Dashboard > Project Settings > Realtime Settings) to enforce authorization on all channels and prevent public channel access. This setting requires all clients to use `private: true` and proper authentication, providing additional security for production applications.

## Error Handling & Reconnection

### Automatic Reconnection (Built-in)
**Supabase Realtime client handles reconnection automatically:**
- Built-in exponential backoff for connection retries
- Automatic channel rejoining after network interruptions
- Configurable reconnection timing via `reconnectAfterMs` option

### Channel States
The client automatically manages these states:
- **`SUBSCRIBED`** - Successfully connected and receiving messages
- **`TIMED_OUT`** - Connection attempt timed out
- **`CLOSED`** - Channel is closed
- **`CHANNEL_ERROR`** - Error occurred, client will automatically retry

```javascript
// Client automatically reconnects with built-in logic
const supabase = createClient('URL', 'ANON_KEY', {
  realtime: {
    params: {
      log_level: 'info',
      reconnectAfterMs: 1000 // Custom reconnection timing
    }
  }
})

// Simple connection state monitoring
channel.subscribe((status, err) => {
  switch (status) {
    case 'SUBSCRIBED':
      console.log('Connected (or reconnected)')
      break
    case 'CHANNEL_ERROR':
      console.error('Channel error:', err)
      // Client will automatically retry - no manual intervention needed
      break
    case 'CLOSED':
      console.log('Channel closed')
      break
  }
})
```

## Complete Authorization Example

### Database Setup

```sql
-- Enable RLS on realtime.messages
ALTER TABLE realtime.messages ENABLE ROW LEVEL SECURITY;

-- Policy for room-based access
CREATE POLICY "Room members can read messages"
ON realtime.messages
FOR SELECT
TO authenticated
USING (
  topic LIKE 'room:%' AND
  EXISTS (
    SELECT 1 FROM room_members
    WHERE user_id = auth.uid()
    AND room_id = SPLIT_PART(topic, ':', 2)::uuid
  )
);

CREATE POLICY "Room members can send messages"
ON realtime.messages
FOR INSERT
TO authenticated
WITH CHECK (
  topic LIKE 'room:%' AND
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

### Client Implementation

```javascript
import { createClient } from '@supabase/supabase-js'

const supabase = createClient(
  process.env.SUPABASE_URL,
  process.env.SUPABASE_ANON_KEY,
  {
    realtime: {
      params: {
        log_level: 'info'
      }
    }
  }
)

async function subscribeToRoom(roomId) {
  const channel = supabase.channel(`room:${roomId}:messages`, {
    config: {
      private: true,
      broadcast: { self: true, ack: true }
    }
  })

  // Set authentication
  await supabase.realtime.setAuth()

  // Subscribe to events
  channel
    .on('broadcast', { event: 'message_created' }, (payload) => {
      console.log('New message:', payload)
    })
    .subscribe((status, err) => {
      if (status === 'SUBSCRIBED') {
        console.log('Successfully subscribed to room')
      } else if (status === 'CHANNEL_ERROR') {
        console.error('Subscription error:', err)
      }
    })

  return channel
}

// Usage
const channel = await subscribeToRoom('123e4567-e89b-12d3-a456-426614174000')

// Send a message
await channel.send({
  type: 'broadcast',
  event: 'message_created',
  payload: { text: 'Hello, room!' }
})

// Cleanup on unmount
channel.unsubscribe()
```
