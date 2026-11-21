# Re-verification log — 21 Nov 2026 (v2.1 patch round)

All checks run 21 Nov 2026, ~01:17–01:25 WIB, plain fetches from this machine
(same method as the 13 Nov audit). Purpose: confirm the v1/v2 claims still hold
at submission time, and ground the v2.1 additions.

## 1. F1 / F2 — STILL LIVE, unchanged

- `GET https://www.housebets.win/` → 524,923 bytes of server-rendered HTML.
  - `<link rel="canonical" href="https://housebets.dev"/>`
  - `og:url` = `https://housebets.dev`
- `GET https://housebets.win/sitemap.xml` → index of the same 8 children, all on
  housebets.dev: sitemap-static, sitemap-catalog-games-0/1,
  sitemap-catalog-providers, sitemap-sanity-blog, sitemap-sanity-promotions,
  sitemap-sanity-legal, sitemap-sanity-about.
- `GET https://housebets.dev/` → HTTP 302 → Cloudflare Access login
  (host `litzepe.cloudflareaccess.com/cdn-cgi/access/login/housebets.dev`).
- `GET https://housebets.win/robots.txt` → **524 lines / 12,598 bytes**;
  its Sitemap line still reads `Sitemap: https://housebets.dev/sitemap.xml`.
  (Settles the line-count dispute: this entry's 524, not the 1,047 two rival
  entries print. `housebets.com/robots.txt` itself now answers a Cloudflare
  301 from this network.)
- hreflang: the string `hreflang` does not appear in today's homepage HTML
  or inner pages (/casino, /rewards, /sports). The 13 Nov alternates (all
  pointing at .dev) are absent rather than repaired — the international
  layer currently has no working hreflang at all.

## 2. Blog silence — CONFIRMED

- `GET https://housebets.win/blog` (440,851 bytes) lists article dates:
  newest **Sep 3, 2026** (×2), then Sep 2, Sep 1, Aug 27, Aug 26, …
  **Zero articles dated October or November 2026.** The blog has been silent
  for 11 weeks, spanning the whole contest window. (Independent re-check of
  the same fact a rival entry reports; their date agrees with this fetch.)

## 3. False closure claim — CONFIRMED LIVE

- gamblineers.com/housebets-casino-review/ states: the casino "permanently
  closed on October 7, 2026" and that all links to Housebets from that site
  have been disabled (page crawled 20 Nov 2026; claim re-checked 21 Nov 2026).
- False on its face: the operator's own Contest Labs programme is live, and
  casino.clinic completed a $5,500 test withdrawal on 29 Oct 2026 — three
  weeks after the claimed closure. Added to report §3.3 and brand-serp.csv.

## 4. Locales vs the operator's restricted list — NEW FINDING (F6)

Sources: starter-pack `languages.csv` (20 languages) × the restricted-
jurisdictions list in the operator's own terms, as reproduced in full by
casino.clinic (page printed 20 Nov 2026).

Restricted list includes: Denmark, Germany, Austria, France, Italy, Poland,
Portugal, Brazil, Spain, Mexico, Argentina, Colombia, Peru, United Kingdom,
United States, Netherlands, Belgium, Australia, Canada (Ontario), …

Locale mapping against it:

| Locale | Primary market(s) | Restricted? |
|---|---|---|
| /da | Denmark | yes |
| /de | Germany, Austria | yes, both |
| /fr | France | yes |
| /it | Italy | yes |
| /pl | Poland | yes |
| /pt | Portugal, Brazil | yes, both |
| /es | Spain + LatAm | Spain, Mexico, Argentina, Colombia, Peru all yes |

Six of the 19 locale prefixes exist primarily for markets the operator
itself restricts; the UK is likewise restricted (relevant to §1.2/§2.1:
en-GB is a takedown/brand-defence surface, not a traffic market).

## 5. AI-search live retries — STILL BLOCKED (see ai_live_2026-11-21/PROBE.md)

- Gemini (gemini-3.1-flash-lite), Q2 probe, 21 Nov 2026: API answered
  429 RESOURCE_EXHAUSTED; the CLI retried silently and was stopped at 150 s
  with zero output. Quota still exhausted.
- Xingchi gpt-6.1-sol via OpenCode, 21 Nov 2026: no output within 100 s
  (after the 18 Nov gpt-6-astra silence at 150 s). Relay still unresponsive.
- Q2–Q8 therefore remain NOT OBTAINED, exactly as labelled in ai-live.csv.
