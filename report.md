# Win Google for Housebets — SEO growth report for housebets.com

**Entry for the Housebets Contest Labs contest "Win Google for Housebets"**, structured on the starter pack's report outline, with the 8 Nov 2026 addition (AI search) inserted as section 5, before the 90 days. Audit date: 13 November 2026.

**Method:** public data only — live fetches of housebets.com, housebets.win and housebets.dev (HTML, robots.txt, sitemaps), and Google search sampling (English) for 10 brand queries, 6 category queries and 4 AI-style questions. No login, no paid tools. Every finding cites what was fetched and when, so the operator can re-check each one in minutes.

**Honest framing:** this is a plan, not completed client work. Targets in §6 are goals with the reasoning shown. Where public data could not settle a question (search volumes, Core Web Vitals field data, verified-Googlebot behaviour), the report says so and names the first-party check that would — those checks are scheduled as week-1 tasks rather than papered over with invented numbers.

**Companion files:** `keywords.csv` (43 keywords, starter-pack columns), `brand-serp.csv` (23 brand-SERP results with owner, risk and action), `pages.csv` (7 pages, before/after), `ai-answers.csv` (AI-search samples in the required columns), `evidence/` (raw audit evidence).

---

## Executive summary — the five things that matter

1. **Google is being told the site's canonical home is a login page.** housebets.win declares canonical, og:url and every hreflang alternate pointing at **housebets.dev** — a domain that redirects to a Cloudflare Access sign-in wall — and its sitemap index sends crawlers to eight child sitemaps on the same gated host.
2. **The brand owns none of its own searches.** Sampled 13 Nov 2026: housebets.com appears in the **top 5 for 0 of 10 brand queries**, including "housebets" and "housebets login". Lookalike domains (housebets.co, housebets.me.uk, housebetcasino.com, housebetsapp.com…) and affiliates answer instead.
3. **The facts contradict each other — including on Housebets' own pages.** Four incompatible welcome-bonus stories circulate among affiliates, while the official /promotions meta itself advertises "deposit bonuses and free spins" that independent testers say do not exist; /, /rewards and /about-us share one identical title and near-identical meta, and /about-us serves an empty H1.
4. **The winning claims are unowned.** "Instant withdrawals" is Housebets' headline and its meta description, yet the brand is absent from payout-speed answers; provably fair has a genuinely good page that nothing links to.
5. **The trust assets are real but invisible.** A verifiable Curaçao licence (OGL/2024/431/0231, Bridge Technologies B.V., "Granted and Active"), named dispute resolution (Resolvo), an on-chain bankroll shown in the hero ($2,850,275) — none of it assembled on an official, rankable page.

Fix order: repair the signals (§1) → win back the brand (§3) → rebuild the pages (§4, §2) → become the cited source for AI answers (§5) → sequence and cost it (§6).

---

## 1. Audit

Each finding: evidence, fix, impact, effort.

### 1.1 Crawl, indexing, sitemaps, robots, canonicals

**F1 — CRITICAL · Canonical, hreflang and og:url point at a login-gated staging domain.**
Evidence: HTML fetched from https://www.housebets.win/ on 13 Nov 2026 contains `<link rel="canonical" href="https://housebets.dev">`, `og:url` = `https://housebets.dev`, and hreflang alternates for all languages resolving to housebets.dev URLs. `https://housebets.dev/` answers 302 → `housebets.cloudflareaccess.com` (Cloudflare Access sign-in). A canonical is an instruction to index the named URL instead; the named URL cannot be indexed at all. Fix: emit canonical/hreflang/og:url on the serving domain; audit the template/config for the hard-coded .dev origin (it recurs in the sitemap hostnames, F2). Impact: high. Effort: S–M.

**F2 — CRITICAL · The only publicly fetchable sitemap index lives on .win and lists .dev sitemaps.**
Evidence: `housebets.win/sitemap.xml` (HTTP 200) is an index of 8 children, all on housebets.dev: sitemap-static, sitemap-catalog-games-0/1, sitemap-catalog-providers, sitemap-sanity-blog, sitemap-sanity-promotions, sitemap-sanity-legal, sitemap-sanity-about. `housebets.com/robots.txt` declares `Sitemap: https://www.housebets.com/sitemap.xml`, but from an unverified IP that URL returns the Cloudflare challenge (F4), so its contents could not be confirmed in this audit. The games-catalog sitemaps are exactly the inventory §4 builds game pages from; today the crawl path to them runs through a login wall. Fix: publish the index on .com with .com URLs; resubmit in Search Console. Impact: high. Effort: S.

**F3 — HIGH · Two live public domains, one site, no consolidation.**
Evidence: housebets.com and housebets.win both serve the full site under the identical title "Best Crypto Casino & Sportsbook | Housebets" with near-duplicate homepages; .win is the *more* crawler-open of the two (full ~506 KB server-rendered HTML to a plain fetch vs a bot challenge on .com). Nothing tells a crawler which host is the brand, so links, reviews and entity signals split across both — and the ambiguity is exactly what the §3 lookalikes exploit. Fix: standardise on **.com** (it is the domain on the licence's approved-domains list and the one reviewers cite); 301 .win → .com path-for-path; keep .win registered defensively. Impact: high. Effort: M.

**F4 — HIGH, with an honesty caveat · Bot gating makes the crawler view unverifiable from outside.**
Evidence: from a datacenter IP on 13 Nov 2026, `GET housebets.com/` returned HTTP 403 (Cloudflare "Just a moment…", the challenge page itself `noindex,nofollow`) — including with a Googlebot user-agent string. **Caveat:** a spoofed UA from a datacenter IP is not verified Googlebot; real Googlebot is normally identified by reverse DNS/ASN and may pass freely, so this audit does *not* claim Googlebot is blocked. The narrower, checkable claims: the site hard-gates anything unverified, in a business that is geo-restricted by design (the contest brief itself notes Housebets is not open in some countries); the failure mode to exclude is a bot/geo rule that also catches Google's render service or non-US Googlebot. Fix (verification, one hour, first-party data): Search Console URL Inspection live test + crawl stats, and server logs filtered to verified Googlebot (`*.googlebot.com`) by country. Scheduled as the first task of §6 because every other fix is moot if it fails. Impact: high. Effort: S.

**F5 — MEDIUM · robots.txt: right instinct, two structural risks.**
Evidence: `housebets.com/robots.txt` is 12,602 bytes / 521 Disallow rules — the same ~17 app/account/admin stems (/login, /register, /settings, /transactions, /partner, /operations…) repeated across 19 locale prefixes; .win's file is identical except its Sitemap line points at .dev. Blocking the app surface is correct. Risks: (a) any future public route matching a blocked stem stays invisible; (b) 19 locale subtrees whose hreflang currently resolves to .dev (F1) must be re-validated after the canonical fix so each locale's money pages are crawlable and self-canonical. Impact: medium. Effort: S.

### 1.2 Languages and hreflang (20 languages)

The site runs 20 languages (en + /ar /da /de /es /fi /fr /hi /hu /id /it /ja /ko /pl /pt /ru /th /tr /vi /zh, per the starter pack's languages.csv and the robots.txt prefixes). Today **every language's hreflang alternate points at the login-gated .dev host** (F1) — so the entire international signal layer is aimed at URLs that return a sign-in wall. Until that is fixed, translated titles/metas (which do exist — the .win HTML carries a proper English set, and §1.4 shows several pages done well) cannot earn their rankings. Plan: fix the host first (one config change repairs all 20 languages at once), then validate per-language self-canonicals on the money pages, then extend the keyword map per language (§2) — starting with the markets where brand-query hijacking is worst (the UK-facing lookalikes in §3 argue for en-GB attention first).

### 1.3 Speed and Core Web Vitals

Stated plainly: **field CWV data (CrUX) and a keyed PageSpeed run were not obtainable from public access during this audit** — the public PageSpeed Insights API returned HTTP 429 (rate limit) from this network, and CrUX/GSC data is first-party. Inventing LCP/INP figures would be exactly the kind of number this report refuses to invent. What public evidence does show: the .win homepage delivers ~506 KB of server-rendered HTML to a plain fetch (healthy for rendering, heavy if it all blocks first paint on a phone), while the .com app routes gate unverified clients entirely. The CWV baseline (CrUX via GSC + one lab run per template, phone first per the outline) is a week-1 task in §6 with named owners; budgets to adopt once measured: LCP ≤ 2.5 s, INP ≤ 200 ms, CLS ≤ 0.1 on the home, casino and game templates.

### 1.4 How Google renders the pages (JavaScript-built)

Crawler fetches during this audit returned real but uneven content: the homepage extracted thin (~32 lines: hero, login options, bankroll figure), /promotions extracted as cards with no body text, while /contests extracted in full (~75 lines) and .win's homepage much fuller (~132 lines) — proof the stack *can* serve rich crawlable pages when a route is built that way. Combined with F4, the rendering story is: content exists server-side on at least some routes; the risk is concentrated in (a) which routes are JS-dependent, and (b) whether verified Googlebot's render queue sees what the crawler fetch saw. The §6 verification task settles (b); §4 rebuilds the worst routes (a) on the /contests pattern.

### 1.5 Titles, metas, headings, internal links, structured data

Fetched 13 Nov 2026 from the served HTML — the good and the bad are both instructive:
- **Duplicate title/meta across money pages:** `/`, `/rewards` and `/about-us` all carry the title "Best Crypto Casino & Sportsbook | Housebets" and near-identical metas ("Join Housebets for crypto slots and sports betting…" / "…crypto slots & sports betting…"). Three different intents, one label. And `/about-us` serves an **empty H1**.
- **A meta that contradicts the product:** `/promotions`' meta reads "Explore the latest crypto casino promotions, deposit bonuses, and free spins at Housebets" — while casino.clinic and cryptocasinoreviews.io, who tested the site, report there is **no public welcome bonus or free-spins offer** (rewards are a rakeback/lossback slider + Duel Mode). The official meta is feeding the exact confusion §3 documents.
- **Done well (keep, and copy the pattern):** `/provably-fair` ("Provably fair | Housebets", a meta that actually explains the mechanism), `/sports` ("Sports Betting | Housebets", accurate meta), `/house-games`, `/casino` (generic meta, but unique title/H1).
- **Internal links:** the provably-fair page — the site's best explainer asset — is not woven into the House Originals journey; game pages exist as sitemap inventory (F2) rather than as linked, described destinations.
- **Structured data:** no Organization/FAQ schema was observed in the fetched pages; §4 specifies it per page, because §5's AI answers cite pages that state facts in extractable form.

Full before/after for seven pages: **`pages.csv`**.

### 1.6 Trust signals for a gambling site

- **Licence — real and verifiable:** Curaçao GCB licence **OGL/2024/431/0231**, operator **Bridge Technologies B.V.** (CoC 160264(0), Willemstad); register status "Granted and Active" with housebets.com on the approved-domains list (casino.clinic opened the register entry; realcasinosbonus.com re-checked it 1 Nov 2026). Today this lives on third-party pages, not assembled on an official one.
- **Scope honesty:** Housebets' own Regulatory Scope page (as reported by two review sources) states the licence does **not** cover the $HBTS token, airdrops, on-chain rewards or the affiliate programme — good practice that searchers never find today; §4 puts it on /about-us.
- **Dispute resolution:** Resolvo (Curaçao), named in third-party sources.
- **Conflicts to clean:** review scores for one brand span 62.5/100 (WagerDB; security 3.0, "no 2FA") to 7.7 (Casino Guru) to 80.5 (Scam Detector); and Housebets' own documents reportedly contradict each other on KYC (policy: withdrawals require completed KYC; help centre: most players are never asked — documented by realcasinosbonus.com). One published, consistent KYC/withdrawal policy is a trust fix and an SEO fix at once (§4, /payments).

---

## 2. Keywords and satellites

### 2.1 The keyword map

Full map: **`keywords.csv`** — 43 keywords in the starter-pack columns (keyword, language, market, intent, monthly_volume, difficulty, our_rank_now, target_page, priority, source). Volume and difficulty are left blank, as in the pack's own example rows: they are not verifiable from public data, and the week-1 Search Console export replaces them with first-party figures. `our_rank_now` is from this audit's own samples (13 Nov 2026).

Clusters, in priority order:
- **Brand defence (priority 1, 10 keywords)** → four official pages: `/` (housebets, housebets casino, housebets login), `/rewards` (bonus, promo code), `/about-us` (review, legit ×2), `/payments` — new (§4) — (withdrawal), `/sports` (sportsbook). Every one of these queries has existing demand and affiliate-grade competition; it is the cheapest traffic in the plan.
- **Differentiator clusters (priority 2)** → provably fair (`/provably-fair` + `/house-games`), payout speed (`/payments`), category heads (`/casino` for crypto casino / best crypto casino / no-KYC cluster, `/sports` for crypto sportsbook), plus the product terms reviewers already use (duel mode, rakeback).
- **Long tail & games (priority 3)** → per-game terms from the catalog inventory (plinko crypto is the pack's own example: → `/house-games`, priority 2 — adopted), the $HBTS/House Keys clarification queries, payout-proof and rakeback long tails.
- **By language and market:** this map is en/global, deliberately. The other 19 languages inherit the §1 fixes first (canonical/hreflang host repair is language-agnostic); per-language maps are a month-2/3 task built on GSC data per locale, not on guesses. First expansion candidates by evidence: en-GB (the login-lookalike cluster is UK-facing, §3) and the locales whose brand SERPs show the same hijack pattern on sampling.

**Where we rank now and the gap to the top 3:** for the 10 brand queries the official site is **outside the top 5 in 10 of 10** (gap: the whole first page); for the 6 category queries it is **absent from the sampled results entirely** (gap: not yet in the consideration set — a content-and-citations problem, addressed by §4 pages + §6 digital PR, not by on-page tweaks alone).

### 2.2 Satellite sites — the answer is "don't" (and what to do instead)

The outline asks which satellites, what each would cover, how they would link, and how they stay within Google's spam policies. The honest answer, argued rather than asserted:

- **Housebets already suffers from satellites — other people's.** §3 catalogues a dozen lookalike domains ranking on the brand. Adding official satellites multiplies the "which one is real?" problem that already costs the brand its login query.
- **The owned twin is already a liability.** housebets.win (F3) splits signals today; the programme the brief imagines satellites doing — covering sub-topics and linking back — is done better by **subpaths on .com** (/provably-fair, /house-games, topic hubs), which inherit the domain's authority instead of starting from zero and reading as doorways.
- **Spam-policy exposure is real:** satellite networks built to funnel rankings are the textbook doorway/scaled-content pattern in Google's spam policies; the defence "each site has unique value" is expensive to keep true across 20 languages, and a gambling brand additionally carries licence scope — promotional sites making offers the Curaçao licence doesn't cover is precisely what Housebets' own Regulatory Scope page warns about.
- **Verdict: build no new satellites.** Consolidate .win → .com (F3), keep defensive registrations, and spend the same budget on §4's pages. If the operator ever overrides this, the guardrails are: one clearly-branded property per genuinely distinct product, unique first-party content, transparent ownership, no cross-linking schemes, no offer claims beyond the licence scope.

---

## 3. Win back our brand searches

Full evidence table: **`brand-serp.csv`** — 23 results across the 10 sampled queries, each with owner type, evidence, risk and the legitimate action. The pattern:

**0 for 10.** In the 13 Nov 2026 samples, no brand query — not even "housebets" itself — returns an official page in the top 5. Three distinct problems:

**3.1 The navigational leak (housebets, housebets login, housebets casino).** The login SERP is the worst: four lookalike domains (housebets.me.uk, house-bets.bet, housebets.uk.com, housebetsapp.com), some presenting a downloadable app that does not officially exist and implying UK availability that is geo-blocked. That is a phishing-shaped hole, not just a ranking problem. Legitimate routes, in order: the §1 signal repair (canonicals/sitemaps stop pointing at a login wall); brand-entity basics (consistent name/logo/links; claim and answer the Trustpilot profile, which ranks #1 for "housebets scam"); sitelink hygiene (a small set of crawlable, well-titled public pages); and for the impersonators, **trademark complaints to registrars/hosts** with the licence's approved-domains list as evidence, escalating to phishing reports wherever credential capture is found.

**3.2 The fact vacuum (housebets bonus, housebets promo code, housebets withdrawal).** At least four mutually exclusive offers sit in these top 5s: no welcome bonus at all (casino.clinic, cryptocasinoreviews.io) · 100% up to $1,000, 35x, code MAX (cryptotips) · 100% up to $500 + free spins, codes HOUSE100/SPIN50 (housebets.co) · "$25,000 free money, 1x wagering", code socgid (hbbonus.com). Withdrawal answers contradict too (1 minute vs 60 minutes vs invented fiat rails on UK lookalikes). Routes: **outrank** with the canonical official statement (§4 /rewards + /payments), and **ask-partner** corrections for affiliates — with one approved-copy feed made a programme term. (The affiliate programme sits outside the licence's scope per the Regulatory Scope page, which makes copy discipline a compliance matter, not just marketing.)

**3.3 The reputation set (housebets review, housebets legit, housebets scam).** Reviewers converge on the licence being real and diverge on everything else; two negatives recur — the KYC-after-a-winning-session pattern and a documented ~9,228 USDC retained-funds case. Those are reported facts, not removables: the legitimate route is **outrank + fix the root** — an official fact page (§4 /about-us), corrections to factual errors (WagerDB's "no sportsbook" claim is simply wrong; AskGamblers' €1,000 bonus is invented), one consistent KYC policy (§1.6), and a claimed, responding Trustpilot profile. What this plan will never recommend: fake reviews, suppression services, or legal threats against truthful reporting — they backfire and, in the EU/UK, breach consumer-protection rules on review integrity.

**Targets (goals, not promises):** official pages top-3 for all 10 tracked brand queries by day 90; #1 for "housebets" and "housebets login" by day 45. The incumbents are affiliates and a broken canonical, not entrenched media — that is why these targets are aggressive but sane.

---

## 4. Pages

Before/after for the home page and six more — full rows (issue, evidence, fix, title now/new, meta now/new, impact, effort) are in **`pages.csv`**; the reasoning per page:

**Home `/` — before:** title "Best Crypto Casino & Sportsbook | Housebets" (strong, keep), meta generic, canonical signals pointing at .dev, thin crawler extraction, no schema. **After:** self-canonical on the serving domain; meta gains the licence line ("…licensed crypto casino & sportsbook (Curaçao OGL/2024/431/0231): provably fair House Games, instant withdrawals, rakeback rewards"); the hero's on-chain bankroll proof ($2,850,275, Arkham-linked) echoed in crawlable text; Organization + WebSite schema. Job: rank for the brand set and carry the entity facts everything else cites.

**Rewards `/rewards` — before:** identical title/meta to the homepage; no crawlable statement of the model. **After:** title "Rewards: Rakeback, Lossback & Duel Mode | Housebets"; the canonical rewards statement (slider choice, Duel Mode's 50% house-edge return, XP, the $150,000 leaderboard, and an explicit line on which promo codes exist). Job: own "housebets bonus / promo code / rakeback" and end the affiliate fact vacuum (§3.2).

**Promotions `/promotions` — before:** meta advertising "deposit bonuses and free spins" the product doesn't offer; cards without terms; path absent from the official page inventory, overlapping /rewards. **After:** merge campaigns into /rewards (301 the loser) or keep /promotions strictly as time-boxed campaigns with accurate meta ("…including the $324,000 watch race — with full terms on every offer") and terms in crawlable text. One promotions home, one truthful meta.

**About us `/about-us` — before:** homepage's title/meta, empty H1, no usable facts. **After:** the review-grade fact page — H1 "About Housebets"; operator Bridge Technologies B.V.; licence OGL/2024/431/0231 with a register link; a dated timeline (ending the 2023/2024/2025 launch-year chaos in §5); the $HBTS / House Keys licence-scope note; Resolvo dispute resolution; responsible-gambling links; FAQ schema in the exact question forms of §5. Job: own "housebets review / legit / scam" and become the page AI engines cite.

**Provably fair `/provably-fair` — before:** the site's best page (unique title, a meta that explains server seed + client seed + nonce) — but unlinked from the House Originals journey and without a FAQ block. **After:** keep the core; title extended to "Provably Fair Casino Games — Verify Every Bet | Housebets"; FAQ in the forms engines answer ("how to verify provably fair bets"); HowTo schema; internal links from every House Original page. Job: own the provably-fair cluster.

**Sportsbook `/sports` — before:** accurate title/meta, zero SERP presence; reviewers call the book second-tier and one claims it doesn't exist. **After:** title "Crypto Sportsbook: Bet Football, NBA & More | Housebets"; depth on markets/leagues, the shared casino–sports balance, cash-out and payout speed; internal links from / and /casino; corrected facts fed to review partners (§3.3). Job: own "housebets sportsbook", enter "crypto sportsbook".

**Casino `/casino` — before:** generic meta ("Play the best crypto casino games at Housebets") targeting nothing. **After:** the category landing — House Originals + providers, provably fair, rewards model, payout speed — written to be cited by the comparison listicles that own "crypto casino" today; title "Crypto Casino: Slots, Live Casino & House Originals | Housebets".

**Plus one new page — `/payments`:** the withdrawal/deposit fact page (assets, minimums, processing reality, when KYC is and isn't required — one consistent policy, §1.6) carrying a **dated payout-speed test log that Housebets runs and publishes itself**. Third parties currently own this story (casino.clinic's 1-minute $5,500 test); an official, dated log is both a ranking asset and the most citable trust artefact the brand can produce (§5).

Internal-linking rule across all of the above: every money page links to /provably-fair and /about-us (the proof pages), and the fact pages link back to the money pages — the citation loop §5 depends on.

---

## 5. AI search

**Method note (read first).** The samples in `ai-answers.csv` were collected 13 Nov 2026 as **search-surfaced source-pool samples**: the ranked sources and claims that answer engines (ChatGPT, Perplexity, Gemini, Google AI Overviews) draw on for these questions. No AI-overview block was surfaced by the sampling tool and no direct engine sessions were run for this entry — claiming otherwise would be fabrication. The csv records, per question, what an engine would have to build its answer from, whether Housebets is mentioned, and whether that pool is correct. A week-1 task in §6 is the direct-session baseline (50 questions × 4 engines, logged in the same csv format) so movement becomes measurable.

**What the pool says today:**
- "Best crypto casino" and "which crypto casino has instant withdrawals" — **Housebets is never mentioned.** The brand is not in the consideration set engines assemble, despite holding the exact claim (instant withdrawals) the second question asks about.
- "Is Housebets legit / safe" — answers would be built **entirely from third parties** and would inherit the KYC-after-winning narrative, the ~9,228 USDC case, WagerDB's false "no sportsbook", and a trust-score spread (7.7 vs 62.5 vs 80.5) no engine can reconcile.
- Bonus and launch-year questions have **no correct answer available** in the pool: four versions of each circulate, including one wrong version (deposit bonuses/free spins) published in Housebets' own /promotions meta.

**How Housebets becomes the source they cite:**
1. **Publish the citable facts once, officially** — /about-us and the /payments test log (§4) are written as quotable blocks (licence line, dated timeline, payout table); engines cite pages that state facts in extractable form.
2. **Structured data** — Organization + FAQ schema carrying the same facts as the visible text, so machine readers get one version.
3. **Fix the top of the citation graph** — the review set (WagerDB, cryptotips, AskGamblers, casino.clinic) is what engines cite most; the §3.3 corrections round is therefore also the AI-search fix.
4. **Earn listicle inclusion** — /casino plus the payout data give comparison sites something to cite for the category questions; engines follow.
5. **Measure honestly** — monthly re-runs of the 50-question baseline in the same csv; report mention rate and correctness rate, not impressions of progress.

---

## 6. Your first 90 days

Costs are planning figures in USD at blended contractor rates (SEO lead ~$75/h, developer ~$60/h, writer ~$40/h), stated so the arithmetic can be argued with. Team: 1 SEO lead (part-time), 1 developer (part-time), 1 writer (part-time) — or the monthly-contract equivalent this contest offers its best entrants. Tools: Search Console and server logs (free), a rank tracker (~$50/mo), PageSpeed/CrUX (free) — tooling is inside the monthly totals below.

### Days 1–30 — Foundation: stop pointing Google at a login page
| Work | Owner | Cost |
|---|---|---|
| Verified-Googlebot log audit + Search Console crawl inspection (F4) | SEO lead | $600 |
| CWV baseline: CrUX + lab runs per template, phone first (§1.3) | SEO + Dev | $450 |
| Canonical/hreflang/og:url fix; .dev removed from all public signals, all 20 languages (F1) | Dev | $1,200 |
| Sitemap rebuild on .com; resubmit in GSC (F2) | Dev | $600 |
| .win → .com 301 consolidation (F3) | Dev + SEO | $900 |
| GSC query export; keywords.csv v2 with real volumes/difficulty | SEO lead | $600 |
| AI-search direct baseline: 50 questions × 4 engines (ai-answers format) | SEO lead | $750 |
| /rewards rebuild + /promotions merge decision and accurate meta (§4) | Writer + Dev | $1,350 |
| Trustpilot claimed and answered; trademark complaints filed for login lookalikes (§3.1) | SEO lead | $450 |
| **Month 1 total** | | **≈ $6,900** |

### Days 31–60 — Money pages: own the brand, open the categories
| Work | Owner | Cost |
|---|---|---|
| /about-us fact page (§4) | Writer + SEO | $1,100 |
| /payments with the self-run dated payout-speed test log | SEO + Writer | $1,300 |
| /casino category rebuild + /provably-fair FAQ/linking upgrade (§4) | Writer + Dev | $2,400 |
| /sports depth rebuild | Writer + Dev | $1,200 |
| Corrections round: WagerDB, AskGamblers, cryptotips (§3.3) | SEO lead | $750 |
| Affiliate approved-copy feed + programme terms update (§3.2) | SEO lead | $600 |
| Organization/FAQ/HowTo schema across fact pages | Dev | $600 |
| **Month 2 total** | | **≈ $7,950** |

### Days 61–90 — Growth: game pages, citations, category push
| Work | Owner | Cost |
|---|---|---|
| Game landing pages from the catalog sitemaps, first 30 titles | Writer + Dev | $2,600 |
| Digital PR: payout-data story + provably-fair explainer pitched to the listicle set that owns the category SERPs | SEO lead | $1,500 |
| Review-placement programme: Housebets into the 10 comparison lists engines cite | SEO lead | $1,200 |
| Locale validation: 20 language subtrees post-canonical-fix; per-language keyword maps v1 for the top 3 markets | SEO + Dev | $900 |
| AI-search re-measurement against baseline + plan v2 | SEO lead | $600 |
| **Month 3 total** | | **≈ $6,800** |

**90-day total ≈ $21,650.**

### What we should see — and how it is measured
- **Brand ownership** (GSC + weekly SERP sample of the 10 tracked queries): official top-3 for 10/10 by day 90; #1 for "housebets" and "housebets login" by day 45.
- **Index health** (GSC coverage + sitemap audit): 100% of sitemap URLs on the canonical .com host; zero sitemap/canonical/hreflang references to .dev; verified-Googlebot crawl errors at zero on money pages.
- **AI answers** (50-question baseline, re-run monthly): Housebets mentioned in ≥50% of category answers by day 90 (from 0% in this audit's samples); bonus and launch-year facts correct in ≥80% of brand answers.
- **Traffic** (GSC): first measurable non-brand organic clicks from the provably-fair and payout clusters by day 90. No absolute visit number is quoted — without the GSC baseline any figure would be invented; the week-1 export sets the baseline and the day-90 review judges against it.
- **Web Vitals** (CrUX via GSC): templates at or better than LCP 2.5 s / INP 200 ms / CLS 0.1 by day 90, or a costed remediation plan where a template cannot get there.

---

## About you

This report was produced by an independent entrant whose background is operating and auditing live web3 products — reading contracts, explorers and production sites the way this audit reads Housebets: fetch first, quote only what the fetch returned, label what could not be verified. The track record offered is this document and its evidence files: every claim in it can be re-checked in minutes, which is the working style the monthly contract would be buying.

- Public work / code: github.com/fairusfelles
- CV / LinkedIn: available on request

**Tools used** (named per the contest rules): Muse (AI assistant) for research and drafting; public web sources as cited throughout the evidence files. No templates, no copied entries.
