# Cached Product Names / Slug / ID

Only cache these 3 fields: **Name, Slug, _id**. And only cache if `main_product = true`.

### annual-return
| Name | Slug | _id |
|---|---|---|
| U.K. Company Annual Confirmation Statement | united-kingdom-annual-confirmation-statement-1 | 1707576956745x291934909640736800 |
| U.S. Delaware Company Annual Return | united-states-delaware-annual-return-1 | 1707577048477x707235810076524500 |
| Estonian Company Dormancy Report | estonia-annual-return-1 | 1707577149625x983005754155860000 |

### company-incorporation
| Name | Slug | _id |
|---|---|---|
| British LTD Company Registration | united-kingdom-ltd-company-incorporation-1 | 1707842044439x932829192081899500 |
| Estonian OU Company Incorporation | estonia-ou-company-incorporation-1 | 1707842053941x721882660968857600 |
| Irish LTD company registration | ireland-ltd-company-incorporation-1 | 1707842062248x364908618788896800 |
| Singapore PTE LTD Company Incorporation | singapore-pte-ltd-company-incorporation-1 | 1709554865677x848498245046370300 |

## API Endpoint Format

**IMPORTANT:** The product type name is `product:all` — `:all` is **part of the type name**, not a list-mode suffix. Always include it in both list AND single-record URLs:
- ✅ `/api/1.1/obj/product:all` — list
- ✅ `/api/1.1/obj/product:all/<id>` — single record
- ❌ `/api/1.1/obj/product` — 404 "Type not found"
- ❌ `/api/1.1/obj/product/<id>` — 404 "Type not found"

**Constraint Query Format:**
```
/api/1.1/obj/product:all?constraints=[{"key":"service_type","constraint_type":"equals","value":"company-incorporation"}]&limit=100
```

URL-encode the constraints JSON array when using in query string.