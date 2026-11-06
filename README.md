# Roasby for Claude

Roasby lets you manage Google Ads, Microsoft Ads and ChatGPT Ads by talking to Claude. Ask how your campaigns are doing, find searches that waste money, and make changes after a preview of exactly what will happen.

You need a [Roasby](https://roasby.com) account with at least one connected ad account.

Install it in Claude Code:

```
/plugin marketplace add Roasby/roasby-claude-plugin
/plugin install roasby@roasby
```

## What this plugin contains

- A connection to the Roasby MCP server at `https://roasby.com/mcp`. You sign in with OAuth when you first use it.
- The `working-with-roasby` skill, which tells Claude how to use the Roasby tools safely.

## What it connects to and sends

The plugin connects only to `https://roasby.com/mcp`. It runs no local code, has no hooks and stores nothing on your computer. What you ask Claude to do with your ads goes to Roasby, which reads from and writes to the ad accounts you connected. The [privacy policy](https://roasby.com/privacy) explains how Roasby handles that data.

## How changes stay safe

- Reading is free. Every change takes a preview first, then your confirmation.
- Everything Roasby creates starts paused.
- Nothing is ever deleted from your ad accounts.
- Budget and bid changes stay within guardrails that only people can set.
- Every change is logged, and most can be reverted.

## Limitations

- Roasby never deletes anything from your ad accounts: it pauses instead.
- Guardrails, connecting ad platforms, billing and account settings are set by people in Roasby on the web. When only you can do something, Claude tells you what to do and where.
- A new search campaign is built whole from one blueprint (up to 1,000 ad groups with keywords, negative keywords and up to 3 ads each, and its own negative keywords, within 10,000 operations) and targets whole countries; a larger one is created in parts. Ad groups (with their keywords, negative keywords and up to 3 ads) can be added to an existing search campaign, and keywords and responsive search ads (with pinned headlines if you want) to an existing ad group (always paused), and a search campaign's settings (countries, search partners, tracking, ad schedule, device bid adjustments, ad rotation, Microsoft Ads languages) can be changed within the max bid change, the text of responsive search ads (headlines, descriptions, pins, display paths, final URL) can be edited in place, one ad at a time or by find and replace, and a keyword's text and match type can be changed (by a replace on Google Ads: the new keyword is added paused and the old one paused; a match type in place on Microsoft Ads), as can its own final URL, but Roasby can't yet edit other ad types or assets. Keyword research gives live estimates from Google Ads and Microsoft Ads.
- A search campaign's conversion goals and its own bidding strategy and targets (manual CPC, maximize clicks, maximize conversions with a target CPA, maximize conversion value with a target ROAS, target impression share) can be changed, with targets within the max bid and the max bid change; portfolio strategies and bidding Roasby could not put back are changed in the ad platform, and Roasby never creates custom conversion goals.
- Some platforms can't do everything: for example, Microsoft Ads has no broad match negative keywords and ChatGPT Ads has no keywords.
- Sitelinks, callouts, structured snippets and the other text assets can be added to an ad account, search campaign or ad group and paused or enabled: on Google Ads each is linked paused; Microsoft Ads has no paused links, so Roasby links new ones only to a paused campaign or ad group, and pausing one there takes the link away (the asset stays). Images, logos and business names are not added yet, and an asset's text is not edited.
- Roasby answers from a synced copy of your data, refreshed on a schedule or when you ask. Keyword Quality Scores and bid estimates are missing when the platform has too little data or (estimates) under automated bidding; Claude then says so instead of guessing.
- One connection reaches every workspace you belong to. Claude works in your current workspace unless you name another, and can change ads only where your role allows it.

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
