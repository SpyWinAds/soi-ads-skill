# Competitor ad analysis — spy on Facebook video ads with SpyWinAds (free skill)

Free skill for **Claude, ChatGPT, Claude Code, Codex, Cursor**, powered by real SpyWinAds data. No Facebook scraping, no Facebook login,
no Meta API key.
Skill miễn phí: **Soi ads đối thủ đầy đủ (xếp hạng video, Hook · Offer · CTA, 3 góc test)** bằng dữ liệu thật của SpyWinAds. Không cào Facebook, không đăng nhập Facebook.

Free web tools, no AI needed: https://spywinads.com

## What it does

| You give | You get |
|---|---|
| A store domain `shop.com` | Live ads, new ads in 7 days, top videos, products it pushes |
| An Ad Library link | **Real age** of the video (day the file first hit Meta) + **how many stores** run it |
| A Facebook Page link | The Page's videos + the stores it sends traffic to |
| A Shopify product link | Title, price, variants, images + **Shopify-import CSV** |
| A keyword | The SpyWinAds desktop app scans on your computer, then the AI reads the results (needs the app) |

Then: ranking of videos worth testing · Hook · Offer · CTA · 3 concrete test angles.

## How it differs from Ad Library scraper skills

| | Ad Library scraper skills | This skill |
|---|---|---|
| Ad age | Days the ad has run — resets when a store pauses and restarts | **Real video age**: day the file first hit Meta |
| Signal | Number of variants of **one** shop | **How many different stores** run the same video (video fingerprints) |
| Data | Browser automation — breaks when Facebook changes its layout, blocked by IP | SpyWinAds library over MCP, ready |
| After the analysis | Test angles | Test angles + **Shopify CSV** + the stores running the same video |

## Setup (3 minutes, free)

Two parts, you need both: **connect the SpyWinAds MCP** (the data) and **add this skill** (the instructions). The MCP sign-in step creates a
free SpyWinAds account: 30 credits a day, no card. No GitHub account needed. Then try: `spy on the Facebook ads of achandise.com and give me 3 ad angles to test`

| You use | Works? |
|---|---|
| Claude.ai, any plan incl. Free | ✅ |
| ChatGPT Plus / Pro / Business | ✅ |
| ChatGPT Free | ❌ cannot add MCP yet → use Claude.ai Free |
| Claude Code · Codex · Cursor | ✅ |

### Claude.ai (web, desktop, mobile), works on **Free**
Custom connectors are available on Free (one connector) and Skills too
([connectors](https://support.claude.com/en/articles/11175166-getting-started-with-custom-connectors-using-remote-mcp),
[skills](https://support.claude.com/en/articles/12512180-using-skills-in-claude)).
1. **Connect:** Customize → Connectors → **+** → **Add custom connector** → name `SpyWinAds`, URL `https://spywinads.com/mcp` → Add →
   **Connect** → sign in or create a SpyWinAds account → **Allow**.
2. **Add the skill:** this repo *is* the skill (SKILL.md at the root), so the zip uploads as is, **no unzipping**:
   - **Code → Download ZIP** on this page, or
   - direct link: https://github.com/SpyWinAds/soi-ads-skill/releases/latest/download/spywinads-soi-ads.zip

   → Customize → **Skills** → **+** → Create skill → **Upload a skill** → pick the zip. Turn on Code execution (Settings → Capabilities)
   if Claude asks.
3. New chat: `spy on the Facebook ads of achandise.com and give me 3 ad angles to test`

### ChatGPT (Plus / Pro / Business)
1. Settings → Apps & Connectors → Advanced settings → **Developer mode** on → **Create** → name `SpyWinAds`, URL
   `https://spywinads.com/mcp`, Authentication **OAuth** → sign in → Allow.
2. Create a **Project** → Instructions → paste all of
   [SKILL.md](https://raw.githubusercontent.com/SpyWinAds/soi-ads-skill/main/SKILL.md).
3. In that Project, turn SpyWinAds on (**+** → Developer mode) and type `spy on the Facebook ads of achandise.com and give me 3 ad angles to test`.

ChatGPT Free cannot add a custom MCP yet: use Claude.ai Free (above), or the free web tool https://spywinads.com.

### Claude Code
```
claude mcp add --transport http spywinads https://spywinads.com/mcp
git clone https://github.com/SpyWinAds/soi-ads-skill ~/.claude/skills/spywinads-soi-ads
```
Then type `/mcp` → spywinads → finish the sign-in in the browser (adding it alone does not connect it).

### Codex / Cursor / other agents
Free API key at https://spywinads.com/developers → MCP `https://spywinads.com/mcp`, header `Authorization: Bearer <key>` (never in the
URL). Paste SKILL.md into `AGENTS.md` / your rules.

## More SpyWinAds skills

| Skill | For |
|---|---|
| [spywinads-soi-ads](https://github.com/SpyWinAds/soi-ads-skill) | Full competitor analysis: ranking, Hook · Offer · CTA, 3 test angles |
| [spywinads-store-spy](https://github.com/SpyWinAds/shopify-store-spy-skill) | A store's video ads, scaling or testing |
| [spywinads-page-lookup](https://github.com/SpyWinAds/facebook-page-ads-skill) | A Facebook Page's video ads and the stores behind it |
| [spywinads-video-age](https://github.com/SpyWinAds/video-ad-age-skill) | Real age of an ad video, who else runs it |
| [spywinads-product-clone](https://github.com/SpyWinAds/shopify-product-clone-skill) | Any Shopify product → Shopify CSV |

## Limits (plainly)

Free: 30 credits a day (1 tool call = 1 credit; a full analysis usually needs 3–5). Paid plans: 100. Data = Meta ads shown in the US, scanned by
SpyWinAds users. Counts are the minimum ads observed: **no spend, reach, orders or revenue**. Many ads or many stores means tested or
copied, **not proven to sell**.

MIT license · SpyWinAds — https://spywinads.com
