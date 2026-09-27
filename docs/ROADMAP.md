# Roadmap — HatuaBoard

Phases are shipped as GitHub milestones. Each phase ends with a tag.

## Phase 0 — Foundations
- Repo, docs, CI
- Angular scaffold, routing, layout shell
- Theme + design tokens
- Supabase project + auth wired

**Tag:** `v0.1.0`

## Phase 1 — Auth & Workspaces
- Signup, login, logout, reset
- Create / list / switch workspaces
- App shell with sidebar
- Demo account seed

**Tag:** `v0.2.0`

## Phase 2 — Boards & Lists
- Create / rename / delete boards
- Create / rename / reorder / delete lists
- Empty + loading states

**Tag:** `v0.3.0`

## Phase 3 — Tasks (CRUD)
- Create, edit, delete tasks
- Assignee, due date, priority, labels
- Task drawer UI

**Tag:** `v0.4.0`

## Phase 4 — Drag & Drop
- CDK drag-and-drop for tasks
- Reorder within list, move between lists
- Persist positions

**Tag:** `v0.5.0`

## Phase 5 — Collaboration
- Comments on tasks
- Supabase Realtime subscriptions
- Live board updates

**Tag:** `v0.6.0`

## Phase 6 — Search, Filter, Dashboard
- Search tasks
- Filter by assignee / label / due date
- Dashboard charts

**Tag:** `v0.7.0`

## Phase 7 — Roles & Permissions
- owner / admin / member / viewer
- Invite members
- RLS enforcement

**Tag:** `v0.8.0`

## Phase 8 — Polish
- Dark mode
- Accessibility pass
- PWA + offline read
- Performance pass

**Tag:** `v0.9.0`

## Phase 9 — Launch
- Deploy to Vercel
- Custom domain
- Demo video + README polish
- Portfolio-ready

**Tag:** `v1.0.0`

---

## Stretch goals (post v1)

- Attachments
- Activity feed
- Presence (who's online)
- Burndown chart
- Keyboard shortcuts
- Offline write queue
- Templates for boards
- Email notifications
- AI: summarize comments, suggest subtasks