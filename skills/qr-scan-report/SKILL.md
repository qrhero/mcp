---
name: qr-scan-report
description: Use when someone asks how their QR Hero codes are doing, such as scans this week, month or quarter, the best and worst codes, countries, devices or languages, or wants a scan report or CSV to share.
---

# QR scan report

## 1. Agree the scope

- **What:** the whole organization, one brand (`brand_ulid` from `brands_list`) or one code (`code_ulid` from `codes_list`).
- **When:** the last 7, 30 or 90 days. These are the only ranges available. If the person asks for something else, offer the nearest one and say so.

## 2. Fetch

- `analytics_read` with `range` and the scope gives totals (scans, unique scanners), a daily time series, and a per-code or per-brand breakdown (`child_breakdown`).
- Add `dimension` (`country`, `device`, `referrer` or `language`) for a breakdown by that dimension. Call once per dimension the person cares about. Do not fetch all four by default.
- `codes_list` gives each code's name, status and lifetime `redirects_count` if you need context outside the chosen range.

The account's plan decides how far back statistics are kept. If a range comes back short or empty, say so instead of guessing.

## 3. Write the report

Keep it short and lead with what matters:

1. **Headline:** total scans and unique scanners for the period.
2. **Trend:** busy and quiet days from the time series, in words.
3. **Top and bottom codes** from the breakdown, with scan counts.
4. **Where and how:** top countries, devices or languages, if fetched.
5. **Worth a look:** one to three concrete observations. For example, an active code with no scans may be badly placed, too small or hard to reach. A spike may match a mailing or an event the person can confirm.

Report only what the data shows. Do not project future scans or compare with a period you did not fetch.

## 4. Export when asked

`analytics_export` takes the same arguments and returns `csv_rows`. Turn them into a CSV file with a header row, and name it after the scope and range, for example `qr-scans-spring-menu-30d.csv`.
