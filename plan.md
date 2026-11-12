# Win Google for Housebets — an SEO growth plan for housebets.com

**Entry for the Housebets Contest Labs contest "Win Google for Housebets".**
Audit date: 13 November 2026. Method: public data only — live fetches of housebets.com, housebets.win and housebets.dev, their robots.txt and sitemap files, and search-result sampling (Google, English) for 10 brand queries, 6 category queries and 4 AI-style questions. No login, no paid tools, nothing the operator has not already published; every finding below can be re-checked from the URLs and files cited.

**Honest framing:** this is a plan, not completed client work. The targets in §6 are goals with the reasoning shown, not results already achieved. Where public data could not settle a question, the plan says so and names the check that would.

**Companion files:** `keywords.csv` (43 keywords mapped to pages), `ai-answers.csv` (AI-search samples in the required format), `evidence/` (the raw audit evidence this plan is built from).

---

## Executive summary — the five things that matter

1. **Google is being told the site's canonical home is a login page.** housebets.win declares `rel="canonical"` and `og:url` pointing at **housebets.dev**, and its sitemap index sends crawlers to eight child sitemaps on housebets.dev — a domain that redirects to a Cloudflare Access **login wall**. Whatever the internal intent (the .dev host is evidently the origin/staging domain), the signals leaving the building point search engines at a door they cannot open.
2. **The brand does not own its own name.** In samples taken 13 Nov 2026, **housebets.com appears in the top 5 results for 0 of 10 brand queries** — including "housebets" itself and "housebets login". Affiliates, review sites and lookalike domains (housebets.co, housebets.me.uk, housebets.uk.com, housebetcasino.com, house-bets.net…) answer every brand question instead.
3. **The answer pool contradicts itself on the basic facts.** Four incompatible welcome-bonus stories, four different launch years, a sportsbook that one scored review says does not exist, an app that affiliates invented. AI answer engines inherit all of it, because the official site gives them nothing to cite.
4. **The product claims that should win searches are unowned.** "Instant withdrawals" is Housebets' own headline, yet the brand is absent from the payout-speed answers; provably fair is a Housebets feature with no Housebets page.
5. **The trust assets are real but invisible.** A verifiable Curaçao licence (OGL/2024/431/0231, Bridge Technologies B.V., "Granted and Active" on the register), an on-chain bankroll page, named dispute resolution (Resolvo) — none of it is assembled on an official, rankable page.

The fix order follows from that: repair the signals (§1) → own the brand queries (§3) → build the money pages (§4, §2) → become the cited source for AI answers (§5) → sequence and cost it (§6).

---

## 1. Audit (20 pts)

### F1 — CRITICAL: canonical, hreflang and og:url point at a login-gated staging domain
Fetched 13 Nov 2026 from https://www.housebets.win/ (HTTP 200, full server-rendered HTML):
- `<link rel="canonical" href="https://housebets.dev">`
- `og:url` = `https://housebets.dev`
- hreflang alternates for ~19 locales all resolve to `housebets.dev/...` URLs.

`https://housebets.dev/` itself answers with a 302 to `housebets.cloudflareaccess.com` (Cloudflare Access sign-in). A canonical is a instruction to search engines: "index that URL instead of me." The URL named cannot be indexed at all. Fix: emit canonical/hreflang/og:url on the serving domain — .com canonical to .com, and decide the .win question in F3. Verify no template in the stack hard-codes the .dev origin (the meta description and sitemap hostnames below suggest it does).

### F2 — CRITICAL: the only fetchable sitemap index lives on .win and lists .dev sitemaps
- `housebets.win/sitemap.xml` (HTTP 200) is a sitemap index whose 8 children are all on **housebets.dev**: sitemap-static, sitemap-catalog-games-0/1, sitemap-catalog-providers, sitemap-sanity-blog, sitemap-sanity-promotions, sitemap-sanity-legal, sitemap-sanity-about.
- `housebets.com/robots.txt` declares `Sitemap: https://www.housebets.com/sitemap.xml` — but from an unverified IP that URL returns the Cloudflare challenge (F4), so its contents could not be confirmed in this audit.
- The games-catalog sitemaps are exactly the inventory that should power game landing pages (§4); today the crawl path to them runs through a login wall.

### F3 — HIGH: two live public domains, one site, no consolidation
housebets.com and housebets.win both serve the full site with the **identical title** ("Best Crypto Casino & Sportsbook | Housebets") and near-duplicate homepages. .win is *more* crawler-open than .com (full 506 KB SSR HTML to a plain fetch, vs a challenge on .com). Nothing tells a crawler which domain is the brand. This splits links, reviews and entity signals across two hosts and invites the "which one is real?" problem that the lookalike domains in §3 already exploit. Recommendation: pick **.com** (it is the domain on the licence's approved-domains list and the one reviewers cite), 301 .win → .com path-for-path, and keep .win registered defensively.

### F4 — HIGH (with an honesty caveat): bot gating makes the crawler view unverifiable from outside
From a datacenter IP on 13 Nov 2026: `GET housebets.com/` → HTTP 403 Cloudflare "Just a moment…" — including with a Googlebot user-agent string (the challenge page itself is `noindex,nofollow`). **Caveat, stated plainly:** a spoofed UA from a datacenter IP is not verified Googlebot; real Googlebot is normally identified by reverse-DNS/ASN and may pass freely. This audit therefore does **not** claim "Googlebot is blocked." It claims two narrower, checkable things:
- The site hard-gates anything it cannot verify, in a business that is **geo-restricted by design** (the contest brief itself notes Housebets is not open in some countries). The failure mode to rule out is a geo/bot rule that also catches Googlebot's non-US crawls or the render service.
- The required verification is one hour of work with first-party data: Search Console's URL Inspection live test + crawl stats, and server logs filtered to verified Googlebot (reverse-DNS `*.googlebot.com`) by country. That check is task #1 in the 90-day plan (§6) because every other fix is moot if it fails.

### F5 — MEDIUM: robots.txt is a 521-rule wall around the app — correct instinct, two risks
`housebets.com/robots.txt`: 12,602 bytes, 521 Disallow rules — the same ~17 app/account/admin path patterns repeated across 19 locale prefixes (ar, da, de, es, fi, fr, hi, hu, id, it, ja, ko, pl, pt, ru, th, tr, vi, zh). Blocking /login, /register, /settings, /transactions etc. is right. Risks: (a) any future public route that accidentally matches a blocked stem stays invisible; (b) 19 locales × identical app-path rules implies 19 locale subtrees whose hreflang set currently points at .dev (F1) — after the canonical fix, re-validate that each locale's money pages are crawlable and self-canonical. .win's robots.txt is byte-identical except its Sitemap line points at .dev (F2).

### F6 — MEDIUM: the promotions page cannot answer the bonus question
The crawler fetch of `/promotions` returns cards (Duel Mode, the $324,000 watch race, the $150,000 leaderboard, Ambassador Program) with **no explanatory body text** — no statement of the rewards model, no terms, nothing a search engine or an AI can quote. Into that vacuum, affiliates publish four contradictory offers (§3). The page that should be the canonical answer to "housebets bonus" currently says almost nothing in crawlable words.

### F7 — MEDIUM: trust signals exist but are scattered and conflicting
What Google (and YMYL reviewers) want from a gambling site, checked against public evidence:
- **Licence:** real and verifiable — Curaçao GCB licence **OGL/2024/431/0231**, operator **Bridge Technologies B.V.** (CoC 160264(0), Willemstad); register status "Granted and Active", housebets.com on the approved-domains list (per casino.clinic, which opened the register entry; realcasinosbonus.com re-checked the register 1 Nov 2026). But this lives on third-party pages, not assembled on an official one.
- **Scope honesty:** Housebets' own Regulatory Scope page (as reported by two review sources) states the licence does **not** cover the $HBTS token, airdrops, on-chain rewards or the affiliate programme — good practice that almost no searcher will ever find today.
- **Dispute resolution:** Resolvo (Curaçao) is named in third-party sources.
- **Conflicts to clean up:** review scores for the same brand range from 62.5/100 (WagerDB, security 3.0, "no 2FA") to 7.7 (Casino Guru) to 80.5 (Scam Detector); Housebets' own documents reportedly contradict each other on KYC (policy says withdrawals require completed KYC; help centre says most players are never asked) — one consistent, published KYC/withdrawal policy is both a trust fix and an SEO fix (§4, /payments).
- **A genuinely differentiating proof:** the homepage already shows "House bankroll · on-chain $2,850,275" with an Arkham link. That is a citable, checkable trust asset no affiliate can copy — it belongs in the SEO story, not just the hero.

### F8 — Satellite sites: the verdict is DON'T — consolidate instead
Housebets already has, in effect, a satellite problem it didn't choose (lookalikes, §3) plus one it did (the .win twin, F3). Building further satellite/doorway sites would add: duplicate-content competition with the main domain, licence-scope exposure (promotional sites making offers the licence doesn't cover is precisely what the Regulatory Scope page warns about), and more surface for the affiliate-copy chaos in §3. **Recommendation: no new satellites.** Consolidate .win → .com, fix .dev out of every public signal, and spend the same effort on subpaths (which inherit the domain's authority) instead of new domains.

---

## 2. Keywords and content (15 pts)

Full map: **`keywords.csv`** — 43 keywords, each with intent, one target page and a priority. The logic in brief:

- **P0 — brand defence first (13 keywords).** Every "housebets + X" query is high-intent and currently answered by someone else. They map to just four official pages: `/` (brand, casino, login), `/promotions` (bonus, promo code, rakeback, duel mode), `/about-us` (review, legit, scam, app, token scope), `/payments` (withdrawal). Owning the brand set is the cheapest traffic in the plan: the demand exists today and the competition is affiliates, not Stake.
- **P1 — claim the differentiator clusters (14).** Provably fair (explainer + game pages), payout speed (payments page + published test log), no-KYC (only with the corrected, consistent policy from F7), crypto casino / crypto sportsbook head terms via the new landing pages.
- **P2/P3 — long tail and per-game pages (16).** The games-catalog sitemaps (F2) are a ready-made inventory for game landing pages (crash, plinko, mines, dice…), sequenced after the money pages.
- **Content rule:** no keyword is targeted with a claim the official pages cannot prove. Where affiliates outrank with invented offers, the counter is not a louder offer — it is the canonical fact, published once and syndicated (§3).

Volumes are deliberately **not** quoted: they are not verifiable from public data without the operator's Search Console, and inventing them would poison the prioritisation. First-party GSC export is a week-1 task in §6; the P0/P1 order above is built from intent and SERP ownership, which public data *can* establish.

---

## 3. Brand searches (20 pts)

### The state of play (sampled 13 Nov 2026, top 5 per query)

| Query | Official .com in top 5? | Who answers instead |
|---|---|---|
| housebets | No | cryptocasinoreviews.io, house-bets.net, WagerDB, cryptotips, getinspired |
| housebets casino | No | review sites + housebetcasino.com (lookalike) |
| housebets bonus | No | cryptotips, housebets.co (lookalike), casino.clinic |
| housebets promo code | No | cryptotips, housebets.co, hbbonus.com, eu-housebets.com |
| housebets review | No | WagerDB, cryptotips, casino.clinic, realmoneyaction |
| housebets login | No | housebets.me.uk, house-bets.bet, housebets.uk.com, housebetsapp.com |
| housebets withdrawal | No | casino.clinic, realcasinosbonus, UK lookalikes |
| housebets scam | No | Trustpilot profile, scam-detector, Scamadviser, Casino Guru |
| housebets legit | No | review sites (licence cited correctly, caveats attached) |
| housebets sportsbook | No | cryptotips, house-bets.net, eu-housebets.com |

**0 for 10.** Three distinct problems, three distinct fixes:

### 3.1 The navigational leak (housebets, login, casino)
People typing the brand — including existing players typing "housebets login" — land on third parties. The login SERP is the worst: four lookalike domains, some claiming a downloadable app and UK availability that do not exist. This is a phishing-shaped hole, not just an SEO metric. Fixes, in order: (1) the canonical/sitemap repair (F1/F2) so the official domain's signals stop pointing at a login wall; (2) brand-entity basics — consistent name/logo/links across the official socials and the Trustpilot profile (which already ranks #1 for "housebets scam" and should be claimed and answered); (3) sitelinks hygiene — a small set of crawlable, well-titled public pages (/, /promotions, /contests, /about-us) for Google to choose from; (4) report the impersonating login pages to their registrars/hosts and to Google's phishing/impersonation channels, with the licence's approved-domains list as evidence.

### 3.2 The fact vacuum (bonus, promo code, withdrawal)
The top-5 for "housebets bonus"/"promo code" contains at least **four mutually exclusive offers**: no welcome bonus at all (casino.clinic, cryptocasinoreviews.io) · 100% up to $1,000, 35x, code MAX (cryptotips) · 100% up to $500 + 100 free spins, codes HOUSE100/SPIN50 (housebets.co) · "$25,000 free money, 1x wagering", code socgid (hbbonus.com, tying into the $5M Wave wallet-scan campaign). A player cannot tell which is real; neither can an AI (§5). Fixes: (1) rebuild /promotions as the canonical, crawlable statement of the actual model — rakeback/lossback slider, Duel Mode's 50% house-edge return, XP/leaderboard, the watch race, and an explicit line on promo codes (which exist, which don't); (2) ship affiliates one approved-copy feed and make its use a programme term — the programme is outside the licence's scope per Housebets' own Regulatory Scope page, which makes copy discipline a compliance matter, not just marketing; (3) /payments publishes the real withdrawal facts (assets, minimums, the observed ~1-minute processing, when KYC is and isn't required — one policy, F7) so the withdrawal SERP has an official answer to rank.

### 3.3 The reputation set (review, legit, scam)
Reviewers converge on the licence being real — and diverge on everything else, with two recurring negatives: the KYC-after-a-winning-session pattern and a documented ~9,228 USDC retained-funds case (cryptotips, repeated by crash-games.net and a Bitcointalk thread). Fixes: (1) /about-us as a review-grade fact page: operator, licence number with a register link, dated timeline (kills the 2023/2024/2025 launch-year chaos), the token-scope statement, ADR (Resolvo), responsible-gambling links; (2) a corrections round: WagerDB's "no sportsbook" is factually wrong and its 3.0 security score cites missing 2FA — both are fixable facts, by product change or documented correction; AskGamblers' invented €1,000 bonus likewise; (3) answer, don't ignore, the Trustpilot profile — it is the #1 result for "housebets scam", and a claimed, responding profile changes what that page says about the brand.

**Target (goal, not promise):** official pages in the top 3 for all 10 tracked brand queries by day 90, and #1 for the navigational set (housebets, login) by day 45 — the mechanics (canonical repair + entity work + four crawlable pages) are the same mechanics that fix §5.

---

## 4. Pages (15 pts)

| Page | State | Job to do | Primary keywords |
|---|---|---|---|
| `/` | Exists; thin crawler extraction; canonical signals broken | Rank for the brand; carry the entity facts (licence line, on-chain bankroll proof) in crawlable text | housebets, housebets casino, housebets login |
| `/promotions` | Exists; cards only, no terms text (F6) | Become the canonical rewards statement (§3.2) | housebets bonus, promo code, rakeback, duel mode |
| `/about-us` | Exists as a sitemap section (sanity-about) | Review-grade fact page: operator, licence + register link, timeline, token scope, ADR, RG | housebets review, legit, scam, is housebets legit |
| `/payments` | **New** | Real withdrawal/deposit facts + a dated payout-speed test log Housebets runs and publishes itself | housebets withdrawal, instant withdrawals cluster |
| `/crypto-casino` | **New** | Category landing: what the casino is, games, PF, rewards model — written to be cited by listicles and AI | crypto casino, best crypto casino, no kyc cluster |
| `/provably-fair` | **New** | Explainer (how PF works, how to verify a bet) wired to the playable House Originals | provably fair casino/games, how to verify |
| `/sportsbook` | **New landing** (the book exists in-app) | Give the sportsbook an indexable home: markets, the shared balance, cash-out — the page reviewers currently claim doesn't exist | housebets sportsbook, crypto sportsbook |
| Game pages (from catalog sitemaps) | Inventory exists in sitemaps | One crawlable page per House Original + top third-party titles, PF verification embedded | crash/plinko/mines/dice clusters |
| `/contests` | Exists; fullest crawler extraction on the site | Keep — it's proof the stack *can* serve rich crawlable pages; use it as the internal template for the rebuilds above | (brand/PR value) |

Every page ships with: self-referencing canonical on the serving domain, FAQ blocks written in the exact question forms tracked in `ai-answers.csv`, and the licence/operator footer block. Pages are specified before they are written — the briefs are a week-2 deliverable in §6.

---

## 5. AI search (15 pts)

**Method note (read first).** The samples in `ai-answers.csv` were collected 13 Nov 2026 as **search-surfaced source-pool samples**: the ranked sources and claims that answer engines (ChatGPT, Perplexity, Gemini, AI Overviews) draw on for these questions. No AI-overview answer block was surfaced by the sampling tool, and no direct engine sessions were run for this entry — claiming otherwise would be fabrication. The csv therefore documents, per question, what an engine *would have to build its answer from*, whether Housebets is mentioned, and whether that pool is correct. A week-1 task in §6 is the direct-session baseline (50 questions × 4 engines, logged in the same csv format) so movement becomes measurable.

**What the pool says today (evidence: `ai-answers.csv`, `evidence/ai_search_notes.md`):**
- "Best crypto casino" and "instant withdrawals" answers **never mention Housebets** — the brand is not in the consideration set the engines assemble, despite holding a product claim (instant withdrawals) that is literally its meta description.
- "Is Housebets legit / safe" answers would be built **entirely from third parties** and would inherit: the KYC-after-winning narrative, the 9,228 USDC case, WagerDB's false "no sportsbook", and a trust-score spread (7.7 vs 62.5 vs 80.5) that no engine can reconcile.
- Bonus and launch-year questions have **no correct answer available** in the pool — four versions of each circulate (§3.2, ai-answers rows 5 and 8).

**How Housebets becomes the source they cite:**
1. **Publish the citable facts once, officially** — the /about-us fact page and /payments test log (§4) are written as quotable blocks (licence line, dated timeline, payout table), because engines cite pages that state facts in extractable form.
2. **Structured data** — Organization + FAQ schema on the fact pages, same facts as the visible text, so machine readers get one version.
3. **Fix the top of the citation graph** — the review set (WagerDB, cryptotips, AskGamblers, casino.clinic) is what engines cite most; the corrections round in §3.3 is therefore also the AI-search fix.
4. **Earn listicle inclusion** for the category questions — the /crypto-casino page plus the payout data give comparison sites something to cite; engines follow.
5. **Measure honestly** — monthly re-runs of the 50-question baseline, logged to the same csv; report mention rate and correctness rate, not vibes.

---

## 6. First 90 days (15 pts)

Costs are planning figures in USD at blended contractor rates (SEO lead ~$75/h, developer ~$60/h, writer ~$40/h), stated so the arithmetic can be argued with. Team assumed: 1 SEO lead (part-time), 1 developer (part-time), 1 writer (part-time) — or the monthly-contract equivalent.

### Days 1–30 — Foundation: stop pointing Google at a login page
| Work | Owner | Cost |
|---|---|---|
| Verified-Googlebot log audit + Search Console crawl inspection (F4) | SEO lead | $600 |
| Canonical/hreflang/og:url fix; .dev removed from all public signals (F1) | Dev | $1,200 |
| Sitemap rebuild on .com; resubmit in GSC (F2) | Dev | $600 |
| .win → .com 301 consolidation plan + execution (F3) | Dev + SEO | $900 |
| GSC query export; keyword map v2 with real volumes (replaces §2 estimates) | SEO lead | $600 |
| AI-search direct baseline: 50 questions × 4 engines into ai-answers.csv format (§5) | SEO lead | $750 |
| /promotions rebuild: canonical rewards statement, crawlable terms (F6) | Writer + Dev | $1,350 |
| Trustpilot claimed; impersonation reports filed for login lookalikes (§3.1) | SEO lead | $450 |
| **Month 1 total** | | **≈ $6,450** |

### Days 31–60 — Money pages: own the brand, open the categories
| Work | Owner | Cost |
|---|---|---|
| /about-us fact page (licence, timeline, scope, ADR, RG) (§3.3) | Writer + SEO | $1,100 |
| /payments with self-run dated payout-speed test log | SEO + Writer | $1,300 |
| /crypto-casino and /provably-fair landings (§4) | Writer + Dev | $2,400 |
| /sportsbook landing | Writer + Dev | $1,200 |
| Corrections round: WagerDB, AskGamblers, cryptotips fact fixes (§3.3) | SEO lead | $750 |
| Affiliate approved-copy feed + programme terms update (§3.2) | SEO lead | $600 |
| FAQ/Organization schema across fact pages | Dev | $600 |
| **Month 2 total** | | **≈ $7,950** |

### Days 61–90 — Growth: game pages, citations, category push
| Work | Owner | Cost |
|---|---|---|
| Game landing pages from catalog sitemaps, first 30 titles | Writer + Dev | $2,600 |
| Digital PR: payout-data story + PF explainer pitched to listicle set (category SERP owners, evidence file 3) | SEO lead | $1,500 |
| Review-placement programme: get Housebets into the 10 comparison lists that dominate category answers | SEO lead | $1,200 |
| Locale check: 19 locale subtrees validated post-canonical-fix (F5) | SEO + Dev | $900 |
| AI-search re-measurement + plan v2 | SEO lead | $600 |
| **Month 3 total** | | **≈ $6,800** |

**90-day total ≈ $21,200** (contractor model; less under a monthly contract, which is the arrangement this contest offers its best entrants).

### Targets — goals with the reasoning shown, not promises
- **Brand ownership:** official top-3 for 10/10 tracked brand queries by day 90; #1 for "housebets" and "housebets login" by day 45. Reasoning: the incumbents are affiliates and a login-wall canonical, not entrenched media.
- **Index health:** 100% of sitemap URLs on the canonical .com host, zero sitemap/canonical references to .dev, verified-Googlebot crawl errors at zero on money pages.
- **AI answers:** Housebets mentioned in ≥50% of category answers in the 50-question baseline by day 90 (from 0% in this audit's samples), and the bonus/launch-year facts correct in ≥80% of brand answers.
- **Traffic:** first measurable non-brand organic clicks from the PF and payout clusters by day 90. (No absolute visit number is quoted: without the GSC baseline any figure would be invented. The week-1 export sets the baseline; the day-90 review judges against it.)

---

## About you

This plan was produced by an independent entrant whose background is operating and auditing live web3 products — reading contracts, explorers and production sites the way this audit reads Housebets: fetch first, quote only what the fetch returned, label what could not be verified. The track record offered is this document itself plus its evidence files: every claim in it can be re-checked in minutes, which is the working style the monthly contract would be buying.

- Public work / code: github.com/fairusfelles
- CV / LinkedIn: [to be added by the entrant before submission — the plan stands as the proof of work either way]

**Tools used** (named per the contest rules): Muse (AI assistant) for research and drafting; public web sources as cited. No templates, no copied entries.
