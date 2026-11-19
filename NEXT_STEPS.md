# Next Steps

1. **60-sec video** — follow `VIDEO_SCRIPT.md`: Save → Recall → Delete → Verify on a phone.
2. **Live demo (Replit)** — import this project into Replit, add `DATABASE_URL` (Neon/Supabase free Postgres) as a Secret if you want persistence (or leave unset for in-memory demo), run `npm install && npx prisma generate && npm run dev`, open the webview URL on your phone.
3. **GitHub** — push this finished, tested project to the public `OmniLens-AI` repo (README + docs + app in one commit). This build was packaged as a zip for exactly that step.
4. **Real persistence** — set `DATABASE_URL`, run `npx prisma db push`, restart; the data layer switches from in-memory to Prisma/Postgres automatically.
5. **Retention jobs** — add a scheduled purge for 30d/90d/1y retentions (delete + verify + audit).
6. **Phone-as-glasses prototype** — a page using the phone camera + on-device face detection to blur faces in the preview, proving live-privacy before any hardware exists.
7. **Glasses hardware** — separate partner track (see Memo §7): discreet glasses, 360 camera, auto face/voice blur, clean-room build, NDA first.
