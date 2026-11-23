# Performance Max asset groups

Performance Max campaigns on Google Ads and Microsoft Ads have asset groups instead of ad groups. Read them with `list_asset_groups` (narrow it with `ad_account_id` or `campaign_id`): each has its Roasby ID, status, ad strength (null on Microsoft Ads), first final URL, headlines, long headlines and descriptions. Each also lists its images, logos and videos by slot (`media`). Business names are not listed, and neither are the texts and media Google created by itself.

To change the texts, call `preview_update_asset_group_texts` with the `asset_group_id` and the whole new list of any of `headlines` (3 to 15, up to 30 characters each), `long_headlines` (1 to 5, up to 90) and `descriptions` (2 to 5, up to 90). A field you leave out keeps its texts. Characters of Chinese, Japanese and Korean and emoji count as two. Show the user every `details` line: the texts before and after, and that new texts are added as new assets and linked, while texts that go are unlinked from the asset group (their assets stay in the account). `asset_group_texts_invalid` names each problem in the details: fix those texts and preview again. After the user confirms, call `update_asset_group_texts` with the same arguments and the `confirmation_token`. The ad platform reviews new texts before they show.

Only one text change and one signals change per campaign run at a time (`has_pending_change`). A revert links the old texts again and unlinks the ones the change added; it is refused as changed since once the texts changed again.

To guide an asset group toward the right people and searches, add signals with `preview_add_asset_group_signals`: the `asset_group_id`, search_themes (searches it should show for) and `audience_ids` (the platform IDs from the top-level `audiences` of `list_asset_groups`, for that asset group's ad account; on Microsoft Ads they are the account's audience groups). A theme the group has already is left out. To take signals away, also ones added in the ad platform, use `preview_remove_asset_group_signals` and `remove_asset_group_signals` with the same arguments (a revert adds them back as new signals, so `reversibility` is partial); `audience_not_available` means the audience is not one the account has; an asset group takes one audience signal, so a second is refused (`one_audience_per_asset_group`), and at most 50 search themes, counting those it has (`too_many_search_themes`). Tell the user that signals guide the ad platform, they do not limit who sees the ads. After the user confirms, call `add_asset_group_signals` with the same arguments and the `confirmation_token`. A revert takes away exactly the signals the change added.

## Images and videos

`list_media` lists the image and video library of one Google Ads, Microsoft Ads or Meta Ads account (`ad_account_id` is required; on Meta Ads each image's platform ID is its `image_hash`, the one new Meta ads, their `vertical_image_hash` and carousel cards take): what the person uploaded in the ad platform, each with its Roasby ID, platform ID, size, file type, a link to see it, and `fits`, the slots its aspect ratio and size fit:

- `marketing_image` (landscape, 1.91:1)
- `square_marketing_image` (1:1)
- `portrait_marketing_image` (4:5, Google Ads only)
- `logo` (1:1) and `landscape_logo` (4:1)
- `video` (a YouTube video on Google Ads, a video Microsoft Ads downloaded)

Pass `slot` to see only what fits it, `kind` for images or videos, `search` for a name. Open the `url` to look at an image before suggesting it. If the user just uploaded media in the ad platform, `run_ad_account_sync` reads the library again. Roasby never downloads an image or video file itself.

To change an asset group's media, call `preview_update_asset_group_media` with the `asset_group_id` and the whole new list of any slot: `marketing_images`, `square_marketing_images`, `portrait_marketing_images` (Google Ads only), `logos`, `landscape_logos`, `videos`. A slot you leave out keeps its media. Each item is exactly one of:

- `media_id`: the Roasby ID of an image or video from `list_media` (pick one whose `fits` has the slot: a library image that does not fit its slot is refused);
- `image_url` (image slots): a public link to a JPEG, PNG, GIF or WebP image. The preview fetches and checks it (it must fit the slot's ratio and size); the write uses what the preview fetched, so call the write with the same link within the hour. Roasby uploads it to the library only after the confirmation;
- `youtube` (videos, Google Ads): a YouTube link or the video's ID;
- `video_url` (videos, Microsoft Ads): an https link to the video file that Microsoft downloads.

Show the user every `details` line: the media before and after. A YouTube video or video link becomes a new video in the library first; media that goes is unlinked and stays in the library. An asset group needs at least one landscape and one square image (and on Google Ads a logo), and takes at most 20 images, 5 logos and 5 videos. A Google Ads campaign with brand guidelines (Google's default) has its business name and logos on the campaign: its asset groups need no logo and take none, so leave `logos` and `landscape_logos` out there. `asset_group_media_invalid` names each problem in the details. After the user confirms, call `update_asset_group_media` with the same arguments and the `confirmation_token`. The ad platform reviews new media before it shows; a revert links the old media again.


## Add an asset group or pause one

To add an asset group to a Performance Max campaign the user already has (a campaign takes up to 100), call `preview_add_asset_group` with the `campaign_id` and one asset group in the same fields as a new campaign's: `name` (one the campaign does not use yet), `final_url`, `business_name` (25 characters; leave it out on a Google Ads campaign with brand guidelines, whose business name and logos are the campaign's), `headlines`, `long_headlines`, `descriptions`, optional search themes and its media slots. Show the user every `details` line; `asset_group_invalid` names each problem and `too_many_asset_groups` says the campaign is full. After the user confirms, call `add_asset_group` with the same arguments and the `confirmation_token`.

The new asset group is created **paused**, also under a running campaign. To start it, or to pause any asset group, call `preview_update_asset_group_status` with the `asset_group_id` and `status` (`paused` or `enabled`), show the user the status before and after, and after they confirm call `update_asset_group_status`. Reverting a new asset group pauses it; reverting a status change sets the status back.


## URL options of an asset group

An asset group's URL options (`urls` in `list_asset_groups`) are used for that asset group instead of its campaign's own: the display path (`path1`, `path2`, up to 15 characters each, shown after the final URL's domain; `path2` only with `path1`), `final_mobile_url`, `tracking_url_template` (with a landing page tag such as `{lpurl}`), `final_url_suffix` (such as `utm_source=google&utm_medium=cpc`, no leading `?`) and `custom_parameters` (up to 8 `key`/`value` pairs, keys of letters and digits up to 16, used in the template as `{_key}`). `urls` is null until a sync read them.

A new asset group takes them in the same fields (`preview_add_asset_group`, and each asset group of `preview_create_pmax_campaign`). To change them, call `preview_update_asset_group_urls` with the `asset_group_id` and only the options that change: each one given replaces the asset group's own, `null` takes it away, an option left out is kept, and `custom_parameters` is the whole new list (an empty list takes them all away). Show the user the options before and after; `asset_group_urls_invalid` names each problem in the details. After the user confirms, call `update_asset_group_urls` with the same arguments and the `confirmation_token`. A revert sets the options back.
