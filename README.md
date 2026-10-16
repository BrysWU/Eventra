# Eventra — Phase 1

Real-time website events, multi-site dashboard, account login, MongoDB history, and browser notifications.

## Backend (Render Web Service)
1. Push the project to GitHub and create a Node Web Service with root directory `backend`.
2. Build command: `npm install`. Start command: `npm start`.
3. Add environment variables from `backend/.env.example`: `MONGODB_URI`, `JWT_SECRET` (generate a long random secret), `DB_NAME=eventra`, `DASHBOARD_ORIGIN=https://YOUR-FRONTEND.netlify.app`.
4. Configure MongoDB Atlas network access and database credentials. Test `https://YOUR-BACKEND.onrender.com/health`.

## Frontend (Netlify)
1. Create a Netlify site with base directory `frontend`.
2. Build command: `npm run build`; publish directory: `dist` (relative to base).
3. Set `VITE_API_URL=https://YOUR-BACKEND.onrender.com` as a Netlify environment variable, then deploy.
4. Make sure `DASHBOARD_ORIGIN` on Render matches your exact Netlify site origin (no trailing slash).
5. Register an account, add a website with its exact origin, and copy the embed snippet.

## Local development
- Backend: `cd backend && npm install && cp .env.example .env` (edit secrets), then `npm start`.
- Frontend: `cd frontend && npm install && npm run dev`.
- Frontend defaults to `http://localhost:3000` backend when `VITE_API_URL` is unset.

## JavaScript SDK
Embed the generated script. The script is served from the frontend site root as `/eventra.js`. Automatic site visits are sent once per browser tab session per 30 minutes. Trigger a custom event with `Eventra.track('download_clicked',{file:'installer.exe'})` or annotate a button with `data-eventra-event="signup_click"`.

## Important limitations / next steps
- This is Phase 1, not yet the Windows tray application or email delivery service.
- Browser notifications require HTTPS or localhost and user permission; currently they appear when the dashboard tab is hidden and connected.
- Event ingestion is public by design. Origin checking and rate limiting are abuse deterrents, not proof that events are genuine; clients can forge requests. Add per-site quotas, bot protection, account verification, and operational monitoring before a public launch.
- Only register sites you control. Do not include personal data in event properties. Implement consent/privacy controls appropriate to your visitors.
- History expires after 30 days; each site view loads its 100 newest events. Counters are counts in that loaded feed, not full-day analytics.
- Auth tokens are stored in localStorage for this MVP; production should use a hardened session architecture, CSRF controls if cookies are used, and stronger abuse prevention.
- Site deletion removes stored events but cannot retract already delivered notifications.
