# Roasby

[![skills.sh](https://skills.sh/b/Roasby/roasby)](https://skills.sh/roasby/roasby)

[Roasby](https://roasby.com) lets you manage Google Ads, Microsoft Ads, ChatGPT Ads and Meta Ads by talking to your AI assistant. Ask how your campaigns are doing, find searches that waste money, and make changes after a preview of exactly what will happen.

You need a [Roasby](https://roasby.com) account with at least one connected ad account. This repository is the Roasby plugin for Claude, Codex, ChatGPT and Cursor, and the skills for any assistant that reads Agent Skills.

## Install

**Claude Code**

```
/plugin marketplace add Roasby/roasby
/plugin install roasby@roasby
```

**Codex**

```
codex plugin marketplace add Roasby/roasby
codex plugin add roasby@roasby
```

**Cursor**: [add the Roasby MCP server](https://cursor.com/install-mcp?name=roasby&config=eyJ1cmwiOiJodHRwczovL3JvYXNieS5jb20vbWNwIn0=), or install the plugin from the Cursor marketplace.

**Skills only** (any assistant that reads Agent Skills):

```
npx skills add Roasby/roasby
```

The skills need the Roasby connection to work, so add the MCP server too.

**Claude.ai, Claude Desktop, ChatGPT, Gemini and any other MCP client**: add a custom connector with the URL `https://roasby.com/mcp`. You sign in to Roasby with OAuth when you first use it. The [setup guide](https://roasby.com/docs/connect-your-ai) has the steps for each assistant.

## What this plugin contains

- A connection to the Roasby MCP server at `https://roasby.com/mcp`. You sign in with OAuth when you first use it.
- Skills for your assistant:
  - `working-with-roasby`: how to use the Roasby tools safely (read freely, preview every change, wait for your yes), with detailed guides it reads when needed.
  - `roasby-wasted-spend-audit`: find spend without conversions and cut it with negative keywords and pauses.
  - `roasby-weekly-review`: a review of the week across platforms, with suggested next steps.
  - `roasby-measure-results`: what the ads and the changes really brought, with Google Analytics: the tracking check, visit quality and what happened after each change.
  - `roasby-launch-search-campaign`: plan and create a search campaign, created paused.
  - `roasby-budget-and-bid-check`: where more budget or higher bids would help, within your guardrails.

## What it connects to and sends

The plugin connects only to `https://roasby.com/mcp`. It runs no local code, has no hooks and stores nothing on your computer. What you ask your assistant to do with your ads goes to Roasby, which reads from and writes to the ad accounts you connected. The [privacy policy](https://roasby.com/privacy) explains how Roasby handles that data.

## How changes stay safe

- Reading is free. Every change takes a preview first, then your confirmation.
- Everything Roasby creates starts paused.
- There are no delete tools: Roasby pauses instead of deleting.
- Budget and bid changes stay within guardrails that only people can set.
- Every change is logged, and most can be reverted. Changes that can't be undone are off unless a workspace owner or admin allows them, and each one needs a second confirmation that names the risk.

## Limitations

- Roasby has no delete tools: it pauses instead (it archives only paused Meta Ads and ChatGPT Ads items, which can't be reverted), and a revert takes away only what that Roasby change added.
- Guardrails, connecting ad platforms, billing and account settings are set by people in Roasby on the web. When only you can do something, your assistant tells you what to do and where.
- A new search campaign is built whole from one blueprint (up to 1,000 ad groups with keywords, negative keywords and up to 3 ads each, and its own negative keywords, within 10,000 operations) and targets whole countries; a larger one is created in parts. Ad groups (with their keywords, negative keywords and up to 3 ads) can be added to an existing search campaign, and keywords and responsive search ads (with pinned headlines if you want) to an existing ad group (always paused), and a search campaign's settings (countries, excluded countries, regions, cities and radiuses, search partners, tracking, ad schedule, device bid adjustments, ad rotation, Microsoft Ads languages) can be changed within the max bid change, the text of responsive search ads (headlines, descriptions, pins, display paths, final URL) can be edited in place, one ad at a time or by find and replace, and a keyword's text and match type can be changed (by a replace on Google Ads: the new keyword is added paused and the old one paused; a match type in place on Microsoft Ads), as can its own final URL. Meta Ads ads and ChatGPT Ads chat cards can be edited too; other ad types and an asset's text are edited in the ad platform. Keyword research gives live estimates from Google Ads and Microsoft Ads.
- The conversion goals and own bidding strategy and targets of search, Performance Max, shopping and display campaigns (manual CPC, maximize clicks, maximize conversions with a target CPA, maximize conversion value with a target ROAS, target impression share; display bidding on Google Ads only) can be changed, with targets within the guardrails you set and the max bid change; switching away from bidding Roasby could not put back (such as Google Ads enhanced cost per click) is a change that can't be reverted. Roasby creates portfolio bid strategies, a search campaign joins or leaves one Roasby created, and it creates Google Ads custom conversion goals (both can't be reverted); the advertiser's own portfolios are changed in the ad platform. An ad group of a ChatGPT Ads clicks campaign switches between a target cost per click and maximize clicks, and one added to a campaign bids the way that campaign does (a cost per click or maximize clicks, a cost per thousand impressions on Reach, maximize conversions or a target cost per conversion on Conversions).
- Some platforms can't do everything: for example, Microsoft Ads has no broad match negative keywords, and ChatGPT Ads and Meta Ads have no keywords.
- Sitelinks, callouts, structured snippets and the other text assets, images and logos from the account's library, and (Google Ads) business names can be added to an ad account, campaign or ad group and paused or enabled: on Google Ads each is linked paused; Microsoft Ads has no paused links, so a new one shows at once where its place runs, and pausing one there takes the link away (the asset stays). An asset's text is not edited.
- On Meta Ads, an audience Roasby creates can't be reverted: Meta can't pause one and Roasby never deletes. A published ad set's conversion event, instant lead forms and customer list audiences stay in Meta Ads Manager.
- Roasby answers from a synced copy of your data, refreshed on a schedule or when you ask. Keyword Quality Scores and bid estimates are missing when the platform has too little data or (estimates) under automated bidding; your assistant then says so instead of guessing.
- One connection reaches every workspace you belong to. Your assistant works in your current workspace unless you name another, and can change ads only where your role allows it.

See [every limitation by platform](https://roasby.com/docs/limitations).

## Example prompts

- "How did my ads do in the last 7 days?"
- "Find wasted spend and suggest negative keywords."
- "Pause the Brand campaign."
- "Add these 20 keywords as phrase match to the Running shoes ad group."
- "Add trail, road and kids ad groups to the Running shoes campaign, each with its keywords and an ad."
- "Add a second ad to the Trail ad group, with our brand name pinned as the first headline."
- "Which workspaces can you see? Show me last week's spend in the Agency workspace."

## Support

Read the [docs](https://roasby.com/docs) or email support@roasby.com.

This repository is generated from Roasby's private source, so changes made here are overwritten.
