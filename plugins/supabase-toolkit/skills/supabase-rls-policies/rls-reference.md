# RLS Policies - Complete Reference

## Helper Functions

Supabase provides some helper functions that make it easier to write Policies.

### `auth.uid()`

Returns the ID of the user making the request.

### `auth.jwt()`

Returns the JWT of the user making the request. Anything that you store in the user's `raw_app_meta_data` column or the `raw_user_meta_data` column will be accessible using this function. It's important to know the distinction between these two:

- `raw_user_meta_data` - can be updated by the authenticated user using the `supabase.auth.update()` function. It is not a good place to store authorization data.
- `raw_app_meta_data` - cannot be updated by the user, so it's a good place to store authorization data.

The `auth.jwt()` function is extremely versatile. For example, if you store some team data inside `app_metadata`, you can use it to determine whether a particular user belongs to a team. For example, if this was an array of IDs:

```sql
create policy "User is in team"
on my_table
to authenticated
using ( team_id in (select auth.jwt() -> 'app_metadata' -> 'teams'));
```

### MFA

The `auth.jwt()` function can be used to check for [Multi-Factor Authentication](https://supabase.com/docs/guides/auth/auth-mfa#enforce-rules-for-mfa-logins). For example, you could restrict a user from updating their profile unless they have at least 2 levels of authentication (Assurance Level 2):

```sql
create policy "Restrict updates."
on profiles
as restrictive
for update
to authenticated using (
  (select auth.jwt()->>'aal') = 'aal2'
);
```

## RLS Performance Recommendations

Every authorization system has an impact on performance. While row level security is powerful, the performance impact is important to keep in mind. This is especially true for queries that scan every row in a table - like many `select` operations, including those using limit, offset, and ordering.

Based on a series of [tests](https://github.com/GaryAustin1/RLS-Performance), we have a few recommendations for RLS:

### Minimize Joins

You can often rewrite your Policies to avoid joins between the source and the target table. Instead, try to organize your policy to fetch all the relevant data from the target table into an array or set, then you can use an `IN` or `ANY` operation in your filter.

For example, this is an example of a slow policy which joins the source `test_table` to the target `team_user`:

```sql
create policy "Users can access records belonging to their teams" on test_table
to authenticated
using (
  (select auth.uid()) in (
    select user_id
    from team_user
    where team_user.team_id = team_id -- joins to the source "test_table.team_id"
  )
);
```

We can rewrite this to avoid this join, and instead select the filter criteria into a set:

```sql
create policy "Users can access records belonging to their teams" on test_table
to authenticated
using (
  team_id in (
    select team_id
    from team_user
    where user_id = (select auth.uid()) -- no join
  )
);
```

## Complete Example: User-Owned Records

Here's a complete example showing all CRUD operations for a user-owned resource:

```sql
-- Enable RLS
alter table documents enable row level security;

-- Add index for performance
create index idx_documents_user_id on documents(user_id);

-- SELECT: Users can view their own documents
create policy "Users can view own documents"
on documents
for select
to authenticated
using ( (select auth.uid()) = user_id );

-- INSERT: Users can create documents
create policy "Users can create own documents"
on documents
for insert
to authenticated
with check ( (select auth.uid()) = user_id );

-- UPDATE: Users can update their own documents
create policy "Users can update own documents"
on documents
for update
to authenticated
using ( (select auth.uid()) = user_id )
with check ( (select auth.uid()) = user_id );

-- DELETE: Users can delete their own documents
create policy "Users can delete own documents"
on documents
for delete
to authenticated
using ( (select auth.uid()) = user_id );
```

## Complete Example: Team-Based Access

```sql
-- Enable RLS
alter table projects enable row level security;

-- Add indexes for performance
create index idx_projects_team_id on projects(team_id);
create index idx_team_members_user_team on team_members(user_id, team_id);

-- SELECT: Team members can view team projects
create policy "Team members can view projects"
on projects
for select
to authenticated
using (
  team_id in (
    select team_id
    from team_members
    where user_id = (select auth.uid())
  )
);

-- INSERT: Team members can create projects
create policy "Team members can create projects"
on projects
for insert
to authenticated
with check (
  team_id in (
    select team_id
    from team_members
    where user_id = (select auth.uid())
  )
);

-- UPDATE: Team members can update projects
create policy "Team members can update projects"
on projects
for update
to authenticated
using (
  team_id in (
    select team_id
    from team_members
    where user_id = (select auth.uid())
  )
)
with check (
  team_id in (
    select team_id
    from team_members
    where user_id = (select auth.uid())
  )
);

-- DELETE: Only team admins can delete projects
create policy "Team admins can delete projects"
on projects
for delete
to authenticated
using (
  team_id in (
    select team_id
    from team_members
    where user_id = (select auth.uid())
      and role = 'admin'
  )
);
```

## Public Access Example

```sql
-- Enable RLS
alter table blog_posts enable row level security;

-- SELECT: Anyone can view published posts
create policy "Anyone can view published posts"
on blog_posts
for select
to anon, authenticated
using ( status = 'published' );

-- INSERT: Only authenticated users can create posts
create policy "Authenticated users can create posts"
on blog_posts
for insert
to authenticated
with check ( (select auth.uid()) = author_id );

-- UPDATE: Authors can update their own posts
create policy "Authors can update own posts"
on blog_posts
for update
to authenticated
using ( (select auth.uid()) = author_id )
with check ( (select auth.uid()) = author_id );

-- DELETE: Authors can delete their own posts
create policy "Authors can delete own posts"
on blog_posts
for delete
to authenticated
using ( (select auth.uid()) = author_id );
```
