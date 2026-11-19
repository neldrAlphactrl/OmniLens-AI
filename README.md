# OmniLens AI

Privacy-first AI companion with user-owned memory, explicit consent, retention controls, and verifiable deletion.

Demo flow (phone): **Save → Recall → Delete → Verify**

## Vision
An always-with-you companion that sees what you see and helps you build anything. Long-term: discreet everyday smart glasses — 360 camera, live privacy by default (bystander faces and other people's voices auto-blurred in real time, the wearer's voice clear, no raw unblurred stream ever leaves the glasses), prescription compatible.

## Principles
- User-owned memory — view, export, delete all of your data
- Explicit consent — no memory saved without consent = "User Approved"
- Retention controls — Permanent, 30d, 90d, 1y, auto-purge on expiry
- Verified deletion — deletion is not complete until verified
- End-to-end encryption — data encrypted at rest and in transit (production goal)
- Transparent auditing — every create/delete is logged

## Stack
Next.js (App Router) + TypeScript, PostgreSQL + Prisma (`Memory`, `AuditLog`).

**Stack notes:** (1) Data layer (`src/lib/store.ts`): Prisma/Postgres is used when `DATABASE_URL` is set; when it is not set, an in-memory store is used so the demo and acceptance tests run anywhere with no database/credentials. (2) Styling: mobile-first plain CSS in `src/app/globals.css` instead of Tailwind, to keep the install light in a phone-only workflow — component logic is unchanged and Tailwind can be added later.

## Run it
```bash
npm install
cp .env.example .env   # fill in DATABASE_URL for a real Postgres (Neon/Supabase), or leave unset for the in-memory demo
npx prisma generate
npx prisma db push     # only when DATABASE_URL is set
npm run dev            # http://localhost:3000
```

## API (base /api/v1)
- `POST /memories` `{text, category, retention, consent}` — 201, or 400 without consent / empty text
- `GET /memories?q=` — Recall/search
- `DELETE /memories/:id` — returns `deletionId` (del_xxxxxx) + 5 verification steps
- `GET /deletion/:id/verify` — `{status: "Passed" | "Pending"}`
- `GET /audit` — audit log, newest first

Full contract: `docs/API_CONTRACT.md` · Trust model: `docs/TRUST_ENGINE.md` · Next: `docs/NEXT_STEPS.md` · Video: `VIDEO_SCRIPT.md`
