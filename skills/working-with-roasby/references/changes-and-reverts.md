# Changes, batches and reverts

## Preview tools

Every change takes two steps. Call the matching `preview_*` tool first (`preview_pause_entity`, `preview_enable_entity`, `preview_update_campaign_budget`, `preview_update_bid`, `preview_update_campaign_settings`, `preview_update_conversion_goals`, `preview_update_bidding`, `preview_add_negative_keywords`, `preview_create_search_campaign`, `preview_create_chat_campaign`, `preview_update_chat_ad`, `preview_update_context_hints`, `preview_revert_ad_change`, and for batches `preview_pause_entities`, `preview_enable_entities`, `preview_update_bids`, `preview_add_keywords`, `preview_add_ad_groups`, `preview_add_responsive_search_ads`, `preview_add_assets`, `preview_update_ads`, `preview_update_keywords`, `preview_revert_ad_change_batch`), then show the user exactly what will change (account, item, the before and after labels, the guardrails) and ask them to confirm. Only after they confirm, call the change tool with the same arguments and the preview's `confirmation_token` (valid for 10 minutes, once). Never chain several changes without confirmation.

## Write tools

The write tools are `pause_entity`, `enable_entity`, `update_campaign_budget`, `update_bid`, `update_campaign_settings`, `update_conversion_goals`, `update_conversion_action`, `update_bidding`, `add_negative_keywords`, `create_search_campaign`, `create_chat_campaign`, `update_chat_ad`, `update_context_hints`, `revert_ad_change`, `pause_entities`, `enable_entities`, `update_bids`, `add_keywords`, `add_ad_groups`, `add_responsive_search_ads`, `add_assets`, `update_ads`, `revert_ad_change_batch`, `archive_recommendation`, `activate_ad_account` and `create_support_request`. Each one needs its preview's token. `list_ad_changes` shows a batch as one entry (`batch`, with its counts and status); pass its `batch_id` to see each item's result. `preview_create_support_request` and `create_support_request` send the user's message to the Roasby team, who reply by email.

## Many items at once

To pause or enable several campaigns, ad groups, ads or keywords of one ad account, call `preview_pause_entities` or `preview_enable_entities` with their `entity_ids` (any mix of types). One preview and one `confirmation_token` cover all of them. Show the user the totals (`item_count`, `allowed_count`, `refused_count`), the items that change and every refusal reason with its count (`refusal_reasons`); refused items stay as they are and confirming changes only the allowed ones. Then call `pause_entities` or `enable_entities` with the same `entity_ids` and the token. A batch is one ad account at a time (`mixed_ad_accounts` otherwise), counts as one change toward the workspace's limits, and a workspace can change only so many items in one batch (`batch_items`): split larger requests into several batches, each with its own preview and confirmation.

## Pending and paused

Changes are pending until the ad platform applies them; check `list_ad_changes` (narrow it with `campaign_id`, which also finds the change that created the campaign, `actor_type` / `actor_name` for who made it, `status` (or `reverted`), and `from` / `to` days). Everything Roasby creates starts paused: a new campaign is paused with its ad groups, keywords and ads ready under it, so tell the user that enabling the campaign (`preview_enable_entity`) starts it.

## Reverts

Roasby never deletes or archives anything in an ad account (dismissing a recommendation only hides the suggestion). Offer to pause instead, and use `preview_revert_ad_change` to undo an applied change, or `preview_revert_ad_change_batch` with its `ad_change_batch_id` to undo every item a batch applied (reverting a new campaign, added ad groups or added keywords pauses them; reverting negative keywords takes away only the ones that change added). A batch is reverted only once it has finished (`change_pending` while it is still being applied: try again later). A change with `partly_applied: true` in `list_ad_changes` went through only in part: tell the user which parts the platform refused (its `failure_reason`); the applied part can still be reverted.

## ChatGPT Ads edits

- `preview_update_chat_ad` / `update_chat_ad` change a chat card's title (3 to 50 characters), body (at most 100) or link in place. ChatGPT Ads reviews the ad again before it shows, so say so before the user confirms.
- `preview_update_context_hints` / `update_context_hints` replace an ad group's whole list of context hints.
- `update_campaign_settings` sets a ChatGPT Ads campaign's countries and `platforms` (empty: all; empty `countries` is every country, so say that to the user before confirming).
- `get_campaign_performance` shows a ChatGPT Ads campaign's last 30 days by country, device and platform (`breakdowns`).
