# Testing — HatuaBoard

## 1. Strategy

| Layer | Tool | What it covers |
|---|---|---|
| Unit | Vitest (or Jest) | Services, pipes, pure logic |
| Component | Angular TestBed + Vitest | Component behavior, inputs/outputs |
| Integration | TestBed + Supabase local | Feature flows |
| E2E | Playwright | Real user journeys |
| A11y | axe-core in Playwright | Accessibility regressions |
| Visual | Playwright screenshots | Layout drift |

Targets:
- 80%+ coverage on services and core logic
- All P0 user stories covered by E2E
- Zero axe violations on main pages

## 2. Conventions

- Test files next to source: `foo.service.spec.ts`
- E2E in `e2e/` mirroring features
- Use factories/fixtures, not copy-paste data
- Mock Supabase client in unit tests, real instance in E2E

## 3. Commands

```bash
npm run test          # unit + component
npm run test:watch    # watch mode
npm run test:cov      # coverage
npm run e2e           # playwright
npm run e2e:ui        # playwright UI mode
npm run lint
```

## 4. Example unit test

```ts
describe('TaskService', () => {
  it('moves a task to a new list and reindexes', () => {
    // arrange
    // act
    // assert
  });
});
```

## 5. Example E2E test

```ts
test('user creates a task and drags it to Done', async ({ page }) => {
  await page.goto('/');
  await login(page, 'demo@hatuaboard.app', 'demo1234');
  await page.getByRole('button', { name: 'Add task' }).click();
  await page.getByLabel('Title').fill('Write PRD');
  await page.getByRole('button', { name: 'Save' }).click();
  await dragTo(page, 'Write PRD', 'Done');
  await expect(page.getByText('Write PRD')).toBeInViewport();
});
```

## 6. CI

Every PR runs:
1. `npm ci`
2. `npm run lint`
3. `npm run test -- --run`
4. `npm run e2e` (against preview deploy)

Red CI blocks merge.

## 7. Manual QA checklist (pre-release)

- Signup, login, logout
- Create workspace, board, list, task
- Drag task across lists
- Realtime: two browsers, one changes, other updates
- Mobile: 375px width, all flows
- Dark mode
- Offline: load, disconnect, reload
- Lighthouse: Perf > 90, A11y > 95