# TKEG Product / Service / Supply API Reference

## Core Rules
- Prioritize `main_product = true` matches before non-main variants.
- When a user asks for a product's price, they mean the complete solution including sub-products needed to satisfy requirements.
- Always check `supply_info` and inspect the supply record for additional context.
- Always check `supply_requirement` — requirements determine completion conditions and extra fees.

## Product / Service Workflow

1. Resolve `product_id` from user input:
   - Website link with `dynamic` in path → next segment is likely a `Slug` or `_id`.
   - If `_id` known → skip to step 2.
   - If product name or `Slug` → resolve likely `service_type`, check `product-service-cache.md` for cached `_id`.
   - If not cached → query `product:all` constrained by `service_type`. Prefer `main_product = true`.
2. Fetch `product:all/<product_id>`.
3. If `main_product = true` and not cached → add name, `Slug`, `_id` to `product-service-cache.md` under the service category.
4. Read `supply_info` from the product.
5. Fetch `supply_all/<supply_id>`.
6. Check `supply_included_service` for included/excluded service rows.
7. Check `supply_requirement` for conditions, completion requirements, extra-fee logic.
8. Check `supply_requirement_document` for required client documents.
9. If country applicability matters, query `info_country` and map jurisdiction IDs.
10. If a requirement has `solution_specify_supply`, map those `supply_all` IDs back to products via `product:all.supply_info`.
11. In the answer, clearly separate: what the product is, what it includes, conditions, documents needed, and potential extra fees.

## Service Types

- `company-incorporation`
- `bank-account-opening`
- `accounting`
- `consulting`
- `ready-made-company`
- `registered-address`
- `nominee-director`
- `company-secretary`
- `company-dissolution`
- `tax-registration`
- `special-license`
- `annual-return`
- `company-amendment`
- `administration-fee`
- `other-services`

## Endpoints

| Object | Path |
|--------|------|
| Product list | `/api/1.1/obj/product:all` |
| Single product | `/api/1.1/obj/product:all/<product_id>` |
| Single supply | `/api/1.1/obj/supply_all/<supply_id>` |
| Included services | `/api/1.1/obj/supply_included_service` |
| Document requirements | `/api/1.1/obj/supply_requirement_document` |
| Conditional requirements | `/api/1.1/obj/supply_requirement` |
| FAQ | `/api/1.1/obj/frequent_questions` |
| FAQ | `/api/1.1/obj/frequent_questions` |

## Data Structure

### `product:all`
Sellable products.

| Field | Type | Notes |
|-------|------|-------|
| `_id` | text | Unique product ID |
| `Slug` | text | Stable slug |
| `url_name` | text | Display name |
| `product-name-new2` | text | Multilingual product name |
| `service_type` | string | One of the service types above |
| `supply_info` | supply_all | → `supply_all._id` |
| `belonging_jurisdiction` | info_country | → `info_country._id` |
| `full-applicable-jurisdictions` | list.info_country | All applicable jurisdictions |
| `corporate_price` | number | Price |
| `default_marking_currency` | string | Currency code |
| `main_product` | boolean | Primary variant flag |
| `product_image` | image | |
| `case-study-projects-items` | list.project:projectitem | |
| `tkeg_product_id (New)` | text | |
| `TKEG Expat Ireland Stripe Price ID` | text | |
| `TKEG Expat US Stripe Price ID` | text | |
| `Created Date` | date | |
| `Modified Date` | date | |

### `supply_all`
Underlying service/supply definition behind products.

| Field | Type | Notes |
|-------|------|-------|
| `_id` | text | Unique supply ID |
| `Slug` | text | |
| `tkeg_supply_id` | text | |
| `supply_product_name` | text | |
| `service_type` | string | |
| `supplier_entity` | entity_supplier | |
| `belonging_jurisdiction` | info_country | → `info_country._id` |
| `applicable_jurisdiction` | list.info_country | |
| `full-applicable-jurisdiction-list` | list.info_country | All applicable countries |
| `data: document requirements` | list.supply_requirement_document | → `supply_requirement_document._id[]` |
| `data: requirements` | list.supply_requirement | → `supply_requirement._id[]` |
| `included services (new)` | list.supply_included_service | → `supply_included_service._id[]` |
| `product-element` | product:all | Back-reference to product |
| `quick_view_new2` | text | Multilingual quickview |
| `public_memo_new2` | text | Multilingual public memo |
| `internal_memo` | text | |
| `documentation` | text | |
| `supplier_original_words` | text | |
| `product_website` | text | |
| `marking_currency` | string | |
| `cost_price` | number | |
| `estimated-business-days` | number | |
| `estimated-government-fee` | number | |
| `product_valid` | string | |
| `has_product` | boolean | |
| `allow-for-direct-payment` | boolean | |
| `applicant must be present` | boolean | |
| `block-index` | boolean | |
| `qa-list` | list.frequent_questions | |
| `vat` | number | |
| `vat-charge-method` | string | |
| `vat-rate-type` | info:tax:vatrate | |
| `unit - new` | string | |
| `default-rd-entitu` | entity_rd | |
| `supplied-by-tkeg-expat-entity` | entity:tkegexpat | |
| `secondary: I/S: Company Type` | info_company_type | |
| `secondary: B: bank type` | string | |
| `secondary: S: bank_balance` | number | |
| `secondary: S: has_bank_account` | string | |
| `secondary: S: has_tax_id` | string | |
| `secondary: S: no_operation` | string | |
| `secondary: T: tax_id_type` | string | |
| `secondary: VA: Address` | geographic_address | |
| `Created Date` | date | |
| `Modified Date` | date | |

### `supply_included_service`
Services included (or explicitly excluded) within a supply.

| Field | Type | Notes |
|-------|------|-------|
| `_id` | text | |
| `Slug` | text | |
| `belonging-supply` | supply_all | → `supply_all._id` |
| `service-type` | string | |
| `service-name_new2` | text | Multilingual label |
| `quantity-included` | number | |
| `included service (supply)` | supply_all | Nested included supply ID |
| `not-included` | boolean | Explicit exclusion marker |
| `image` | image | |
| `Created Date` | date | |
| `Modified Date` | date | |

### `supply_requirement_document`
Client documents required for a supply.

| Field | Type | Notes |
|-------|------|-------|
| `_id` | text | |
| `Slug` | text | |
| `belonging_supply` | supply_all | → `supply_all._id` |
| `applicable_entity` | string | |
| `document_type` | string | |
| `document_format` | string | |
| `document_process` | string | |
| `memo-NEW2` | text | Multilingual memo |
| `Created Date` | date | |
| `Modified Date` | date | |

### `supply_requirement`
Conditions/requirements attached to a supply.

| Field | Type | Notes |
|-------|------|-------|
| `_id` | text | |
| `Slug` | text | |
| `supply` | supply_all | → `supply_all._id` |
| `service_type` | string | |
| `requirement_name-NEW2` | text | Multilingual name |
| `question_prompt-NEW2` | text | Multilingual prompt |
| `condition-NEW2` | text | Multilingual condition |
| `has solution` | boolean | If false, client must resolve themselves |
| `solution_specify_supply` | list.supply_all | → `supply_all._id[]` (not product IDs) |
| `solution_specify_supplier` | entity_supplier | Supplier reference |
| `item_included` | number | If > 0, original supply already covers this |
| `max_item_allowed` | number | |
| `Created Date` | date | |
| `Modified Date` | date | |

**Requirement logic:**
- If `has solution` = false → client must satisfy the requirement themselves.
- If `item_included > 0` → already covered by the supply (up to that quantity).
- If `solution_specify_supply` present → only those supplies can solve the requirement; map back to products via `product:all.supply_info`.
- If no `solution_specify_supply` → query `supply_all` by `service_type` + jurisdiction + supplier to find solutions.
- Many `supply_all` records have no selling `product:all` — always confirm a sellable product exists.
- Requirements may imply extra fees — do not skip them in product answers.

### `frequent_questions`
Multilingual FAQ entries.

| Field | Type | Notes |
|-------|------|-------|
| `_id` | text | |
| `Slug` | text | |
| `question_new2` | text | Multilingual question |
| `answer_new2` | text | Multilingual answer |
| `specified-jurisdictions` | list.info_country | |
| `specified-service-types` | list.string | |
| `Created Date` | date | |
| `Modified Date` | date | |
