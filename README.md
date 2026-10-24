# zillow scraper tool: what actually works on Zillow's anti-bot wall, what each listing really costs, and when a general API beats a dedicated one

Zillow removed most of the easy answers. Its official API stopped accepting new registrations years ago, Bridge Interactive access is limited to MLS members and licensed brokers, and the Zestimate API is no longer publicly available at all. So when people search for a zillow scraper tool today, they're really asking three things: what still works, how much does each page of data cost, and will the whole thing fall apart on page 500. This article answers all three with current numbers.

Quick answers first: Zillow tracks over 110 million US properties and protects them with Imperva's WAF, which puts it in the same difficulty tier as LinkedIn and Google. Generic scraping approaches die fast here — datacenter IPs get blocked almost immediately, and pages render as empty shells without JavaScript execution. That's why the realistic options in 2026 are (a) a scraping API that handles rendering, residential proxies, and anti-bot bypass for you, or (b) a dedicated Zillow API that returns pre-parsed JSON at a much lower per-listing price.

> **Quick picks:**
> - Need Zillow plus other sites (Amazon, Google, etc.), or custom data handling? A general scraping API like **ScraperAPI** is the flexible pick. Its Hobby plan is $49/month with 100,000 credits, and an independent benchmark recorded a **100% success rate on Zillow**. There's a 7-day trial with 5,000 credits that needs no credit card.
> - Only ever need Zillow data, in fixed JSON format, at low volume? Dedicated Zillow APIs (covered in the comparison below) cost 10–100x less per listing.
>
> 👉 [Try ScraperAPI free — 5,000 credits, no credit card](https://www.scraperapi.com/?fp_ref=coupons)

## Why scraping is the only remaining route into Zillow data

It wasn't always this way. Zillow launched a public API in the mid-2000s and for years developers could pull property data, Zestimates, and listing details with a simple key. That access shut down gradually: new registrations closed, key endpoints were deprecated, and what survived got funneled to a narrow set of partners through the Bridge Interactive platform. MLS members and licensed brokers can still qualify; individual developers and proptech startups mostly can't. Listing data now flows to Zillow Premier Agent partners as part of lead-generation arrangements, not as a general data API.

That leaves scraping — and Zillow has responded accordingly. The site runs Imperva (formerly Incapsula), one of the more sophisticated commercial bot-detection products, operating on multiple layers at once: network-level traffic analysis, TLS fingerprinting, HTTP header profiling, and JavaScript-based browser fingerprinting through its client-side sensor. In the ScrapeOps benchmark suite, Zillow scores high enough to sit in the "hard" tier alongside LinkedIn and Google.

Practically, this shapes what any zillow scraper tool must do:

- **Render JavaScript.** Zillow serves pages built on Next.js; without headless browser rendering you get empty shells, not listings.
- **Use US residential IPs.** Zillow serves all inventory (including Canadian MLS feeds) from zillow.com behind US IPs, and datacenter addresses get flagged fast.
- **Throttle politely.** Even proxied requests can trigger rate limiting; a couple of seconds between page requests is the conservative baseline.

One more thing worth knowing before you write any code: Zillow's current Terms of Use explicitly prohibit automated queries, screen scraping, and bypassing protective measures. The legal picture is a genuine gray area — the Ninth Circuit's hiQ v. LinkedIn ruling established that scraping publicly accessible pages doesn't violate the federal CFAA, but Zillow has real remedies short of court: cease-and-desist letters, IP bans, and civil claims against commercial operations. People do this at scale; nobody should pretend it's sanctioned.

## What ScraperAPI brings to a Zillow project

ScraperAPI is a general-purpose scraping API founded in 2018, now serving over 10,000 companies and processing roughly 36 billion API requests a month. You send it a URL, it rotates through a pool of 40M+ residential and datacenter IPs across 50+ countries, renders JavaScript, solves CAPTCHAs, retries failures, and returns HTML or parsed JSON. It's not Zillow-specific, which cuts both ways — I'll get to that.

The Zillow-specific evidence is solid. In a benchmark of 16 scraping APIs run against seven of the hardest domains (Amazon, Indeed, GitHub, Zillow, Capterra, Google, X), ScraperAPI posted a 72.57% average success rate — which looks weak until you see the per-domain split. On Instagram it scored zero. On Amazon, 98%. On Zillow: a perfect 100%, with a 5.6-second average response time that beat most competitors. A general API with mediocre averages can still be the right tool when your target site happens to be one it handles well. Zillow is one of those sites for ScraperAPI.

The standard recipe for Zillow requests, per the implementation guides that have been validated against the live site, uses `render: true` (non-negotiable), `premium: true` (residential IPs), a 90-second timeout to account for render plus Imperva challenge time, and an inter-request delay around 2 seconds.

All of this comes with the plan you buy — every tier includes JS rendering, premium proxies, JSON auto-parsing, CAPTCHA handling, custom sessions, unlimited bandwidth, and a 99.9% uptime guarantee. Billing only counts successful requests (HTTP 200 and 404), and each response includes a `sa-credit-cost` header telling you exactly what that request consumed.

## Full plan and pricing breakdown

Here's the complete lineup from ScraperAPI's pricing page. Annual billing knocks 10% off across the board.

| Plan | Monthly | Annual (per mo) | Credits | Concurrency | Get started |
| --- | --- | --- | --- | --- | --- |
| Free | $0 | — | 1,000/mo | 5 | [ Start free](https://www.scraperapi.com/?fp_ref=coupons) |
| Hobby | $49 | $44.10 | 100,000 | 20 | [ View Hobby plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Startup | $149 | $134.10 | 1,000,000 | 50 | [ View Startup plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Business | $299 | $269.10 | 3,000,000 | 100 | [ View Business plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Scaling | $475 | $427.50 | 5,000,000 | 200 | [ View Scaling plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Professional | $975 | $877.50 | 10,500,000 | 300 | [ View Professional plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Advanced | $1,975 | $1,777.50 | 21,500,000 | 500 | [ View Advanced plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Enterprise | Custom | Custom | 22,000,000+ | 500+ | Contact sales |

A few details the table can't show:

- **Geotargeting is capped below Business.** Hobby and Startup can only target US and EU. If your Zillow work is US-based, that's fine — but if you later need Japan, Brazil, or Australia geotargeting, no amount of configuration gets it on the cheaper plans.
- **PAYG (pay-as-you-go overrun) starts at Scaling.** On Hobby through Business, running out of credits mid-month means scraping stops until the next cycle. Credits don't roll over either. Hobby, Startup, Business, Scaling, and Professional support opt-in auto-upgrade, which bumps you to the next plan automatically when credits run dry.
- **The trial and refund cushion.** New accounts get 5,000 credits for 7 days, no card required, plus a 7-day no-questions-asked refund after paying. Between those two, testing a Zillow workflow costs nothing.
- Professional and Advanced were added as "Growth" plans in May 2026 for teams that need multi-million credit volumes without going through enterprise sales.

## The real cost of a Zillow page: credit math most reviews skip

This is the part that trips people up, and it's the single most useful thing to understand before picking a plan. ScraperAPI charges credits per request, and the base rate of 1 credit applies only to the simplest pages. Everything about Zillow pushes that number up:

| Cost component | Credits | Why |
| --- | --- | --- |
| Base request | 1 | Any page |
| `render=true` | +10 | Required — Zillow pages are empty without JS |
| `premium=true` | +10 | Required — datacenter IPs get blocked by Imperva |
| Possible anti-bot bypass | +10 | Auto-added when protection is detected and bypassed |

So a realistic planning figure for a Zillow request is **around 21 credits**, potentially 31 if the bypass surcharge kicks in on a given request. That's an estimate built from ScraperAPI's published parameter costs plus Zillow's known requirements — the exact per-domain multiplier for zillow.com isn't published, so before committing to a plan, use the Domain Cost Estimator in the dashboard (or the `urlcost` API endpoint) to get the exact figure for your actual URLs. You can also set a `max_cost` per request so a single expensive scrape can't blow past your limit.

What does that buy you? Rough arithmetic on the Hobby plan's 100,000 credits: about 4,700 Zillow pages per month at 21 credits each — call it $10–11 per 1,000 listings if the estimate holds. The free plan's 1,000 credits gets you roughly 47 test requests, enough to validate a workflow but not much more.

Two quirks worth knowing: combining features costs more than the sum of parts (`premium` + `render` is +25, not +20; `ultra_premium` + `render` is +75), and requests you cancel before the 70-second window completes still get billed. The `sa-credit-cost` response header on every request is your friend here — log it and you'll never be surprised by a bill.

For context on whether those numbers are good: a 2026 benchmark testing seven proxy and scraping services against Zillow found costs ranging from $210 to $850 per 1,000 pages across the industry. If ScraperAPI's Zillow cost lands anywhere near the low end of its estimated range, it's competitive for a general tool.

👉 [See current ScraperAPI plan pricing and start the 7-day trial](https://www.scraperapi.com/pricing/?fp_ref=coupons)

## Dedicated Zillow APIs vs. a general scraper tool

Honest answer to the question everyone actually has: for Zillow-only projects at moderate volume, a dedicated Zillow API is dramatically cheaper. Here's the verified landscape:

| Option | Pricing model | Verified numbers |
| --- | --- | --- |
| omkarcloud's Zillow Scraper API | Per call | Free: 1,000 calls/mo. Starter $16/mo (20K calls, $0.80/1K). Grow $48/mo (100K, $0.48/1K). Scale $148/mo (400K, $0.37/1K) |
| ScrapeNow | Per returned row | From $0.04/credit, no monthly contract, failed rows not charged |
| Bright Data's Zillow scraper | Free script + enterprise API | Free small-scale scraper available; enterprise API is quote-based |
| Apify marketplace Zillow scrapers | Per actor usage | Multiple actors (search, ZIP code search, sold listings); several list ~41 listings per query cap |

The trade-offs are real, though. A dedicated API returns a fixed JSON schema — typically zpid, price, beds, baths, square footage, Zestimate, rent Zestimate, tax history, school ratings, agent names and license numbers, photos, and MLS identifiers. One sample listing from a dedicated API's documentation came back with 55 photos, a 1.18% tax rate, the agent's DRE license number, and both sale and tax history for a Beverly Hills property — genuinely rich data. But you get the fields they expose, the endpoints they maintain, and their update cadence. If the field you need isn't in the schema, you're stuck.

A general tool like ScraperAPI costs more per page but handles any URL on any site: a Zillow property page today, a county assessor site tomorrow, an Amazon product page the day after. You control parsing yourself and can chase data no prebuilt API covers. If Zillow is 20% of your scraping volume and the rest is scattered across other sites, the general API wins on total cost. If it's 100% of your volume, do the multiplication first.

## Is scraping Zillow legal?

Short version: extracting publicly available data is permitted under US federal law per the hiQ v. LinkedIn precedent, but it violates Zillow's Terms of Use, which expressly prohibit automated queries and bypassing access restrictions. The practical risks are IP bans, cease-and-desist letters, and — particularly for commercial operations — potential civil litigation. Rotating proxies doesn't create authorization; it just makes you harder to block. Commercial users in regulated industries (lending, brokerage) should treat this as a compliance question, not a technical one, and consider licensed data providers where contracts matter.

## FAQ

**How many listings can I get per month on the free plan?** The free tier gives 1,000 credits monthly with 5 concurrent connections. At an estimated ~21 credits per Zillow request, that's roughly 40–45 pages — enough to test your parser, nowhere near enough for a market study.

**Does ScraperAPI return Zillow data as JSON automatically?** It has auto-parsing (`autoparse=true`, which costs no extra credits) for supported structured endpoints, but Zillow isn't a documented structured-data domain like Amazon or Google. Plan to parse the HTML yourself, or check whether an auto-parse template exists for your specific page type using the API Playground before committing.

**What happens when I hit my credit limit?** On Hobby through Business, scraping pauses until the next billing cycle. Enable auto-upgrade (Hobby through Professional) to move up a plan automatically, or use pay-as-you-go on Scaling and above to keep going at a fixed per-credit rate under a spending cap you set.

**Is there a refund?** Yes — 7 days, no questions asked, on top of the 7-day free trial that requires no credit card.

## Which zillow scraper tool should you actually pick

Run your numbers against these rules of thumb:

- **Zillow is your only target, under ~100K listings/month, fixed fields are fine** → a dedicated Zillow API costs you $16–148/month versus hundreds more on a general tool. Take the dedicated route.
- **Zillow plus other sites, or custom parsing needs** → ScraperAPI's Hobby plan ($49, 100K credits, roughly 4,700 estimated Zillow pages) is the entry point, and its 100% Zillow success rate in independent testing removes the biggest risk with general APIs.
- **Serious volume, multiple markets, scheduled jobs** → Scaling ($475, 5M credits) is where pay-as-you-go overage unlocks, so a heavy month doesn't hard-stop your pipeline. DataPipeline is available too, but note it bills at 6 credits per basic request rather than 1.
- **Just curious or prototyping** → don't pay anything yet. The 7-day trial's 5,000 credits will validate your whole workflow.

Before you commit to any paid plan, do one thing: run a handful of your actual Zillow URLs through the dashboard's cost estimator and note the `sa-credit-cost` values. Thirty seconds of checking turns the credit math from an estimate into a fact, and it's the difference between a plan that fits and one that quietly eats your budget.

👉 [Check ScraperAPI's plans and free trial options](https://www.scraperapi.com/?fp_ref=coupons)
