# Self-hosted fork (Vercel-ready)

This fork is configured to run as a **fully static app** and to never call an
Excalidraw-owned server. It builds to `excalidraw-app/build` and can be
deployed to Vercel with zero backend.

## What is disabled by default

Everything below is driven by `.env.production`; leave a value empty to keep
the feature off, or fill it with your own self-hosted backend to enable it.

| Feature | Variable | Default |
| --- | --- | --- |
| Read-only share links (`#json=…`) | `VITE_APP_BACKEND_V2_GET_URL` / `VITE_APP_BACKEND_V2_POST_URL` | disabled |
| Real-time collaboration | `VITE_APP_WS_SERVER_URL` | disabled |
| Collab/share image storage | `VITE_APP_FIREBASE_CONFIG` | disabled (`{}`) |
| Community library store / publish | `VITE_APP_LIBRARY_URL` / `VITE_APP_LIBRARY_BACKEND` | disabled |
| Excalidraw+ promos & sign in | `VITE_APP_PLUS_LP` / `VITE_APP_PLUS_APP` | hidden |
| AI text-to-diagram | `VITE_APP_AI_BACKEND` | disabled |
| Sentry | `VITE_APP_DISABLE_SENTRY` | `true` |
| Simple Analytics | removed from `index.html` | — |

Fonts are served from the deployment itself (`/fonts/...`), not from the
Excalidraw CDN, and there is no Excalidraw+ auto-redirect.

Local features keep working: local storage persistence, `.excalidraw` files,
export to PNG/SVG, local (IndexedDB) libraries, offline/PWA.

## Deploy on Vercel

1. Push this repo to your fork.
2. Import the repo on Vercel. `vercel.json` already sets:
   - install command: `yarn install`
   - build command: `yarn build:app`
   - output directory: `excalidraw-app/build`
3. Deploy. No environment variables are required.

Or from the CLI:

```bash
vercel          # preview deployment
vercel --prod   # production deployment
```

## Re-enabling share links / collaboration later

Both features need a backend outside of Vercel static hosting:

- Collaboration: [excalidraw-room](https://github.com/excalidraw/excalidraw-room)
  (WebSocket server, e.g. on Render/Fly/Railway) → `VITE_APP_WS_SERVER_URL`.
- Share links: the JSON backend + Firebase project used by the app →
  `VITE_APP_BACKEND_V2_*` and `VITE_APP_FIREBASE_CONFIG`.

Then set the variables in the Vercel project settings and redeploy.
