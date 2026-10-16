# Eventra Phase 1 — Connection Fix

This revision prevents unnecessary Socket.IO disconnect/reconnect cycles when changing sites or notification preferences, shows the real connection state, and refreshes events after reconnection.

## Deploy
1. Replace your frontend project files with the `frontend` directory and redeploy Netlify (rebuild with `npm install && npm run build`).
2. You do not need to reset MongoDB or recreate your websites.
3. Confirm Netlify `VITE_API_URL=https://eventra-x8ad.onrender.com` (no trailing slash required).
4. Confirm Render `DASHBOARD_ORIGIN` exactly matches your dashboard's origin, e.g. `https://eventra-webevents.netlify.app` (not your test website). If you have multiple dashboard origins, comma-separate them.
5. Confirm your registered test website origin matches its actual `window.location.origin`, including `www` differences.

## Check events
Open the test website DevTools > Network and filter for `collect`. Click a button. `POST /api/collect` should return **202**. If it returns 403, your registered website origin does not match. If it returns 404, your Site ID does not exist on this backend. If it returns 429, rate limiting is active. If no request appears, check whether the SDK loaded and `window.Eventra` exists.

The dashboard Socket.IO connection is independent from HTTP event ingestion; a WebSocket upgrade warning alone does not prove that events failed to save.
