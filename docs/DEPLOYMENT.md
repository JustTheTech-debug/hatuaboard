# Deployment — HatuaBoard

## 1. Overview

| Piece | Host |
|---|---|
| Angular SPA | Vercel (or Netlify) |
| Auth + DB + Storage | Supabase |
| CI | GitHub Actions |

## 2. Environments

| Env | Branch | URL |
|---|---|---|
| dev | local | http://localhost:4200 |
| preview | any PR | Vercel preview URL |
| prod | `main` | https://hatuaboard.app |

## 3. Environment variables

`src/environments/environment.ts` (dev) and `environment.prod.ts`:

```ts
export const environment = {
  production: true,
  supabaseUrl: 'https://xxxx.supabase.co',
  supabaseAnonKey: 'public-anon-key'
};
```

Only the **anon** key goes in the frontend. Service role key stays
server-side (never in the SPA).

For Vercel, set them as project env vars and inject at build time
using Angular's `--configuration production` and a small build script,
or commit them (safe for anon key) — pick one and document it.

## 4. Build

```bash
npm ci
npm run build -- --configuration production
```

Output: `dist/hatuaboard/browser`

## 5. Deploy to Vercel

1. Import the GitHub repo into Vercel.
2. Framework preset: **Angular** (or Other).
3. Build command: `npm run build`
4. Output directory: `dist/hatuaboard/browser`
5. Add env vars.
6. Deploy.

SPA routing: add a `vercel.json`:

```json
{
  "rewrites": [{ "source": "/(.*)", "destination": "/index.html" }]
}
```

## 6. Supabase setup

1. Create a project.
2. Run migrations in `supabase/migrations/`.
3. Enable RLS on all tables.
4. Configure Auth providers (email + Google).
5. Set redirect URLs to your Vercel domains.
6. Create storage buckets `avatars` and `attachments`.

## 7. CI/CD (GitHub Actions)

`.github/workflows/ci.yml`:
- On PR: lint, unit tests, build, Playwright against preview.

`.github/workflows/deploy.yml`:
- On push to `main`: build and deploy to Vercel.

## 8. Rollback

Vercel keeps every deploy. Roll back from the dashboard in one click.
Supabase migrations are versioned; write down-only migrations for
risky changes.

## 9. Monitoring

- Vercel Analytics (Web Vitals)
- Supabase logs
- Optional: Sentry for errors

## 10. Launch checklist

- [ ] Custom domain configured (hatuaboard.app)
- [ ] HTTPS forced
- [ ] Demo account seeded
- [ ] README screenshots + demo video
- [ ] Lighthouse > 90 perf, > 95 a11y
- [ ] All P0 flows tested in prod