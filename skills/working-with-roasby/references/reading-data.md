# Reading data

Reads need no preview or confirmation. This is what each read gives and how to quote it.

## Read tools

Read freely: `list_ad_accounts`, `get_performance_summary` (spend, conversions and ROAS across platforms for the last 7, 30 or 90 days, per currency, or for one ad account or campaign with `ad_account_id` or `campaign_id`, with a row per day), `get_campaign_performance` (each campaign's results), `get_search_terms` (what people searched), `find_wasted_spend` (searches and keywords that cost money without a conversion), `list_recommendations` (the changes Roasby suggests), `list_alerts` (spend spikes, cost per conversion jumps and disapproved ads, with why), `get_weekly_report` (the weekly report members get by email), `list_conversion_actions` (how each ad account tracks conversions), `list_ad_entities` (campaigns, ad groups, ads, keywords and their IDs), `list_assets` (sitelinks, callouts, structured snippets and the other text assets, with the IDs of their links) and `list_ad_changes` need no confirmation.

## Removed items

An item that failed with "no longer has this item" was removed in the ad platform since Roasby's last sync: leave it out and do not send it again (after bids or pause/enable Roasby marks it removed at once, otherwise the next sync does).

## Search terms and conversions

`get_search_terms` gives `other_terms_cost`: search campaign spend the ad platform shows under no search term (searches too rare to report). Mention it when you sum wasted spend. `list_conversion_actions` gives each action's `recent_conversions` (last 30 days), and a campaign's Google Ads custom goal lists its actions in `custom_goal_conversion_action_ids`.

Conversion value and ROAS come only from ad accounts that report conversion value. When totals have `conversion_value_reported` false, show value and ROAS as n/a, never as zero. Spend from accounts without value is left out of ROAS and reported in `cost_without_value_micros`, so say so when it is not 0.

## Accounts and sync

Accounts: `list_ad_accounts` shows which accounts are active. To start managing one, use `preview_activate_ad_account`, then `activate_ad_account` after the user confirms. `run_ad_account_sync` asks for fresh data from the ad platform and needs no preview. Don't call it after a change made through Roasby: an applied status, budget, bid or accepted edit is in Roasby's copy at once, and after a new campaign, ad group, keyword, ad or asset Roasby syncs that account by itself within a few minutes (`follow_up_sync_due_at` in `list_ad_accounts`; at most once every 5 minutes and 24 times a day, and not while the shared Google Ads quota is low); after negative keywords it reads the account's negative keywords again. Use `run_ad_account_sync` when the user needs fresh numbers or changed something in the ad platform itself (check `last_synced_at` first): Microsoft Ads and ChatGPT Ads accounts can be synced every few minutes, Google Ads accounts less often (once a day to every 15 minutes, by the workspace). It also reads the conversion goals again (`conversions_synced_at` in `list_ad_accounts`), so a new campaign can get its goals and targets. When it answers `started: false`, the account was synced too recently or a sync is already on its way: wait until `next_sync_allowed_at` instead of calling it again. `settings_read_failing: true` means the platform refused the campaign settings read at the last sync. `get_workspace_members` lists who is in the workspace.
