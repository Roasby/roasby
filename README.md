# Roasby for Claude

Roasby lets you manage Google Ads, Microsoft Ads and ChatGPT Ads by talking to Claude. Ask how your campaigns are doing, find searches that waste money, and make changes after a preview of exactly what will happen.

You need a [Roasby](https://roasby.com) account with at least one connected ad account.

Install it in Claude Code:

```
/plugin install roasby --marketplace Roasby/roasby-claude-plugin
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
- A new campaign has one ad group, one ad and up to 50 keywords, and targets whole countries. Keywords can be added to an existing ad group (always paused), but Roasby can't yet add ad groups or ads, edit campaign settings, ad text, assets or schedules, or research keywords.
- Some platforms can't do everything: for example, Microsoft Ads has no broad match negative keywords and ChatGPT Ads has no keywords.
- Roasby answers from a synced copy of your data, refreshed on a schedule or when you ask.
- One connection reaches every workspace you belong to. Claude works in your current workspace unless you name another, and can change ads only where your role allows it.

See [every limitation by platform](https://roasby.com/docs/limitations).

## Example prompts

- "How did my ads do in the last 7 days?"
- "Find wasted spend and suggest negative keywords."
- "Pause the Brand campaign."
- "Add these 20 keywords as phrase match to the Running shoes ad group."
- "Which workspaces can you see? Show me last week's spend in the Agency workspace."

## Support

Read the [docs](https://roasby.com/docs) or email support@roasby.com.

This repository is generated from Roasby's private source, so changes made here are overwritten.
