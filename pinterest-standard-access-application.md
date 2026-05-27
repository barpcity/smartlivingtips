# Pinterest Standard Access Application

## Upgrade Position

Upgrade is required for the production money system. Trial access is enough for OAuth and API testing, but production Pin creation is blocked until Standard Access. Trial write demos must use `https://api-sandbox.pinterest.com/v5`; production publishing uses `https://api.pinterest.com/v5` after approval.

## 2026-05-18 Rejection Findings

Pinterest rejected app `1558961` because the submitted demo did not meet the Standard Access video requirement. The rejection reasons were specific:

- The video did not show how the app prompts a user to connect a Pinterest account.
- The video did not show the complete OAuth authentication/token implementation.
- The video looked like screenshots/static proof pages instead of organic app usage.

The prior asset `demo/pinterest-standard-access-demo.mp4` must not be resubmitted. The replacement video must be recorded as a live walkthrough from the app UI: dashboard -> click Connect Pinterest OAuth -> Pinterest OAuth consent/login redirect -> callback success/token storage page -> `/auth/pinterest/status` authenticated -> live Pinterest API read proof -> Pin/sandbox proof -> owned guide funnel.

## 2026-05-24 Replacement Demo Status

- Current replacement video: `demo/pinterest-standard-access-demo.mp4`.
- Duration/resolution verified with `ffprobe`: 49 seconds, 1600x1000, H.264, 30 fps.
- Frame review verified: app dashboard with visible `Connect Pinterest OAuth`, live system status, generated Pin queue, callback/token page, `/auth/pinterest/status`, `/demo/pinterest-api-proof`, sandbox Pin proof, and public guide funnel.
- Security note: password/2FA entry was paused and not recorded. The video shows the app prompt, callback/token storage, authenticated account, and live API proof without exposing credentials.

## Portal Form Answers

- App name: Smart Living Tips Content Manager.
- Company: Smart Living Tips.
- Website: `https://smartlivingtips.benai.ai/`.
- Privacy policy: `https://smartlivingtips.benai.ai/privacy.html`.
- Purpose: Smart Living Tips Content Manager is an AI-assisted Pinterest content management tool for one owned Pinterest account. It generates original vertical Pins, writes SEO-focused titles/descriptions, assigns Pins to relevant owned boards, places Pins into an owner-approved publishing queue with conservative daily safety limits, tracks outbound clicks, and reports performance using Pinterest analytics. The app is used to manage owned content, not to scrape Pinterest or mass-save third-party Pins.
- Use cases: Pin creation and scheduling; Reporting.
- Audience: Users; Content creators.

## App Description

Smart Living Tips Content Manager is an AI-assisted Pinterest content management tool for one owned Pinterest account. It generates original vertical Pins, writes SEO-focused titles/descriptions, assigns Pins to relevant owned boards, places Pins into an owner-approved publishing queue with daily safety limits, and reports performance using Pinterest analytics.

The app does not scrape Pinterest, does not mass-save third-party content, and does not post user-generated spam. It publishes original content created by the account owner after the owner chooses or schedules the Pins to be published and uses conservative rate limits.

## Resubmission Note

Use this note in the Standard Access resubmission after the replacement video is recorded:

> This resubmission includes a new live walkthrough video recorded from the working app UI. The prior submission was not accepted because the video did not clearly show the Pinterest account connection prompt, the complete OAuth callback/token implementation, or organic app usage. The new video shows the user starting from the Smart Living Tips Content Manager dashboard, clicking "Connect Pinterest OAuth", completing Pinterest's OAuth authorization flow, returning to the registered callback URL, storing the token in the app's encrypted token store, verifying the connected owned Pinterest business account, and calling live Pinterest API endpoints (`GET /v5/user_account` and `GET /v5/boards`). The video also shows the owner-approved Pin queue, safety limits, and the owned public guide funnel with disclosure.

## Requested Scopes

- `user_accounts:read`: verify the connected Pinterest account.
- `boards:read`: list owned boards and resolve board IDs.
- `boards:write`: create missing owned boards when needed.
- `pins:read`: read published Pin metadata and analytics.
- `pins:write`: create original Pins on owned boards.

## Demo Script

1. Confirm the Pinterest Developer Portal redirect URI exactly matches `http://localhost:3999/auth/pinterest/callback`.
2. Start the local server: `PORT=3999 PINTEREST_REDIRECT_URI=http://localhost:3999/auth/pinterest/callback npm run dev`.
3. Open `/demo` and show the dashboard plus the "Standard Access Review Walkthrough" section.
4. Click the visible `Connect Pinterest OAuth` control from the app UI.
5. Complete Pinterest OAuth in the browser and allow the requested scopes.
6. Show the callback success page proving code exchange, encrypted token storage, and connected account verification.
7. Show `/auth/pinterest/status` with `configured: true`, `tokenPresent: true`, `authenticated: true`, and the owned Pinterest business account.
8. Show `/demo/pinterest-api-proof` with live `GET /v5/user_account` and `GET /v5/boards` output.
9. Show `/demo/created-pin-proof` with sandbox Pin metadata if a sandbox write proof has been generated.
10. Show safety controls: owner approval, daily limits, no burst posting, redirect click tracking, and owned guide links.

## Required Video Demo Checklist

Pinterest's Standard Access form requires a video upload before submission. Keep the screen capture short and make the proof visible:

1. App dashboard visibly prompts the user to connect Pinterest.
2. The video captures the actual click on `Connect Pinterest OAuth`.
3. The video captures the Pinterest OAuth authorization page, not only a prepared proof page.
4. The callback page shows token exchange/storage and connected account verification.
5. `/auth/pinterest/status` shows authenticated account state.
6. `/demo/pinterest-api-proof` shows live Pinterest API output.
7. `/demo/created-pin-proof` shows sandbox Pin creation metadata when available.
8. The owned guide funnel loads publicly with disclosure and ad-ready sections.
9. The video shows organic usage; do not submit a slideshow, screenshot sequence, or static proof-only recording.

The portal reCAPTCHA and final submit should be completed manually by the account owner.

Replacement recording command:

- Start the app in one terminal: `PORT=3999 PINTEREST_REDIRECT_URI=http://localhost:3999/auth/pinterest/callback npm run dev`.
- Record the live OAuth walkthrough in another terminal: `npm run demo:video`.
- If the script detects a password field, it pauses recording so credentials are not captured. Finish login/2FA in the opened Chrome window, then press Enter in the terminal to continue.
- Use `DEMO_SKIP_OAUTH=true npm run demo:video` only for internal preview. That output is not acceptable for Pinterest submission because it does not capture the full OAuth flow.

Before final submission, register the exact redirect URI in Pinterest Developer Portal and confirm `/auth/pinterest/status` returns `authenticated: true`. A video that shows `authenticated: false`, a Pinterest `400` redirect URI error, or a production Pin create attempt under Trial access is not final submission material.

## Safety And Compliance Notes

- Pins are original generated assets, not saved third-party content.
- Publishing is capped by dynamic daily limits and board-level anti-spam rules.
- Production scheduling is owner-approved: generated Pins must have `approved_at`
  before `schedule`, `publish`, workers, or exports can move them forward.
- Links are tracked through `/r/:pinId` only when `PUBLIC_BASE_URL` is configured.
- The redirect route only redirects to DB-stored, allowlisted public URLs and rejects
  placeholders, invalid protocols, and local/private destinations.
- OAuth tokens are encrypted at rest.
- Privacy page: `https://smartlivingtips.benai.ai/privacy.html`.
