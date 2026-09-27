# Data Model — HatuaBoard

## 1. Entity Relationship Diagram

```mermaid
erDiagram
  PROFILES ||--o{ WORKSPACE_MEMBERS : has
  WORKSPACES ||--o{ WORKSPACE_MEMBERS : has
  WORKSPACES ||--o{ BOARDS : contains
  BOARDS ||--o{ LISTS : contains
  LISTS ||--o{ TASKS : contains
  TASKS ||--o{ COMMENTS : has
  TASKS ||--o{ ATTACHMENTS : has
  TASKS ||--o{ TASK_LABELS : has
  LABELS ||--o{ TASK_LABELS : has
  PROFILES ||--o{ TASKS : assigned

  PROFILES {
    uuid id PK
    text email
    text full_name
    text avatar_url
    timestamptz created_at
  }

  WORKSPACES {
    uuid id PK
    text name
    uuid owner_id FK
    timestamptz created_at
  }

  WORKSPACE_MEMBERS {
    uuid id PK
    uuid workspace_id FK
    uuid user_id FK
    text role
    timestamptz joined_at
  }

  BOARDS {
    uuid id PK
    uuid workspace_id FK
    text name
    text description
    boolean archived
    timestamptz created_at
  }

  LISTS {
    uuid id PK
    uuid board_id FK
    text name
    int position
  }

  TASKS {
    uuid id PK
    uuid list_id FK
    text title
    text description
    uuid assignee_id FK
    date due_date
    int position
    text priority
    boolean archived
    timestamptz created_at
  }

  COMMENTS {
    uuid id PK
    uuid task_id FK
    uuid author_id FK
    text body
    timestamptz created_at
  }

  ATTACHMENTS {
    uuid id PK
    uuid task_id FK
    text storage_path
    text filename
    int size_bytes
  }

  LABELS {
    uuid id PK
    uuid workspace_id FK
    text name
    text color
  }

  TASK_LABELS {
    uuid task_id FK
    uuid label_id FK
  }
```

## 2. Tables

### profiles
Mirrors `auth.users`. Created via trigger on signup.

| Column | Type | Notes |
|---|---|---|
| id | uuid | PK, = auth.users.id |
| email | text | unique |
| full_name | text | |
| avatar_url | text | |
| created_at | timestamptz | default now() |

### workspaces
| Column | Type | Notes |
|---|---|---|
| id | uuid | PK |
| name | text | |
| owner_id | uuid | FK profiles.id |
| created_at | timestamptz | |

### workspace_members
| Column | Type | Notes |
|---|---|---|
| id | uuid | PK |
| workspace_id | uuid | FK workspaces.id |
| user_id | uuid | FK profiles.id |
| role | text | owner \| admin \| member \| viewer |
| joined_at | timestamptz | |

Unique: (workspace_id, user_id)

### boards
| Column | Type | Notes |
|---|---|---|
| id | uuid | PK |
| workspace_id | uuid | FK |
| name | text | |
| description | text | |
| archived | boolean | default false |
| created_at | timestamptz | |

### lists
| Column | Type | Notes |
|---|---|---|
| id | uuid | PK |
| board_id | uuid | FK |
| name | text | |
| position | int | ordering |

### tasks
| Column | Type | Notes |
|---|---|---|
| id | uuid | PK |
| list_id | uuid | FK |
| title | text | |
| description | text | |
| assignee_id | uuid | FK profiles.id, nullable |
| due_date | date | nullable |
| position | int | ordering within list |
| priority | text | low \| medium \| high \| urgent |
| archived | boolean | |
| created_at | timestamptz | |

### comments
| Column | Type | Notes |
|---|---|---|
| id | uuid | PK |
| task_id | uuid | FK |
| author_id | uuid | FK profiles.id |
| body | text | |
| created_at | timestamptz | |

### attachments
| Column | Type | Notes |
|---|---|---|
| id | uuid | PK |
| task_id | uuid | FK |
| storage_path | text | path in Supabase Storage |
| filename | text | |
| size_bytes | int | |

### labels
| Column | Type | Notes |
|---|---|---|
| id | uuid | PK |
| workspace_id | uuid | FK |
| name | text | |
| color | text | hex |

### task_labels
Join table. Composite PK (task_id, label_id).

## 3. Indexes

- `tasks(list_id, position)`
- `tasks(assignee_id)`
- `lists(board_id, position)`
- `boards(workspace_id)`
- `workspace_members(user_id)`
- `comments(task_id, created_at)`

## 4. Row Level Security policies

General rule: a user can access a row if they are a member of the
owning workspace.

Example — `tasks` read policy:

```sql
create policy "tasks readable by workspace members"
on tasks for select
using (
  exists (
    select 1 from lists l
    join boards b on b.id = l.board_id
    join workspace_members m on m.workspace_id = b.workspace_id
    where l.id = tasks.list_id and m.user_id = auth.uid()
  )
);
```

Write policies additionally check role != 'viewer'.

## 5. Realtime

Enable Postgres changes broadcast on:
- `tasks`
- `lists`
- `comments`

Filter subscriptions by `board_id` or `workspace_id` for efficiency.

## 6. Storage buckets

- `avatars` — public read, owner write
- `attachments` — private, signed URLs only

## 7. Migrations

Tracked in `supabase/migrations/` (SQL files, timestamped).