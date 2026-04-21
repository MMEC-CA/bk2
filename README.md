# bk2
Owned by mmec-ca.

Cloudflare Worker (static assets only) served at `erd.mmec.ca/bk2/`.

## Deploy

Pushes to `main` auto-deploy via GitHub Actions (`.github/workflows/deploy.yml`).
Required repo secrets: `CLOUDFLARE_API_TOKEN`, `CLOUDFLARE_ACCOUNT_ID`.

Manual deploy:
```
npx wrangler@4 deploy
```

## Adding Durable Object / WebSocket support

This repo currently has no Worker script (no `main` in `wrangler.jsonc`). To add
real-time multiplayer capability (WebSocket signaling, in-memory or persistent
server state), mirror the pattern used in sibling repos like `bk3`:

1. Add `src/worker.js` and a DO class file under `src/`.
2. Update `wrangler.jsonc`: set `main: "./src/worker.js"`, add
   `assets.binding: "ASSETS"`, declare `durable_objects.bindings`, and add a
   `migrations` block (use `new_sqlite_classes` for new classes).
3. Push — GitHub Actions redeploys automatically.

The Cloudflare account and the token already support Durable Objects, KV, and
WebSockets at the plan level.
