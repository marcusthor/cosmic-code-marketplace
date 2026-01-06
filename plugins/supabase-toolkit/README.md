# Supabase Toolkit Plugin

Complete Supabase development toolkit with best practices for database functions, migrations, Edge Functions, RLS policies, and Realtime.

## Skills Included

### 1. supabase-db-functions
Expert guidance for writing PostgreSQL database functions with security best practices.

**Key Features**:
- SECURITY INVOKER setup
- search_path configuration
- Explicit typing
- Trigger functions
- Error handling patterns

### 2. supabase-migrations
Create production-ready migration files with proper naming conventions.

**File Format**: `YYYYMMDDHHmmss_description.sql`

**Key Features**:
- Naming conventions
- RLS policy creation
- Destructive operation handling
- Migration best practices

### 3. supabase-edge-functions
Write Edge Functions using Deno runtime with TypeScript.

**Key Features**:
- Web API usage
- Shared utility imports
- Versioned dependencies (npm:, jsr:)
- Environment variables
- Deno.serve patterns

### 4. supabase-rls-policies
Optimized Row Level Security policies with performance best practices.

**Key Features**:
- Policy naming conventions
- authenticated/anon roles
- USING/WITH CHECK clauses
- Indexing strategy
- Performance optimization

**Reference**: [rls-reference.md](skills/supabase-rls-policies/rls-reference.md)

### 5. supabase-realtime
Implement Supabase Realtime using broadcast, presence, and private channels.

**Key Features**:
- Broadcast vs postgres_changes
- Presence tracking
- Topic naming (scope:entity)
- RLS authorization
- Scalability patterns

**References**:
- [broadcast-patterns.md](skills/supabase-realtime/broadcast-patterns.md)
- [authorization-guide.md](skills/supabase-realtime/authorization-guide.md)
- [migration-guide.md](skills/supabase-realtime/migration-guide.md)

### 6. postgres-style-guide
PostgreSQL SQL style guide and naming conventions.

**Key Features**:
- Lowercase SQL keywords
- snake_case naming
- Table/column conventions
- Query formatting

## Usage

Skills are automatically invoked based on context:

```
"Create a Supabase migration for users table"  → supabase-migrations
"Write a database function for search"         → supabase-db-functions
"How do I implement realtime presence?"        → supabase-realtime
"Create RLS policies for posts"                → supabase-rls-policies
```

## Installation

```bash
/plugin install supabase-toolkit@claude-marketplace
```
