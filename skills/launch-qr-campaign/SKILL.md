---
name: launch-qr-campaign
description: Use when someone wants QR codes in QR Hero for a campaign, flyer, poster, menu, packaging, store or event, from a single code to a few hundred, with the right brand, style and domain.
---

# Launch a QR campaign

The goal is codes that scan on the first try, carry the right brand, and are easy to find and report on later.

## 1. Find out what is needed

Ask only for what is missing:

- Where each code should send people (a full `https://` URL).
- How many codes, and what tells them apart: one per store, per table, per poster, per product.
- A short campaign name.

## 2. Read the account setup

- `domains_list`: use the organization's own domain if it has one, otherwise the default QR Hero domain.
- `brands_list`: if there is more than one brand, ask which one the campaign belongs to.
- `styles_list` with that `brand_ulid`: use the brand's default style unless the person names another.

Always take ULIDs from these results. Never guess one.

## 3. Plan slugs, names and destinations

- Slugs are short, lowercase and hyphenated, and follow one pattern per campaign: `spring-menu`, `store-oslo-01`, `table-12`. Pass only the short part. On the shared QR Hero domain the organization's prefix is added for you.
- Names are what a person will search for later, for example "Spring menu, table 12".
- If the person uses web analytics, offer to add `utm_source=qr&utm_medium=print&utm_campaign=<campaign>` to each destination. Keep any query parameters that are already there.
- Show the plan as a short table (slug, name, destination) and wait for a yes before creating anything.

## 4. Create the codes

**One code:** `codes_create` with `slug`, `destination`, `domain_ulid`, `name`, and `brand_ulid` and `style_ulid` if chosen.

**Several codes:** `codes_bulk_create` with shared values in `defaults` (`domain_ulid`, `brand_ulid`) and one row per code with `slug`, `destination` and `name`.

- Up to 10 rows are created straight away.
- Above 10, the first call returns a preview and a `confirmation_id`. Tell the person what the preview says (rows, errors), and only after they agree call again with the same arguments plus `confirm: true` and the `confirmation_id`. The id expires after 5 minutes, so ask again if it has lapsed.
- If the result is an import waiting for confirmation (`awaiting_confirmation: true` with a `confirm_token`), check `valid_row_count` and `preview_errors` with the person, then call `codes_bulk_confirm` with the import ULID and the `confirm_token`. Follow progress with `codes_bulk_read` until it completes.

If a slug is already taken, suggest the next free variant. Never reuse or overwrite an existing code without asking.

## 5. Before anything goes to print

- Share each code's `public_url` and ask the person to scan one from the final artwork or a print proof with a phone.
- Print at least 2 cm (0.8 in) wide for close reading, and larger for posters read from a distance. Keep the light margin around the code, and use a dark code on a light background.
- Remind them that the destination can be changed later without reprinting, because the printed code points at QR Hero, not straight at the page.

## Avoid

- Setting `password`, `redirect_cap` or `schedule` unless asked. A redirect cap stops the code after that many scans, which is rarely what people want on printed material.
- Promising features the account may not have. If a call fails because of the plan, say which call failed and that the plan settings decide it.
