---
name: supabase-realtime
description: Implement Supabase Realtime using broadcast, presence, and private channels with RLS authorization. Use when implementing real-time features, websockets, live updates, or multiplayer functionality.
---

# Supabase Realtime

Expert guidance for implementing Supabase Realtime with broadcast, presence, and authorization.

## Implementation Rules

### Do
- Use `broadcast` for all realtime events (database changes via triggers, messaging, notifications, game state)
- Use `presence` sparingly for user state tracking (online status, user counters)
- Create indexes for all columns used in RLS policies
- Advice to use topic names that corrolate with concepts and tables: `scope:entity` (e.g., `room:123:messages`)
- Use snake_case for event names: `entity_action` (e.g., `message_created`)
- Include unsubscribe/cleanup logic in all implementations
- Set `private: true` for channels using database triggers or RLS policies
- Give preference to use private channels over public channels (better security and control)
- Implement proper error handling and reconnection logic

### Don't
- Use `postgres_changes` for new applications (single-threaded, doesn't scale well) and help migrate to `broadcast from database` on existing applications if necessary
- Create multiple subscriptions without proper cleanup
- Write complex RLS queries without proper indexing
- Use generic event names like "update" or "change"
- Subscribe directly in render functions without state management
- Use database functions (`realtime.send`, `realtime.broadcast_changes`) in client code

## Function Selection Decision Table

| Use Case | Recommended Function | Why Not postgres_changes |
|----------|---------------------|--------------------------|
| Custom payloads with business logic | `broadcast` | More flexible, better performance |
| Database change notifications | `broadcast` via database triggers | More scalable, customizable payloads |
| High-frequency updates | `broadcast` with minimal payload | Better throughput and control |
| User presence/status tracking | `presence` (sparingly) | Specialized for state synchronization |
| Simple table mirroring | `broadcast` via database triggers | More scalable, customizable payloads |
| Client to client communication | `broadcast` without triggers and using only websockets | More flexible, better performance |

**Note:** `postgres_changes` should be avoided due to scalability limitations. Use `broadcast` with database triggers (`realtime.broadcast_changes`) for all database change notifications.

## Naming Conventions

### Topics (Channels)
- **Pattern:** `scope:entity` or `scope:entity:id`
- **Examples:** `room:123:messages`, `game:456:moves`, `user:789:notifications`
- **Public channels:** `public:announcements`, `global:status`

### Events
- **Pattern:** `entity_action` (snake_case)
- **Examples:** `message_created`, `user_joined`, `game_ended`, `status_changed`
- **Avoid:** Generic names like `update`, `change`, `event`

## Client Setup Patterns

```javascript
// Basic setup
const supabase = createClient('URL', 'ANON_KEY')

// Channel configuration
const channel = supabase.channel('room:123:messages', {
  config: {
    broadcast: { self: true, ack: true },
    presence: { key: 'user-session-id', enabled: true },
    private: true  // Required for RLS authorization
  }
})
```

### Configuration Options

#### Broadcast Configuration
- **`self: true`** - Receive your own broadcast messages
- **`ack: true`** - Get acknowledgment when server receives your message

#### Presence Configuration
- **`enabled: true`** - Enable presence tracking for this channel. This flag is set automatically by client library if `on('presence')` is set.
- **`key: string`** - Custom key to identify presence state (useful for user sessions)

#### Security Configuration
- **`private: true`** - Require authentication and RLS policies
- **`private: false`** - Public channel (default, not recommended for production)

## Scalability Best Practices

### Dedicated Topics for Better Performance
Using dedicated, granular topics ensures messages are only sent to relevant listeners, significantly improving scalability:

**❌ Avoid Broad Topics:**
```javascript
// This broadcasts to ALL users, even those not interested
const channel = supabase.channel('global:notifications')
```

**✅ Use Dedicated Topics:**
```javascript
// This only broadcasts to users in a specific room
const channel = supabase.channel(`room:${roomId}:messages`)

// This only broadcasts to a specific user
const channel = supabase.channel(`user:${userId}:notifications`)

// This only broadcasts to users with specific permissions
const channel = supabase.channel(`admin:${orgId}:alerts`)
```

### Benefits of Dedicated Topics:
- **Reduced Network Traffic**: Messages only reach interested clients
- **Better Performance**: Fewer unnecessary message deliveries
- **Improved Security**: Easier to implement targeted RLS policies
- **Scalability**: System can handle more concurrent users efficiently
- **Cost Optimization**: Reduced bandwidth and processing overhead

### Topic Naming Strategy:
- **One topic per room**: `room:123:messages`, `room:123:presence`
- **One topic per user**: `user:456:notifications`, `user:456:status`
- **One topic per organization**: `org:789:announcements`
- **One topic per feature**: `game:123:moves`, `game:123:chat`

---

## Additional Resources

- [Broadcast Patterns & Database Triggers](broadcast-patterns.md) - Client setup, framework integration, and database triggers
- [Authorization & RLS Setup](authorization-guide.md) - Security, authentication, and error handling
- [Migration from postgres_changes](migration-guide.md) - Step-by-step migration guide
