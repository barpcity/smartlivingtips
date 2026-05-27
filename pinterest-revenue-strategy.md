# Pinterest Revenue Strategy

## What Works

- Pinterest is a search and planning platform, not a social feed. Pinterest Business says users save, plan, shop, and 96% of top searches are unbranded, which favors keyword-led new brands and affiliate pages.
- Seasonal intent starts months before the event. The system should publish early for seasonal searches, not on the peak date.
- Pinterest Predicts and Pinterest Trends should feed keyword expansion because Pinterest says its trend signals have a strong historical hit rate and can drive shopping actions.
- Case-study pattern: the biggest traffic lifts come from search-intent boards, strong image pins, readable text hierarchy, and outbound-click optimization, not from generic aesthetic volume.

Sources:
- https://business.pinterest.com/en-ca/audience/
- https://business.pinterest.com/blog/pinterest-brand-moments-guide-2026/
- https://business.pinterest.com/tr/blog/pinterest-predicts-2026-turn-trends-into-unlimited-possibilities/
- https://caffeineandconquer.com/pinterest-case-study-traffic-growth/
- https://help.pinterest.com/en/business/article/pinterest-analytics

## Monetization Model

Use Pinterest for discovery and intent capture. Monetization should happen through controlled destinations first, not through repetitive direct affiliate links:

1. High-intent Pin -> owned guide page on `smartlivingtips.benai.ai`.
2. Guide page explains the topic, carries affiliate disclosure, and reserves display ad slots.
3. Future affiliate links are added only after the relevant program approves the account.
4. Display ads are added only after AdSense or another ad network approves the site.
5. Analytics sync updates impressions, saves, Pin clicks, outbound clicks, and tracked clicks.
6. Autopilot shifts generation toward niches with saves/clicks/revenue signals.

Priority monetization routes:

- Retail and commerce affiliate programs for tech gadgets, smart home, phone accessories, home, kitchen, beauty, and meal prep.
- Digital courses/templates for business, finance, self-improvement, and creator workflows.
- Amazon/retail affiliate for home, beauty, meal prep, and gadgets, but only where search intent is specific.
- Owned guide pages or email capture for expensive/slow-converting products.

## Compliance Rules

- Pinterest allows affiliate links, but requires clear disclosure, original value, authentic account use, and moderation. Avoid repetitive high-volume affiliate Pins.
- FTC disclosure guidance requires material connections to be clear and conspicuous; do not hide disclosure at the end of a hashtag block.
- Google AdSense requires original, policy-compliant content and site ownership access before approval.
- The live system should never publish `example.com`, `amzn.to/example...`, or placeholder affiliate URLs.
- Direct affiliate Pin descriptions must start with a clear disclosure. Owned guide pages must show the site-level disclosure page.

Sources:
- https://policy.pinterest.com/commercial-and-branded-content-guidelines
- https://help.pinterest.com/en/article/suspicious-links
- https://www.ftc.gov/business-guidance/resources/disclosures-101-social-media-influencers
- https://support.google.com/adsense/answer/9724

## Autopilot Rules

- Winners get extra generation quota.
- Losers stop consuming queue after enough data.
- Track outbound clicks with Pinterest API and tracked clicks with `/r/:pinId`.
- Optimize for outbound clicks and save rate. Impressions alone are not enough.
- Keep publishing steady and human-scale; do not burst-post.
- Generated Pins enter an owner approval queue first. `npm run approve -- --all` or
  `npm run approve -- <id>` is required before scheduling when
  `REQUIRE_PIN_APPROVAL=true`.
- Pins use the guide-first funnel by default (`DIRECT_AFFILIATE_PINS=false`).
  Affiliate links belong on owned guide pages until traffic and compliance are
  stable enough to justify direct affiliate Pins.
- Live Pin links must pass the allowlist in `ALLOWED_DESTINATION_HOSTS`. Keep it to
  owned domains until affiliate programs approve the account, then add each
  approved affiliate host intentionally.

## Standard Access

Trial access is enough to test OAuth and API behavior. For a money system, Standard access is expected because production pins must be visible and write access must work reliably. Upgrade submission should show:

- OAuth connect flow.
- User-visible pin generation and scheduling.
- Pin publishing into Pinterest.
- Analytics/reporting screen or terminal output.
- Privacy/safety controls and rate limits.
