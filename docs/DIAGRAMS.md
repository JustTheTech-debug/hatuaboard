# Diagrams — HatuaBoard

All diagrams use Mermaid so they stay versioned with the code.
GitHub renders them automatically.

---

## 1. System Architecture

```mermaid
flowchart LR
  U[User] --> A[Angular SPA]
  A --> AUTH[Supabase Auth]
  A --> DB[(Postgres)]
  A --> RT[Supabase Realtime]
  A --> ST[Supabase Storage]
  A --> CDN[Vercel CDN]
  A -.PWA cache.-> SW[Service Worker]
```

---

## 2. Component Tree

```mermaid
flowchart TD
  App[AppComponent] --> AuthLayout[AuthLayout]
  App --> Shell[AppShell]

  AuthLayout --> Login[LoginPage]
  AuthLayout --> Signup[SignupPage]
  AuthLayout --> Reset[ResetPasswordPage]

  Shell --> Sidebar[SidebarComponent]
  Shell --> Topbar[TopbarComponent]
  Shell --> Router[RouterOutlet]

  Router --> BoardList[BoardListPage]
  Router --> Board[BoardViewPage]
  Router --> Dashboard[DashboardPage]
  Router --> Settings[SettingsPage]

  Board --> List[ListColumnComponent]
  List --> Card[TaskCardComponent]
  Board --> Drawer[TaskDrawerComponent]

  Drawer --> Comments[CommentsComponent]
  Drawer --> Attachments[AttachmentsComponent]
  Drawer --> Checklist[ChecklistComponent]
```

---

## 3. User Flow — First Task

```mermaid
flowchart TD
  A[Sign up] --> B[Create workspace]
  B --> C[Create board]
  C --> D[Create list]
  D --> E[Create task]
  E --> F[Assign + due date]
  F --> G[Drag across lists]
  G --> H[Done]
```

---

## 4. State Flow (Signals)

```mermaid
flowchart LR
  UI[Component] -->|calls| SVC[Feature Service]
  SVC -->|reads/writes| API[Supabase Client]
  API -->|returns| SVC
  SVC -->|updates| SIG[Signal Store]
  SIG -->|reacts| UI
  RT[Realtime Channel] -->|push| SVC
```

---

## 5. Drag-and-Drop Realtime Sequence

```mermaid
sequenceDiagram
  participant U1 as User 1
  participant A1 as Angular Client 1
  participant DB as Postgres
  participant RT as Realtime
  participant A2 as Angular Client 2
  participant U2 as User 2

  U1->>A1: Drag task to "Done"
  A1->>DB: UPDATE tasks SET list_id, position
  DB-->>RT: change event
  RT-->>A1: confirm
  RT-->>A2: broadcast
  A2-->>U2: board updates live
```

---

## 6. CI/CD Pipeline

```mermaid
flowchart LR
  Dev[Developer] -->|push| GH[GitHub]
  GH --> CI[GitHub Actions]
  CI --> L[Lint]
  CI --> T[Unit Tests]
  CI --> B[Build]
  CI --> E2E[Playwright]
  E2E --> V[Vercel Preview]
  GH -->|merge to main| Deploy[Deploy to Vercel Prod]
  Deploy --> Supa[Supabase]
```

---

## 7. ERD (short form)

See [DATA-MODEL.md](./DATA-MODEL.md#1-entity-relationship-diagram) for the full ERD.

---

## 8. Wireframes

Wireframes live in `docs/diagrams/wireframes/` as exported images
from Figma or Excalidraw. Naming convention:

- `01-login.png`
- `02-signup.png`
- `03-app-shell.png`
- `04-board-view.png`
- `05-task-drawer.png`
- `06-dashboard.png`
- `07-settings.png`