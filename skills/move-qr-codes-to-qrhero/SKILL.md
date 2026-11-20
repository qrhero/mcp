---
name: move-qr-codes-to-qrhero
description: Use when someone is moving to QR Hero from another QR code generator and wants their existing codes, an export file or a list of links set up as QR Hero codes.
---

# Move QR codes over from another generator

## 1. Get the list

Ask for an export from the old tool (CSV or spreadsheet) or a pasted list. For each code you need a name and the destination URL. Old short links and scan counts are useful too.

## 2. Be clear about what moves and what does not

Say this before creating anything:

- A printed code contains the old provider's link, so it keeps going through the old provider. QR Hero cannot take it over.
- **If the old codes are dynamic and the old account stays open for now:** create the QR Hero codes, then in the old tool point each old code at its new QR Hero `public_url`. Scans then pass through QR Hero and are counted here until the material is reprinted with QR Hero codes.
- **If the old codes contain the destination directly:** nothing can redirect them. They keep going to the same page until they are replaced.
- Scan history stays in the old tool. QR Hero counts from the first scan of each new code.

## 3. Clean the list

- Flag rows with empty or malformed URLs, and duplicates of the same destination. Ask what to do with each. Never drop rows silently.
- Turn names into slugs that follow one pattern, for example `menu-table-01`.
- Group by brand if the account has several (`brands_list`), and pick the domain with `domains_list`.

Show the cleaned list as a table and wait for a yes.

## 4. Create

Use `codes_bulk_create` with the shared `domain_ulid` and `brand_ulid` in `defaults` and one row per code (`slug`, `destination`, `name`).

- Up to 10 rows are created straight away.
- Above 10, the first call returns a preview and a `confirmation_id`. Share the preview, and after the person agrees, call again with `confirm: true` and the `confirmation_id`.
- If the result is an import waiting for confirmation (`confirm_token`), call `codes_bulk_confirm` with the import ULID and token, then follow it with `codes_bulk_read`.

For long lists, create in batches and report progress between them.

## 5. Hand back the mapping

Give the person a CSV with one row per code: name, old link, destination, new QR Hero `public_url`. They need it to update the old codes (step 2) and to plan reprints.
