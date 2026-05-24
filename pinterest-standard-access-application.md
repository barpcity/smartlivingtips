# Pinterest Standard Access Application

## Upgrade Position

Upgrade is required for the production money system. Trial access is enough for OAuth and API testing, but production Pin creation is blocked until Standard Access. Trial write demos must use `https://api-sandbox.pinterest.com/v5`; production publishing uses `https://api.pinterest.com/v5` after approval.

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

## Requested Scopes

- `user_accounts:read`: verify the connected Pinterest account.
- `boards:read`: list owned boards and resolve board IDs.
- `boards:write`: create missing owned boards when needed.
- `pins:read`: read published Pin metadata and analytics.
- `pins:write`: create original Pins on owned boards.

## Demo Script

1. Show the app settings page with redirect URI `http://localhost:3999/auth/pinterest/callback`.
2. Start the local server: `PORT=3999 PINTEREST_REDIRECT_URI=http://localhost:3999/auth/pinterest/callback npm run dev`.
3. Open `/auth/pinterest` and complete OAuth.
4. Show `/demo/oauth-proof` returning authenticated with the owned Pinterest business account.
5. Run `DRY_RUN=false PINTEREST_API_BASE=https://api-sandbox.pinterest.com/v5 npm run demo:publish-pin` to create and verify a sandbox Pin proof.
6. Show `/demo/created-pin-proof` with the created Pin metadata.
7. Run `npm run sync-analytics` and `npm run report`.
8. Show safety controls: daily limit, no burst posting, affiliate trust period, redirect click tracking.

## Required Video Demo Checklist

Pinterest's Standard Access form requires a video upload before submission. Keep the screen capture short and make the proof visible:

1. App dashboard showing approval-gated publishing, owned guide links, and safety controls.
2. Browser showing `http://localhost:3999/demo/oauth-proof` with `authenticated: true`.
3. Browser showing `http://localhost:3999/demo/oauth-start-proof` with requested OAuth scopes and signed state.
4. Browser showing `http://localhost:3999/demo/pinterest-api-proof` with production read API proof.
5. Browser showing `/demo/created-pin-proof` with sandbox Pin creation metadata.
6. Browser showing the live owned guide funnel with disclosure and ad-ready sections.

The portal reCAPTCHA and final submit should be completed manually by the account owner.

Current prepared demo asset:

- `demo/pinterest-standard-access-demo.mp4`
- Duration: about 59 seconds.
- Purpose: product walkthrough covering the approval-gated dashboard, OAuth connection proof, OAuth start scopes, production read API proof, sandbox Pin creation proof, owned guide funnel, disclosure, safety limits, and analytics readiness.

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
