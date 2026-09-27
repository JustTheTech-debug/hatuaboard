
---

## 2. `docs/PRD.md`

```markdown
# Product Requirements Document — HatuaBoard

## 1. Vision

HatuaBoard is a collaborative project management app that helps small teams
plan, track, and complete projects together in real time. It combines the
simplicity of a Kanban board with the warmth of a shared workspace.

**Tagline:** Every step counts. Hatua kwa hatua, pamoja.

---

## 2. Problem

Small teams (startups, student groups, chamas, freelancers) often juggle:
- WhatsApp for discussion
- Google Sheets for task lists
- Email for updates
- Sticky notes for reminders

There is no single, lightweight, collaborative place where everyone can see
who is doing what, by when, and how far the project has come.

---

## 3. Target users

| Persona | Need |
|---|---|
| **Team lead / founder** | See progress at a glance, assign work, spot blockers |
| **Team member** | Know what to do today, update status, comment |
| **Freelancer / consultant** | Manage multiple client projects cleanly |
| **Student group** | Coordinate assignments and deadlines |

---

## 4. Goals

- Give teams one shared place for planning and tracking work.
- Make task updates feel instant (real-time).
- Be usable on a phone in low-bandwidth conditions (PWA + offline).
- Look and feel like a real product, not a tutorial.

## 5. Non-goals (v1)

- Time tracking / billing
- Gantt charts
- Native mobile apps
- Enterprise SSO / SCIM
- Video calls

---

## 6. Core user stories

### Auth
- As a user, I can sign up with email or Google.
- As a user, I can log in, log out, and reset my password.

### Workspaces
- As a user, I can create a workspace.
- As a user, I can invite members by email.
- As a user, I can switch between workspaces.

### Boards
- As a member, I can create boards inside a workspace.
- As a member, I can rename, archive, or delete a board.

### Lists & tasks
- As a member, I can create lists (columns) on a board.
- As a member, I can create tasks inside a list.
- As a member, I can drag a task between lists and reorder it.
- As a member, I can open a task and edit: title, description,
  assignee, due date, labels, checklist.
- As a member, I can comment on a task.
- As a member, I can attach files to a task.

### Collaboration
- As a member, I see changes from others in real time.
- As a member, I see who is online in the workspace.

### Search & filter
- As a member, I can search tasks by title or description.
- As a member, I can filter by assignee, label, or due date.

### Dashboard
- As a member, I can see: tasks completed, tasks overdue,
  workload per member, and a burndown chart.

### Roles
- As an owner, I can promote/demote members and remove them.
- As a viewer, I can read but not edit.

---

## 7. Success metrics (for portfolio)

- App loads in < 2s on a mid-range phone.
- Lighthouse: Performance > 90, Accessibility > 95.
- All core flows covered by tests.
- 100% of core features work offline (PWA).

---

## 8. Constraints

- Must be built in Angular.
- Must be deployable for free (Vercel/Netlify + Supabase free tier).
- Must be demo-able without requiring a signup (demo account).
- Must be responsive from 320px up.

---

## 9. Risks

| Risk | Mitigation |
|---|---|
| Scope creep | Strict v1 feature list, phased roadmap |
| Free tier limits | Cache aggressively, design for low writes |
| Real-time complexity | Use Supabase Realtime, not custom sockets |
| Offline sync conflicts | Start with read-only offline cache |

---

## 10. Release plan

See [ROADMAP.md](./ROADMAP.md).