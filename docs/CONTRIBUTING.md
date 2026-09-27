# Contributing — HatuaBoard

## 1. Branching

- `main` — always deployable
- `feat/<scope>` — new feature
- `fix/<scope>` — bug fix
- `chore/<scope>` — tooling, docs
- `docs/<scope>` — docs only

## 2. Commits

Conventional Commits:

```
feat(board): add drag and drop between lists
fix(auth): handle expired session on refresh
docs(readme): add live demo link
chore(ci): cache npm in GitHub Actions
```

## 3. Pull requests

- One feature per PR
- Link the issue
- Include screenshots/GIF for UI changes
- Green CI required
- Squash merge to `main`

## 4. Code style

- TypeScript strict mode on
- Prettier + ESLint enforced via pre-commit hook
- Components: standalone, OnPush, signals
- No `any` without a comment explaining why
- Prefer small files (< 300 lines) and pure functions

## 5. Naming

- Components: `hb-feature-name`
- Services: `FeatureNameService`
- Files: `kebab-case.ts`
- Signals: `noun` for value, `setNoun` for setter, `noun$` for Observable

## 6. Angular conventions

- `inject()` instead of constructor injection
- `input()` / `output()` signals
- `@if`, `@for`, `@switch` control flow
- Lazy load every feature route
- Route guards for auth + roles

## 7. Testing

- Add tests with every PR that changes logic
- Keep coverage ≥ 80% for services
- E2E for every new user flow

## 8. Local setup

```bash
git clone <repo>
cd hatuaboard
npm install
cp .env.example .env   # fill Supabase keys
ng serve
```

## 9. Getting help

Open a discussion or ping in the project chat. Be kind.