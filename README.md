# Google Tag Manager MCP Starter Kit — Manage GTM with AI

[![MCP Compatible](https://img.shields.io/badge/MCP-compatible-blue)](https://modelcontextprotocol.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Platform: Google Tag Manager](https://img.shields.io/badge/Platform-GTM-4285F4)](https://tagmanager.google.com)

**Deploy tracking tags without touching your website's code.** Open this repo in Amp, Cursor, or VS Code and manage Google Tag Manager with AI — create tags, build triggers, install conversion pixels, and publish container changes.

---

## Why GTM + AI?

Google Tag Manager is the control center for every tracking pixel, conversion tag, and analytics snippet on your website. Instead of asking a developer to add code for every new ad platform, GTM lets marketers deploy tags through a visual interface.

The problem? GTM's interface is deceptively complex. Tags fire on triggers, triggers depend on variables, variables reference data layers — and one misconfigured trigger can break your entire analytics setup. The Preview mode is powerful but confusing. And the stakes are high: a broken conversion tag means your ad campaigns can't optimize.

An AI agent turns GTM management from a technical task into a conversation. "Add a Google Ads conversion tag for purchase events" becomes a single request instead of a 15-minute configuration exercise.

**Best for:** Marketers who can't edit website code directly, marketing teams managing multiple conversion pixels, agencies handling multiple client containers, anyone setting up tracking for ad campaigns.

---

## Quick Start (30 Seconds)

### Amp / Cursor / VS Code (Copilot)

1. **Get a free API key** at [syntermedia.ai/developer](https://syntermedia.ai/developer)
2. **Set the key:**
   ```bash
   export SYNTER_API_KEY=syn_your_key_here
   ```
3. **Open this repo** in your editor
4. **Start chatting** — MCP tools are pre-configured in `.mcp.json`

### Claude Desktop

Copy `claude_desktop_config.json` to your Claude config directory and replace the API key:

- **macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`

---

## What You Can Do

| Capability | Example |
|-----------|---------|
| **Create Conversion Tags** | "Add a Google Ads conversion tag for purchases with conversion ID AW-123456789" |
| **Build Triggers** | "Create a trigger that fires on form submissions on the /contact page" |
| **Install Pixels** | "Add the Meta Pixel, LinkedIn Insight Tag, and TikTok Pixel to GTM" |
| **List All Tags** | "What tags are currently in my GTM container?" |
| **Publish Changes** | "Publish the current workspace with version name 'Added conversion tracking'" |
| **Preview & Debug** | "Check if the purchase conversion tag is firing correctly" |
| **Custom HTML Tags** | "Add a custom JavaScript snippet that fires on all pages" |
| **Data Layer Events** | "Set up a data layer push for 'add_to_cart' events" |
| **Tag Auditing** | "Are there any duplicate or conflicting tags in my container?" |
| **Variable Creation** | "Create a variable that captures the page URL path" |

---

## Example Conversations

### 1. Setting Up Multi-Platform Conversion Tracking

> **You:** I'm about to launch campaigns on Google, Meta, and LinkedIn. I need conversion tracking for all three.
>
> **Agent:** I'll set up all three conversion pixels in your GTM container. Here's what I need:
>
> **For each platform, I'll create:**
> 1. A base pixel tag (fires on all pages)
> 2. A purchase/conversion event tag (fires on conversion)
> 3. A trigger for the conversion event
>
> **Google Ads conversion tracking:**
> - Tag: Google Ads Conversion Tracking
> - Trigger: Fires when `purchase` event appears in the data layer
> - Requires: Conversion ID (AW-XXXXXXXXX) and Conversion Label
>
> **Meta Pixel:**
> - Tag: Meta Pixel Base Code (fires on all pages — tracks PageView)
> - Tag: Meta Purchase Event (fires on conversion — tracks Purchase with value)
> - Requires: Pixel ID
>
> **LinkedIn Insight Tag:**
> - Tag: LinkedIn Insight Tag (fires on all pages)
> - Tag: LinkedIn Conversion Event (fires on conversion)
> - Requires: Partner ID and Conversion ID
>
> **You:** Here are the IDs: Google AW-123456789/AbcXyz, Meta Pixel 1234567890, LinkedIn Partner 4567890.
>
> **Agent:** All 6 tags created with appropriate triggers. I've also added:
> - A `Conversion Linker` tag for Google (required for cross-domain tracking)
> - All tags are firing on the `purchase` data layer event
>
> **⚠️ Before publishing:** Use GTM Preview mode to verify all tags fire correctly on a test purchase. Want me to publish now or after you preview?

### 2. Debugging a Broken Conversion Tag

> **You:** Google Ads says I have 0 conversions this week but I know people are buying. What's wrong?
>
> **Agent:** Let me audit your GTM container for the Google Ads conversion tag...
>
> **Found the issue.** Your conversion tag has two problems:
>
> 1. **Trigger mismatch:** The tag fires on a `purchase_complete` custom event, but your website pushes a `purchase` event to the data layer. The tag never fires because it's listening for the wrong event name.
>
> 2. **Missing Conversion Linker:** You don't have a Google Ads Conversion Linker tag. Without it, Google can't connect the ad click to the conversion (especially after iOS changes).
>
> **Fixes I'll apply:**
> 1. Update the trigger to listen for `purchase` (not `purchase_complete`)
> 2. Add a Conversion Linker tag that fires on all pages
> 3. Verify the conversion value variable is pulling the correct revenue number
>
> After these fixes, conversions should start appearing in Google Ads within 24-48 hours. The historical data is lost, but going forward tracking will work correctly.
>
> Want me to apply and publish?

### 3. Installing Multiple Pixels Without Conflicts

> **You:** I need Meta Pixel, Reddit Pixel, TikTok Pixel, Google Ads, and LinkedIn Insight Tag. Won't they conflict?
>
> **Agent:** No — tracking pixels don't conflict with each other. Each platform's pixel operates independently. But poor GTM organization can create issues. Here's my approach:
>
> **Container organization:**
>
> | Folder | Tags |
> |---|---|
> | Google | GA4 Config, Google Ads Conversion, Conversion Linker |
> | Meta | Meta Base Pixel, Meta Purchase Event, Meta ViewContent |
> | LinkedIn | Insight Tag, LinkedIn Conversion |
> | Reddit | Reddit Pixel, Reddit PageVisit, Reddit SignUp |
> | TikTok | TikTok Base Pixel, TikTok Purchase, TikTok AddToCart |
>
> **Shared triggers:**
> - "All Pages" trigger (for base pixels)
> - "Purchase" trigger (for conversion events)
> - "Signup" trigger (for lead events)
>
> **Best practice:** Each platform gets its own folder in GTM. Shared triggers avoid duplication. Each platform's conversion tag fires on the same trigger — so one `purchase` data layer push notifies all platforms simultaneously.
>
> **Performance note:** 5 pixels won't noticeably slow your site. Each adds ~20-50ms of load time. The total impact (~100-250ms) is minimal. If you ever need to optimize, use GTM's tag sequencing to load non-critical pixels after the page renders.

### 4. Custom Event Tracking for SaaS

> **You:** I want to track when users click the "Start Free Trial" button on our pricing page.
>
> **Agent:** I'll set up a click-based trigger and data layer event. Two approaches:
>
> **Option A: GTM Click Trigger (no code changes)**
> - Create a "Click — Start Free Trial" trigger
> - Condition: Click Element matches CSS selector `#start-trial-btn` (or whatever your button's ID/class is)
> - Fire on: Click URL contains "/pricing"
> - Pro: No developer needed. Con: Relies on CSS selectors which can break if the page changes.
>
> **Option B: Data Layer Push (recommended)**
> - Ask your developer to add this code to the button click handler:
>   ```javascript
>   dataLayer.push({ event: 'start_free_trial', plan_type: 'pro' });
>   ```
> - Create a Custom Event trigger for `start_free_trial`
> - This is more reliable because it's explicit — the event fires when and only when the right action occurs.
>
> **Then I'll create:**
> 1. A GA4 Event tag sending `start_free_trial` as a custom event with `plan_type` parameter
> 2. A Google Ads Conversion tag for trial starts (so campaigns can optimize for trials, not just clicks)
> 3. A Meta Custom Event tag for the same action
>
> Which approach do you prefer?

### 5. Container Audit and Cleanup

> **You:** I inherited a GTM container from the previous marketing team. Can you audit it?
>
> **Agent:** Let me pull all tags, triggers, and variables...
>
> **Container audit results:**
>
> 📊 **Overview:** 47 tags, 31 triggers, 28 variables
>
> ⚠️ **Issues found:**
>
> | Issue | Count | Impact |
> |---|---|---|
> | Duplicate tags (same pixel, different names) | 4 | Double-counting conversions |
> | Tags with no trigger | 3 | Dead code, no impact |
> | Triggers referencing deleted tags | 2 | Orphaned, no impact |
> | Custom HTML tags with inline scripts | 7 | Potential security/performance risk |
> | Tags with "All Pages" trigger that should be conditional | 5 | Unnecessary page load |
>
> **Critical issues:**
> 1. **Two Meta Pixels firing on all pages** — Pixel IDs `123456` and `789012`. This causes double-counted PageView events. If both are valid, one should be removed. If one is from an old account, delete it.
> 2. **Google Ads conversion tag with wrong value** — It's sending `1` as the conversion value instead of the actual purchase amount. Your ROAS data in Google Ads is completely wrong.
> 3. **Old Hotjar script** — Still loading on every page but the Hotjar account was cancelled 6 months ago. Free performance improvement to remove it.
>
> **Recommended cleanup:** Remove 12 tags, fix 2 critical issues, organize remaining tags into folders. Want me to proceed?

---

## GTM Tips from the Pros

1. **Always use Preview mode before publishing.** One bad tag can break your site or corrupt your analytics. GTM's Preview mode shows exactly which tags fire on each page — use it every time.
2. **One container per domain.** Don't share GTM containers across different websites. Each site should have its own container for clean configuration.
3. **Use data layer events, not CSS selectors.** Click triggers based on CSS selectors break when designers change the page. Data layer pushes are explicit and reliable.
4. **Folders are mandatory.** Organize tags by platform (Google, Meta, LinkedIn) or by function (Analytics, Conversion, Remarketing). A container with 50+ unsorted tags is unmaintainable.
5. **Conversion Linker is required for Google.** Without it, Google Ads can't attribute conversions to ad clicks. Always add it when setting up Google Ads conversion tracking.
6. **Version naming matters.** When publishing, name your version descriptively: "Added Meta Pixel + Purchase Event" not "Update 47." You'll thank yourself when debugging later.
7. **Audit quarterly.** Old tags from cancelled tools, duplicate pixels, and broken triggers accumulate. Audit your container every 3 months.

---

## FAQ

### Is there an MCP for Google Tag Manager?
Yes — this repo. It pre-configures the Synter MCP server for GTM management. Works with Amp, Cursor, VS Code, and Claude Desktop.

### Can AI create GTM tags for me?
Yes. Describe what you want to track — "add a Google Ads conversion tag for purchases" — and the agent creates the tag, trigger, and variables in your GTM container.

### Can the agent publish GTM changes?
Yes, but it will always confirm before publishing. You can also ask it to prepare changes without publishing, then review in the GTM UI before going live.

### Do I need to know JavaScript for GTM?
No. For standard use cases (conversion tracking, pixel installation, event tracking), the agent handles everything. Custom HTML tags may require JavaScript, but the agent can write those too.

### Can this replace a developer for tracking setup?
For standard tracking (pixels, conversion events, pageview tags), yes. For complex setups (custom ecommerce data layers, single-page app tracking, server-side GTM), you may still need developer support for the initial data layer implementation.

---

## Related Repos

- [google-analytics-agent](https://github.com/Synter-Media-AI/google-analytics-agent) — GA4 reporting & audiences
- [google-ads-agent](https://github.com/Synter-Media-AI/google-ads-agent) — Google Ads campaigns
- [conversion-tracking-agent](https://github.com/Synter-Media-AI/conversion-tracking-agent) — Verify all tracking works
- [meta-ads-agent](https://github.com/Synter-Media-AI/meta-ads-agent) — Meta Pixel setup
- [linkedin-ads-agent](https://github.com/Synter-Media-AI/linkedin-ads-agent) — LinkedIn Insight Tag
- [tiktok-ads-agent](https://github.com/Synter-Media-AI/tiktok-ads-agent) — TikTok Pixel setup

---

## License

MIT — see [LICENSE](LICENSE) for details.

Built by [Synter](https://syntermedia.ai) · [Get API Key](https://syntermedia.ai/developer) · [Documentation](https://syntermedia.ai/docs)
