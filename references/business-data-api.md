# TKEG Business Data API Reference

Jurisdiction, tax, and company-type data.

## Workflow

1. Identify the jurisdiction/country by name or abbreviation.
   - Check `info-country-cache.md` first for a cached `_id`.
   - If not cached → query `info_country` to resolve.
2. Fetch the country record from `info_country/<_id>`.
3. If the country was resolved via API and not already cached → add name, abbreviation, slug, `_id` to `info-country-cache.md` under the appropriate continent section.
4. If feature detail matters, expand via `info_country_feature` (linked from `data: feature list`).
5. If province/state detail matters, expand via `info-province-state` (linked from `info-province-list`).
6. If tax summary matters, follow `tax_info` → `info_tax`.
7. If tax feature detail matters, expand via `info_tax_feature` (linked from `data: tax feature list`).
8. If specific VAT/sales-tax rows matter, query `info:tax:vatrate`.
9. If legal entity type matters, query `info_company_type` for the jurisdiction.

Use these object types together for jurisdiction answers:
- `info_country`: base jurisdiction identity and overview
- `info_tax`: tax summary linked from country
- `info_company_type`: legal-entity / incorporation-type detail by jurisdiction

## Endpoints

| Object | Path |
|--------|------|
| Country info | `/api/1.1/obj/info_country` |
| Country features | `/api/1.1/obj/info_country_feature` |
| Province / state | `/api/1.1/obj/info-province-state` |
| Tax summary | `/api/1.1/obj/info_tax` |
| Tax features | `/api/1.1/obj/info_tax_feature` |
| VAT / sales-tax rates | `/api/1.1/obj/info:tax:vatrate` |
| Company types | `/api/1.1/obj/info_company_type` |

## Data Structure

### `info_country`
Country or jurisdiction summary.

| Field | Type | Notes |
|-------|------|-------|
| `_id` | text | |
| `Slug` | text | |
| `abbreviation` | text | ISO code |
| `capital-city` | text | |
| `common-name-NEW2` | text | Multilingual common name |
| `full-name-new2` | text | Multilingual full name |
| `system: continent` | string | |
| `system: jurisdiction nature` | string | |
| `system: belonging-jurisdiction` | list.info_country | Parent jurisdiction IDs |
| `data: feature list` | list.info_country_feature | → `info_country_feature._id[]` |
| `info-province-list` | list.info-province-state | → `info-province-state._id[]` |
| `tax_info` | info_tax | → `info_tax._id` |
| `local-currency` | string | |
| `quick-view-new2` | text | Multilingual overview |
| `easy_doing_business` | string | |
| `falg` | image | Country flag |
| `prime_image` | image | |
| `internal service documentation` | text | |
| `last_update` | date | |
| `dial-code` | number | |
| `Created Date` | date | |
| `Modified Date` | date | |

### `info-province-state`
Province or state subdivisions for a jurisdiction.

| Field | Type | Notes |
|-------|------|-------|
| `_id` | text | |
| `Slug` | text | |
| `belonging-info-country` | info_country | → `info_country._id` |
| `common-name-NEW2` | text | Multilingual province/state name |
| `state-code` | text | ISO 3166-2 subdivision code |
| `info-city-list` | list.info-city | → `info-city._id[]` |
| `Created Date` | date | |
| `Modified Date` | date | |

### `info_country_feature`
Feature cards for a jurisdiction.

| Field | Type | Notes |
|-------|------|-------|
| `_id` | text | |
| `Slug` | text | |
| `belonging_jurisdiction` | info_country | → `info_country._id` |
| `header-new2` | text | Multilingual header |
| `body-new2` | text | Multilingual body |
| `image` | image | |
| `Created Date` | date | |
| `Modified Date` | date | |

### `info_tax`
Tax summary for a jurisdiction.

| Field | Type | Notes |
|-------|------|-------|
| `_id` | text | |
| `Slug` | text | |
| `country_region` | info_country | → `info_country._id` |
| `general_vat_rate` | text | |
| `general_cit_rate-NEW2` | text | Multilingual CIT rate |
| `capital_gain_tax-NEW2` | text | Multilingual capital gains |
| `cit_estimate_payment_due_date-NEW2` | text | |
| `cit_payment_due_date-NEW2` | text | |
| `cit_return_due_date-NEW2` | text | |
| `withdrawing_tax_resident (dividend / interest /royalty)` | text | Format: `D/I/R` |
| `withdrawing_tax_none_resident (dividend / interest /royalty)` | text | Format: `D/I/R` |
| `Composite Effective Average Tax Rate` | text | OECD EATR |
| `Composite Effective Marginal Tax Rate` | text | OECD EMTR |
| `data: tax feature list` | list.info_tax_feature | → `info_tax_feature._id[]` |
| `quick_view-NEW2` | text | Multilingual quick view |
| `update date` | date | Must be updated on every PATCH |
| `Created Date` | date | |
| `Modified Date` | date | |

### `info_tax_feature`
Detailed tax feature cards.

| Field | Type | Notes |
|-------|------|-------|
| `_id` | text | |
| `Slug` | text | |
| `belonging_tax_info` | info_tax | → `info_tax._id` |
| `header-new2` | text | Multilingual header |
| `body-new2` | text | Multilingual body (BBCode) |
| `reference` | text | Plain URL only (no BBCode) |
| `image` | image | |
| `Created Date` | date | |
| `Modified Date` | date | |

### `info:tax:vatrate`
Granular VAT/sales-tax rate entries.

| Field | Type | Notes |
|-------|------|-------|
| `_id` | text | |
| `Slug` | text | |
| `country_region` | info_country | → `info_country._id` |
| `tax_rate_type` | string | |
| `rate` | number | |
| `name-New2` | text | Multilingual name |
| `description-new2` | text | Multilingual description |
| `active` | boolean | |
| `Created Date` | date | |
| `Modified Date` | date | |

### `info_company_type`
Company-type / legal-entity templates per jurisdiction.

| Field | Type | Notes |
|-------|------|-------|
| `_id` | text | |
| `Slug` | text | |
| `country_region` | info_country | → `info_country._id` |
| `legal_entity_full_name` | text | |
| `legal_entity_ abbreviation` | text | |
| `limited_liability` | string | |
| `local_director_not_mandatory` | string | |
| `local_secretary_not_mandatory` | string | |
| `legal_representative_not_mandatory` | string | |
| `capital_injection_not_mandatory` | string | |
| `publicly_participates_in_capital_market` | string | |
| `ownership` | string | |
| `minimum_registered_capital-NEW2` | text | |
| `requirements_for_capital_injection-NEW2` | text | Multilingual |
| `requirements_for_directors-NEW2` | text | Multilingual |
| `requirements_for_shareholders-NEW2` | text | Multilingual |
| `quick_view_NEW2` | text | Multilingual |
| `memo_new2` | text | Multilingual |
| `references` | text | |
| `incorporation-product-list` | list.product:all | → `product:all._id[]` |
| `Created Date` | date | |
| `Modified Date` | date | |
