# RSI Sniper — Fresh Dhan Backend

This ZIP is arranged so `server.js`, `package.json`, `.env.example`, `render.yaml`, and this README are all at the repository root. Do not upload an extra enclosing folder.

## Deploy on Render
1. Extract this ZIP on your device.
2. Upload **all five files** to the root of a new GitHub repository. Confirm `server.js` is visible beside `package.json` in the GitHub file list.
3. Create a Render Web Service from that repository.
4. Set **Root Directory** to blank (unless you intentionally put all files inside a subfolder).
5. Build command: `npm install`
6. Start command: `npm start`
7. Health check path: `/api/health`
8. Add the environment variables below in Render. Never commit real credentials to GitHub.

## Required Render environment variables
- `DHAN_CLIENT_ID` — your Dhan client ID
- `DHAN_PIN` — your Dhan PIN
- `DHAN_TOTP_SECRET` — TOTP secret from Dhan; do not share it in chat or commit it

## Recommended initial settings
Copy values from `.env.example` into Render Environment, then replace placeholders with your own values. `CORS_ORIGIN=*` is for initial connection testing only; once the PWA URL is stable, restrict it to that exact Netlify origin.

## Verify in this order
1. `GET /api/health` — should return `{"ok":true,...}`.
2. `GET /api/status` — inspect `dhanConfigured`, `dhanState`, `lastError`, `lastDhanMessageAt`, `lastTickAt`, `lastTickAgeMs`, and `stale`.
3. `GET /api/spot?symbol=NIFTY` and `GET /api/spot?symbol=SENSEX` — should return actual quotes, not zero placeholders.
4. `GET /api/candles?symbol=NIFTY&timeframe=1m&limit=50` — verify real OHLC rows.
5. `GET /api/option-chain?symbol=SENSEX` — verify active expiry and actual contract values.

A healthy HTTP service does not by itself prove the Dhan feed is receiving fresh instrument ticks. Confirm the status fields and actual responses. Do not share screenshots containing secrets.

## Endpoints
`/api/health`, `/api/status`, `/api/state`, `/api/config`, `/api/spot?symbol=NIFTY`, `/api/ticks`, `/api/candles?symbol=NIFTY&timeframe=1m&limit=250`, `/api/history?symbol=NIFTY&tf=1m&limit=250`, `/api/option-chain?symbol=SENSEX`, `/api/option-chain/expiries?symbol=SENSEX`, `/api/quote?symbol=NIFTY`, `/ws`.

## Important
This backend has passed a local JavaScript syntax check. It has not been authenticated against your Dhan account or end-to-end tested against live market data in Render. Do not treat deployment success alone as proof of live-feed correctness.
