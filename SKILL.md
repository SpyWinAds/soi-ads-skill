---
name: spywinads-soi-ads
description: Full competitor analysis of Facebook / Instagram video ads with real SpyWinAds data — each video's real age (the day the file first hit Meta), how many stores run the same video, what a store / Page / product is pushing, a ranking of videos worth testing, Hook · Offer · CTA, 3 test angles and a Shopify-import CSV. Use when the user wants an analysis rather than a single lookup: several stores or Pages, "which videos should I test", "ad angles", "winning product research", "spy on competitor Facebook ads", "soi ads đối thủ", "phân tích quảng cáo đối thủ", "tìm sản phẩm win", "góc test quảng cáo" — even if SpyWinAds is not mentioned. For a single quick lookup the narrower skills (spywinads-store-spy, spywinads-page-lookup, spywinads-video-age, spywinads-product-clone) fit better if they are installed.
compatibility: Needs the SpyWinAds MCP connection (https://spywinads.com/mcp, free account, 30 credits a day). Runs on Claude.ai, Claude Code, ChatGPT (Plus and up), Cursor, Codex.
license: MIT
---

# Competitor ad analysis with SpyWinAds

Data comes from the **SpyWinAds MCP**: a library of Meta video ads shown in the US, scanned by SpyWinAds users. Two things set it
apart from Ad Library scrapers, and they are what users most need to hear clearly:

- **Real video age** = the day the video file was first uploaded to Meta. The ad start date resets each time a store pauses and
  restarts an ad; the file date does not.
- **Video fingerprints**: the same video running in many stores is recognised as one → you can count *how many different stores*
  run it.

Answer in the user's language, including table headers.

## 0. Check the connection first

No tool named `spywin_*` available → the user has not connected SpyWinAds yet. Do not guess numbers or analyse from memory: the user
will trust those numbers. Say in one line what they get once connected (real video age and how many stores run each video, straight
from the library), give the setup steps for where they are (section **Setup** at the end), and wait until they are connected.

Each SpyWinAds tool call uses 1 credit (free: 30 a day, paid plans: 100), so **batch the work**: call the right tool once per input,
never twice for the same input. If a tool says out of credits or not signed in, repeat that message and link
`https://spywinads.com/developers`.

## 1. Input → tool

| The user gives | Call | You get |
|---|---|---|
| Store domain (`shop.com`) | `spywin_spy_store_ads` | Live ads, new ads in 7 days, top videos, top products |
| Facebook Page link / ID | `spywin_get_page_ads` | The Page's videos + the stores it sends traffic to |
| Ad Library link / ad ID | `spywin_lookup_video_ad` (`ad`) | Real age, stores running / that ran it, earliest stores in the library |
| A `video_id` from another result | `spywin_lookup_video_ad` (`video_id`) | Same as above, no need to ask for an ad link (older connectors without `video_id`: ask for the Ad Library link) |
| Shopify product link | `spywin_export_shopify_product` | Title, prices, variants, images, Shopify CSV link (24 hours), the product's video ads |
| Product keyword | `spywin_queue_keyword_scan` → `spywin_get_scan_status` → `spywin_get_scan_results` | The SpyWinAds app on the user's computer runs the scan |
| Their own tabs (Win, Potential, Scale…) | `spywin_get_my_overview` · `spywin_list_my_videos` · `spywin_get_video_history` | Their own account data |

**Keywords** run through the SpyWinAds desktop app on the user's computer (their home connection), not the server. Read the `status`
the tool returns: `cached` → read the results now · `queued` → say it is scanning, check `spywin_get_scan_status` a few minutes later ·
`app_off` / `no_app` / `fb_paused` → give exactly the next step the tool states (open the app, install it from spywinads.com, wait until
time X). Never promise a finish time yourself. No scan tool available → say keyword search needs the SpyWinAds app, and ask for a
store / Page / ad link to analyse right away.

**Not in the library** (`found: false`) → call `spywin_queue_page_scan` only if it is in your tool list (ask one short question first,
`kind` = page / store / ad), follow with `spywin_get_scan_status(request_id=…)`, then look up again. Not in your tool list → give the
free scan link the tool returned.

## 2. Read the numbers correctly

Most analysis mistakes happen here, so read this before concluding anything:

- **Video age** = `video_first_on_meta`, shown as date + days counted from today ("22 Sep (52 days)"). Empty → the file date is
  unknown: give `first_ad` and say it is the first ad date, not the video age.
- **Scope**: `live_ads` counts only the store / Page / product you looked up (whole library for a single-video lookup).
  `stores_running_now`, `stores_ever` and `pages_running_now` count every store in the library. A paused store can show a video
  that other stores still run: say so, it is not a contradiction. Nightly history (`spywin_get_video_history`) counts per night and
  can differ from these live counts.
- **Group brands by the landing-page domain** (`landing_page`), never by ad copy. Two stores with the same hook are still two brands.
- The library only sees a **minimum count of ads observed**: no spend, reach, orders or revenue. "Long run", "many ads", "many
  stores" mean *being invested in or copied*, not profitable: a big brand can run at a loss to buy customers, a store can pour
  hundreds of ads into a test and drop it. Say "worth a look because…", never "profitable", "winner", "making sales".
- A product name that looks like a code is the store's URL handle: say it is not readable and give the landing page.

## 3. Rank the videos worth testing

Score each video and explain it with numbers:

- **Copied widely** (`stores_running_now` ≥ 3, or `stores_ever` ≥ 5): several stores pay to run the same video → the product is being
  tested broadly.
- **Fresh and live** (video age ≤ 30 days, `live_ads` > 0): being pushed, not saturated yet.
- **Old video coming back**: video age > 60 days but a recent first ad → someone is reusing an old video.
- **One store pouring in ads**: heavy testing / ad-set splitting, not proof of sales.

Suggested score for ordering (say it is an estimate): `3 × stores running now + min(live ads, 50) / 5 + (age ≤ 14 days: 5 · ≤ 30: 3 · else: 0)`.

## 4. Hook · Offer · CTA

The tools return the product name and landing page, not the video itself. So:

1. **Offer** — call `spywin_export_shopify_product` on the landing page of the top 1–2 videos (it reads shops that block ordinary web
   readers): titles often carry urgency ("Last Day – 60% OFF"), price, variants, and the product's other video ads. If you can read
   web pages, read the landing page too: bundles, free shipping, warranty, gifts.
2. With a browser / web reader: open the Ad Library link to get the ad text → **Hook** (first line), **CTA**, **format**.
3. Cannot read it → write "not read". Users shoot real videos from this table; an invented hook costs them real test money.

## 5. Report

Use this layout (in the user's language):

**Summary** — 2–3 sentences: what the competitor pushes, how hard (live ads, stores, freshness).

| # | Product | Brand (domain) | Video age | Stores running now / ever | Live ads | Hook | Offer | CTA · Format | Why it is worth a look |
|---|---|---|---|---|---|---|---|---|---|

**Common pattern** — same kind of offer, hook angle, season.
**Gaps** — angles nobody uses yet (audience, pain point, different offer); say these are suggestions.
**3 test angles** — 2–3 lines each: first-line hook · offer · format · audience.
**Limits** — one line: counts are minimum ads observed, no spend / revenue.
**Next step** — offer in plain words and call the tool yourself if the user picks one: the other stores running the top video
(`spywin_lookup_video_ad`) · a Shopify CSV (`spywin_export_shopify_product`) · the SpyWinAds app (`https://spywinads.com`) for daily
keyword scans and Win / Potential / Scale tabs that update themselves.

## Limits of this skill

It only reads SpyWinAds data: it does not scrape Facebook and never logs in to the user's Facebook account.

## Setup (give this to users who are not connected)

Sign-up happens at the login step: free, 30 credits a day, no card.

- **Claude.ai** (every plan incl. Free, 1 custom connector): Customize → Connectors → + → Add custom connector → URL
  `https://spywinads.com/mcp` → Connect → sign in → Allow.
- **ChatGPT** (Plus / Pro / Business; Free cannot add custom MCP yet, suggest Claude.ai Free): Settings → Apps & Connectors → Advanced →
  Developer mode → Create → URL `https://spywinads.com/mcp`, OAuth → sign in → Allow.
- **Claude Code**: `claude mcp add --transport http spywinads https://spywinads.com/mcp`, then type `/mcp`, pick spywinads and finish
  the sign-in in the browser (adding it alone does not connect it).
- **Cursor / Codex / scripts**: API key at `https://spywinads.com/developers`, header `Authorization: Bearer <key>` (never in the URL).
