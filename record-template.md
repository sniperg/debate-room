---
title: Redis on a static site
date: 2026-09-13
tags: [decision, debate-room]
verdict: accept-with-changes
rounds: 2
searches: 3
judge: false
---

# Add Redis to cache the CMS on a 300-visitor marketing site

**Motion.** Install Upstash Redis and cache CMS responses across the marketing site — roughly one day of work and about $10 a month — because a teammate said in standup that the site feels slow.

## Proceedings

### X — round 1

- **Change:** Add Upstash Redis and cache CMS API responses across the site.
- **Why now:** ~~A teammate said in standup that the site feels slow and users are complaining.~~ `STRUCK`
- **Cost:** ~1 engineer-day, ~$10/month recurring, plus a cache-invalidation surface to maintain forever.
- **Rollback:** Remove the client and the read/write calls, redeploy.

### Y — round 1

- **Premise:** No profiling data, no analytics, no ticket — and every page is statically generated at build time, so there is no per-request CMS call for Redis to intercept.
- **Weak point:** それってあなたの感想ですよね — the whole case rests on one impression at ~300 visitors/day, with no page named and nothing measured.
- **Do it:** Caches a request path static pages never take: ~0ms of user-facing gain for a day of work.
- **Don't:** Nothing breaks. Pages stay exactly as fast, and a day stays free for finding what is actually slow.
- **Else:** Two hours — Lighthouse against real URLs, and grep for any ISR or `getServerSideProps` path that hits the CMS at runtime.
- **Rules:** Verification must match risk; the failure mode itself is not established.
- **Verdict:** `REJECT` — profile first.

### X — round 2

- **Change:** Withdraw Redis. Spend two hours on a read-only diagnostic instead.
- **Why now:** The premise attack holds — static generation means there is likely nothing at request time to cache.
- **Cost:** 2 hours and $0, against 1 day and $10/month.
- **Rollback:** None needed; nothing is installed or deployed.

### Y — round 2

- **Premise:** Now matches the evidence — establish whether a request-time CMS call exists before building for it.
- **Weak point:** "Three representative URLs" names no selection rule and no pass mark, so the diagnostic ends in three numbers nobody can act on.
- **Else:** Ask the teammate which page felt slow — five minutes, but it yields a second unmeasured impression, the exact flaw already struck. It loses.
- **Verdict:** `ACCEPT WITH CHANGES`

## Evidence ledger

| Claim | Grade | Source |
|---|---|---|
| "The site feels slow, users are complaining" | `STRUCK` | No profile, no analytics, no ticket |
| Every page is statically generated at build time | `CITED` | `next.config.mjs` — no ISR, no `getServerSideProps` |
| Upstash runs about $10/month at this volume | `CITED` | upstash.com/pricing, fetched 2026-09-13 |
| Users are abandoning the site over load time | `UNVERIFIED` | Searched "static site abandonment load time benchmarks" — only generic industry figures, none for a 300/day site; a week of RUM data would settle it |

## The three options

| Option | Case |
|---|---|
| Do it | Redis in front of a CMS the pages never call at request time. One day, $10/month, ~0ms. |
| Don't | Nothing breaks. The complaint stays unmeasured and returns at the next standup. |
| **Do something else** ✔ | Two hours: Lighthouse on the highest-traffic CMS-dependent pages, plus a grep for runtime CMS calls. |

## The call

> [!success] ACCEPT WITH CHANGES — Y, round 2
> Diagnose for two hours, build nothing. Pick the three URLs by traffic and CMS dependence rather than arbitrarily, and fix the pass mark — LCP ≤ 2.5s, TTFB ≤ 600ms — *before* running it, so the two hours end in a verdict instead of three numbers.

> [!note] Carried forward from the losing side
> The complaint is still real and still unmeasured. If the diagnostic passes its own thresholds, the next question is what the teammate actually experienced — not whether to cache it.

---
*X proposes · Y challenges · Judge breaks ties — [debate-room](https://github.com/sniperg/debate-room)*
