# Technical & Crawler-View Evidence — Housebets

Collected 2026-11-13. Methods: `browser.open` (search-engine crawler fetch), `curl` from a datacenter IP, `browser.search`. Every item states its source and what was observed.

## 1. What a crawler fetch returns (browser.open)

| Page | Title (as returned) | Real content? | Notes |
|---|---|---|---|
| https://www.housebets.com/ | "Best Crypto Casino & Sportsbook \| Housebets" | Yes, but thin extraction (~32 lines) | H2 "$5M Wave 2 is live."; login options Google / X / Telegram / MetaMask; Casino + Sports links; "House bankroll · on-chain $2,850,275" with a link out to arkm.com. No geo-block or challenge text in the crawler view. Crawled 2026-11-12 19:35 UTC. |
| https://www.housebets.com/promotions | "Promotions \| Housebets" | Cards only | Duel Mode, "Win a $324,000 watch", "$150,000 in leaderboard prizes", Suggest a Feature, Ambassador Program. No explanatory body text about bonus terms. |
| https://www.housebets.com/contests | "Housebets Contest Labs \| Housebets" | Yes, full text (~75 lines) | H1 "Contest Labs". Includes the 8 Nov 2026 update: new part 5 "AI search", scoring (Audit 20 / Keywords 15 / Brand 20 / Pages 15 / AI search 15 / 90 days 15), updated plans accepted until 22 November 2026, and the audit questions on crawler view, trust signals and satellite sites. |
| https://www.housebets.win/ | "Best Crypto Casino & Sportsbook \| Housebets" (identical title to .com) | Yes, much fuller extraction (~132 lines) | H2 "Own Your Edge"; rewards slider copy ("Choose between rakeback and lossback"); coin list incl. TON; the hero block appears twice in the extraction. Same brand/site as .com — near-duplicate homepage. |

## 2. Direct HTTP behaviour (curl, datacenter IP, 2026-11-13)

- `GET https://www.housebets.com/` → **HTTP 403, 5,375 bytes**: Cloudflare "Just a moment…" challenge page; the challenge page itself carries `<meta name="robots" content="noindex,nofollow">`.
- Same request with a **Googlebot user-agent string** → **HTTP 403, 3,272 bytes**, same challenge ("Enable JavaScript and cookies to continue").
  - Caveat: a spoofed UA from a datacenter IP is **not** verified Googlebot (real Googlebot is normally verified via reverse-DNS/ASN allowlisting), so this does **not** prove Googlebot is blocked. It proves the site hard-gates unverified bots, and that the geo/bot behaviour the contest brief asks about cannot be validated from an unverified IP.
- `GET https://www.housebets.win/` → **HTTP 200, 506,216 bytes** of full server-rendered HTML. The .win domain is far more open to crawlers than .com from the same IP, minutes apart.

## 3. The canonical problem: housebets.dev

From the raw HTML of https://www.housebets.win/ (curl, 2026-11-13):

- `<link rel="canonical" href="https://housebets.dev">`
- All `hreflang` alternates (en, ar, es, hu, fr, ru, pt, id, …) point to `https://housebets.dev/...` URLs.
- `og:url` = `https://housebets.dev`.
- Meta description present: "Join Housebets for crypto slots and sports betting. Provably fair games, instant withdrawals, and 24/7 support."

But `https://housebets.dev/` itself **redirects to a Cloudflare Access login** (`housebets.cloudflareaccess.com/cdn-cgi/access/login/housebets.dev…`, page title "Sign in ・ Cloudflare Access"). As observed on 2026-11-13, the domain that .win declares as canonical is a login-gated (presumed staging/dev) domain, not a public site.

## 4. robots.txt

- https://www.housebets.com/robots.txt → **HTTP 200, 12,602 bytes, 524 lines, 521 Disallow rules**, single `User-Agent: *` group.
  - Disallowed pattern (repeated per locale): `/studio`, `/login`, `/register`, `/verify`, `/settings`, `/transactions`, `/notifications`, `/r/`, `/partner`, `/ip-inquiry`, `/maintenance`, `/preview-expired`, `/config`, `/financial-records`, `/operations`, `/players`, `/reports` — i.e. app, account and admin-style paths.
  - **19 locale prefixes** carry the same rule set: ar, da, de, es, fi, fr, hi, hu, id, it, ja, ko, pl, pt, ru, th, tr, vi, zh (plus the unprefixed tree).
  - Final line: `Sitemap: https://www.housebets.com/sitemap.xml`.
- https://www.housebets.win/robots.txt → HTTP 200, 12,598 bytes. Diff vs .com: **identical except the Sitemap line**, which reads `Sitemap: https://housebets.dev/sitemap.xml`.

## 5. Sitemaps

- https://www.housebets.com/sitemap.xml → **HTTP 403** (Cloudflare challenge) via curl with both a default and a browser user-agent; `sitemap_index.xml` and `sitemap-index.xml` on .com also return 403. The sitemap the .com robots.txt declares could not be fetched from this IP at all.
- https://www.housebets.win/sitemap.xml → **HTTP 200, 826 bytes**: a sitemap index whose **8 child sitemaps are all on housebets.dev** — `sitemap-static.xml`, `sitemap-catalog-games-0.xml`, `sitemap-catalog-games-1.xml`, `sitemap-catalog-providers.xml`, `sitemap-sanity-blog.xml`, `sitemap-sanity-promotions.xml`, `sitemap-sanity-legal.xml`, `sitemap-sanity-about.xml`. So the only publicly fetchable sitemap index points crawlers at the login-gated .dev domain.
- Exact sitemap URL counts and lastmod data: **not verifiable from public data** in this pass (.com gated, .dev login-walled).

## 6. Trust signals findable publicly

- **Licence (consistent across independent sources)**: Curaçao Gaming Control Board / Gaming Authority licence **OGL/2024/431/0231**, operator **Bridge Technologies B.V.**, Curaçao Chamber of Commerce **160264(0)**, Dr. M.J. Hugenholtzweg 25, Willemstad; payments partly via Bridge Technologies (Cyprus) Limited (HE 440867).
  - Sources: casino.clinic/casinos/housebets (states they opened the CGA register entry: status "Granted and Active", housebets.com on the approved-domains list, 13 domains on the licence); realcasinosbonus.com/casinos/housebets (register re-checked 1 Nov 2026); house-bets.net/en; cryptocasinoreviews.io/housebets; cryptotips.com/housebets.
- **Regulatory Scope page**: Housebets publishes one (per casino.clinic and housebetcasino.com) stating the licence does **not** cover the planned $HBTS token, airdrops, on-chain rewards or the affiliate programme. Dispute resolution: Resolvo (Curaçao).
- **Responsible gambling**: WagerDB's review scores Housebets' responsible-gambling pillar 8.6/10 (wagerdb.com/casinos/housebets); AskGamblers' sportsbook review has a dedicated responsible-gambling section. The exact on-site RG / about / company page URLs on housebets.com were **not verifiable from public data** in this pass (footer links not captured; .com is bot-gated to direct fetches).
- **Conflicting trust scores for the same brand**: Casino Guru Safety Index **7.7** (via crash-games.net), WagerDB **62.5/100** with security scored 3.0 (no 2FA), Scam Detector **80.5/100** (via brand SERP evidence, brand_serp.md).
