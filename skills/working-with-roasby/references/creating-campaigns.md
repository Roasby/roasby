# Creating campaigns

## What to ask

`create_search_campaign` creates a complete search campaign, paused, from one blueprint with one preview and one confirmation. Ask the user for everything it needs (account, name, daily budget, bidding, countries, and the ad groups with their keywords and ads) and always ask whether it contains EU political advertising; never assume the answer. Ask about the optional targeting too, or say what the defaults are: any number of countries; `location_option` (`presence_or_interest` by default, or `presence` for only people in the countries); search partners (off by default); `languages` (Microsoft Ads ad languages as ISO codes, all by default; on Google Ads search campaigns have no language targeting, so explain that Google matches languages itself and the ads should be written in the languages the user wants to reach); and an optional `final_url_suffix` (such as `utm_source=google&utm_medium=cpc`) and `tracking_url_template` (with `{lpurl}`).

## The blueprint

The campaign's own `negative_keywords` (up to 10,000) and `ad_groups` (1 to 1,000), each in the shape `add_ad_groups` takes: a `name` unique in the campaign, an optional `cpc_bid_micros` (only under `manual_cpc`, within the account's max bid when one is set; without it the ad group uses the campaign's `cpc_bid_micros`), `keywords` (each with an optional own bid and `final_url`), `negative_keywords` and up to 3 `responsive_search_ads`. Everything is created at once: the campaign paused, its ad groups, keywords and ads enabled under it. It is all or nothing: when any part breaks a rule, nothing is created and the preview names every such part in `details`, with its path (`ad group "Shoes" › keyword: shoes! (Exact match): …`, up to 20 and a count of the rest); fix them all and preview again. The campaign and each ad group, keyword, negative keyword and ad count toward the workspace's items per change (`batch_items`), and the whole campaign (with the budget and each country) must fit one Google Ads request of 10,000 operations (`campaign_too_large`, on both platforms): for a larger campaign, create it with part of its ad groups, then add the rest with `preview_add_ad_groups`. On Microsoft Ads a large campaign is made step by step over a few minutes: it stays pending and can't be enabled until it is complete; if Microsoft stops part way, the change fails with what exists, the campaign stays paused, and Roasby won't enable it (`campaign_incomplete`) until a sync shows every part (its negative keywords, and each ad group with all its keywords, negative keywords and ads; Roasby can't see targeting, so a build that stopped before it stays refused): tell the user what is missing, and that they can finish it in the ad platform and enable it there, or pause it and create it again.

## From a file

Roasby takes no file upload. When the user has one (a Google Ads Editor export, a spreadsheet, a pasted list), read it yourself and fill the blueprint from it, then preview. For example, an Editor CSV with the columns `Campaign, Ad group, Keyword, Criterion Type, Max CPC, Headline 1, Headline 2, Headline 3, Description 1, Description 2, Final URL`:
```
Campaign,Ad group,Keyword,Criterion Type,Max CPC,Headline 1,Headline 2,Headline 3,Description 1,Description 2,Final URL
Shoes,Trail,trail running shoes,Phrase,1.20,,,,,,
Shoes,Trail,waterproof trail shoes,Exact,,,,,,,
Shoes,Trail,free,Negative Phrase,,,,,,,
Shoes,Trail,,,,Trail Running Shoes,Grip On Every Path,Free Returns,Light shoes for rough trails.,Order today and run tomorrow.,https://example.com/trail
Shoes,,jobs,Campaign Negative Phrase,,,,,,,
```
becomes (money in micros, match types in lower case, one ad group per `Ad group`, the rows without a keyword as its ads, campaign-level negatives at the top):
```json
{"name": "Shoes", "negative_keywords": [{"text": "jobs", "match_type": "phrase"}],
 "ad_groups": [{"name": "Trail",
   "keywords": [{"text": "trail running shoes", "match_type": "phrase", "cpc_bid_micros": 1200000}, {"text": "waterproof trail shoes", "match_type": "exact"}],
   "negative_keywords": [{"text": "free", "match_type": "phrase"}],
   "responsive_search_ads": [{"final_url": "https://example.com/trail",
     "headlines": [{"text": "Trail Running Shoes"}, {"text": "Grip On Every Path"}, {"text": "Free Returns"}],
     "descriptions": [{"text": "Light shoes for rough trails."}, {"text": "Order today and run tomorrow."}]}]}]}
```

Add the settings the file does not hold (account, daily budget, bidding, countries, the EU question) by asking the user. Paused or removed rows in the file are still created paused or not at all: ask which ones the user wants. Show every line of the preview's `details` before confirming. The account needs a max daily budget set by people in Roasby. On Microsoft Ads, political ads are refused, manual bidding becomes enhanced cost per click (the preview says so), and negative keywords are exact or phrase only. On ChatGPT Ads, use `create_chat_campaign` instead (no keywords there): ask for the account, name, daily budget, bidding (a max cost per click or maximize clicks), countries (optional), context hints (the topics of the conversations the ads fit), the chat card's title and text, the final URL and an https image link.

## Meta Ads campaigns

On Meta Ads (Facebook and Instagram), use `preview_create_meta_campaign` and `create_meta_campaign`: one preview and one confirmation for the whole campaign. Ask the user for:
- the account, the name and a daily budget (on the campaign: Meta spreads it over the ad set);
- the objective: `traffic` (people visiting the links, optimized for link clicks) or `sales` (website conversions: `pixel_id`, the ID of one of the account's pixels in `list_conversion_actions`, and a `conversion_event` such as `purchase` or `lead`);
- `special_ad_category`: ask whether the ads are about housing, employment, credit, or social issues, elections or politics; never assume it. Roasby creates only campaigns of `none`: such ads are made in Meta Ads Manager;
- 1 to 25 countries, optional `age_min` and `age_max` (18 to 65) and dates;
- `page_id`: the Facebook Page the ads run as, one of the account's `pages` in `list_ad_accounts` (read with the conversion setup; a Page added in Meta shows after the next sync);
- when the countries include an EU country, who pays for the ads and who benefits (`dsa_payor`, `dsa_beneficiary`): both are required then (`eu_payer_required`);
- with an end date, the campaign must run longer than 24 hours (`ad_set_too_short`);
- 1 to 5 ads, each with `primary_text`, `headline`, an optional `description`, an https `link`, a `call_to_action` and exactly one media: `image_url` or `video_url` (with an optional `thumbnail_url`), which Meta downloads from the link (Roasby never downloads or uploads media; a link that does not load stops the campaign before anything is made), `image_hash` (an image already in the account's library) or `post_id` (an existing Page post as `pageID_postID`, which brings its own text and link, so the ad has no other field).

The campaign is created paused with its ad set and ads active under it, so enabling the campaign starts them; Meta reviews the ads. A video can take a minute or two to process. If Meta made only part of it, the change fails with what exists and the campaign stays paused: tell the user what is missing. If Meta's answer was lost, the change fails as possibly made and Roasby never sends it twice: the next sync shows whether the campaign exists, so check before creating it again.
