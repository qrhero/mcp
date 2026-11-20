---
name: change-qr-destination
description: Use when a QR Hero code that is already printed or shared should send people somewhere new (a moved page, a broken link, a new offer, a seasonal swap), or when someone says a code stopped working.
---

# Change where a printed code goes

A QR Hero code points at QR Hero, which forwards each scan to the destination. Changing the destination changes every copy of the code that is already printed, with no reprint.

## 1. Find the code

- `codes_list`, then match on name, slug, `public_url` or current destination.
- If more than one code matches, show the candidates (name, slug, current destination, scans) and ask which one.
- `codes_read` the chosen code to see its full settings.

## 2. If the code "stopped working", find out why first

Check these in order and explain what you find in plain words:

1. `status`: anything other than `active` (for example `disabled_manual` or `archived`) means the code is switched off.
2. `redirect_cap` compared with `redirects_count`: if the cap is reached, the code has stopped on purpose.
3. `schedule`, `date_begin` and `expire_at`: the code may only answer at certain times or dates.
4. The destination itself: the page may have moved or gone. Only open the URL if the person asks you to check it.

QR Hero does not stop scans because of the account's plan usage or billing. A code stops only because of its own settings or its destination.

## 3. Confirm the change

Show the change before making it:

| Code | Now | New |
|---|---|---|
| Spring menu, table 12 | https://example.com/menu-spring | https://example.com/menu-summer |

Wait for a yes. If the new page uses tracking parameters, carry over the ones on the old destination unless told otherwise.

## 4. Make the change

- `codes_update` with `code_ulid` and the new `destination`.
- To switch a code back on, include `status: "active"`, but only when the person asked for it.
- To raise or remove a redirect cap, change `redirect_cap` only with the person's agreement.

**Many codes at once:** list them all in one table first, then call `codes_update` once per code. To switch several codes on or off together, use `codes_bulk_update_status`. It always returns a preview first. Call it again with `confirm: true` and the `confirmation_id` once the person agrees.

## 5. Check it

`codes_read` the code again, confirm the new destination is saved, and ask the person to scan a printed copy.
