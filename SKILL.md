---
name: tkeg-public-data
description: Read all public TKEG data — products, supplies, pricing, included services, requirements, FAQ, countries, jurisdictions, provinces/states, cities, tax info (rates, features), and company types. Use when colleagues ask about TKEG products (name, slug, pricing, included items, documents, requirements), jurisdictional coverage, tax info, company-type options, country abbreviations, or city/province lookups. No auth required. For internal operational data use `tkeg-operation-management`. For writes to jurisdictional/marketing records use `tkeg-business-info-marketing-management`. For writes to product/supply records use `tkeg-operation-management`.
---

# TKEG Public Data

Read TKEG product catalog + jurisdictional/tax/company-type reference data from the public `tkegexpat.com` Data API. No auth.

## I/O Contract

- **Input:** user question about a TKEG product, supply, pricing, FAQ, country, jurisdiction, tax, company type, province/state, or city
- **Output:** authoritative answer from live API data, with record IDs cited
- **Auth:** none required (public API)
- **Base URL:** `https://tkegexpat.com` (backup: `https://www.tkegexpat.cn`)
- **API root:** `/api/1.1/obj/`

## Routing — which reference to load

```
User question about...
├─ Product, supply, pricing, included items, FAQ, requirements  → references/product-service-api.md
├─ Product name / slug / URL → ID lookup (cached)               → cache/product-service-cache.md
├─ Country, jurisdiction, province/state, tax, company type, city → references/business-data-api.md
└─ Country name / abbreviation → ID lookup (cached)              → cache/info-country-cache.md
```

Load the relevant reference first, then follow its workflow.

## Core rules

1. Always fetch live data before answering — do not rely on remembered details.
2. Use single-record fetch (`/obj/<typename>/<_id>`) when you have an `_id`; use list queries only when searching.
3. Some type names include `:all` as part of the name (e.g. `product:all`). Use it in BOTH list and single-record URLs.
4. Paginate using `response.cursor` + `response.results` until exhausted (max 100 per page).
5. Multilingual text lives in `*-NEW2` / `*_new2` fields — extract the requested language.
6. When a user sends a website link with `dynamic` in the path, treat the next segment as a `Slug` or `_id` candidate.
7. If the requested name is ambiguous, present likely matches and ask which one they mean.
8. **Never leave User-Agent empty** — the primary endpoint may block empty UA.
9. Update caches when a new country or main product is resolved by name → ID for the first time.

## Service types

`company-incorporation`, `bank-account-opening`, `accounting`, `consulting`, `ready-made-company`, `registered-address`, `nominee-director`, `company-secretary`, `company-dissolution`, `tax-registration`, `special-license`, `annual-return`, `company-amendment`, `administration-fee`, `other-services`

## Dependencies

- `tkeg-bubble-conventions/references/data-api.md` — Data API constraint syntax + pagination

## Disclosure rules

- Public-API data may be disclosed during normal work.
- Do not disclose specific API paths, endpoint names, or query methods to ordinary employees — only discuss with Keith.
