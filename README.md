# TKEG Expat Public Data API — Claude Code Skill

Query **company incorporation, tax, pricing, and jurisdictional data** across **89 countries** using [TKEG Expat](https://tkegexpat.com)'s public API — no authentication required.

Built as a [Claude Code](https://claude.ai/code) skill for AI-assisted business research.

## What's inside

| Data | Coverage |
|------|----------|
| **Products & pricing** | Company incorporation, bank accounts, accounting, registered addresses, and 15 service types |
| **Supply details** | Included services, document requirements, conditional requirements, extra fees |
| **Jurisdictions** | 89 countries across Europe, Asia, Americas, Middle East, Oceania, Africa |
| **Tax info** | Corporate income tax, VAT rates, capital gains, withholding tax, OECD effective rates |
| **Company types** | Legal entity structures per jurisdiction (LTD, LLC, GmbH, OÜ, PTE LTD, etc.) |
| **FAQ** | Multilingual Q&A per product and jurisdiction |

## Quick start

### Use with Claude Code

Add this skill to your Claude Code workspace:

```bash
# Clone into your skills directory
git clone https://github.com/tkeg-expat/tkeg-expat-api.git skills/tkeg-api
```

Then ask Claude things like:

- *"What's the corporate tax rate in Estonia?"*
- *"Show me company incorporation products for Singapore"*
- *"What documents are needed to register a UK LTD?"*
- *"Compare tax rates between Ireland and Netherlands"*

### Use the API directly

The API is public and requires no authentication:

```bash
# List all countries
curl -A "MyApp/1.0" "https://tkegexpat.com/api/1.1/obj/info_country?limit=100"

# Get a specific country (e.g., Estonia)
curl -A "MyApp/1.0" "https://tkegexpat.com/api/1.1/obj/info_country/1708049142555x508322484765589500"

# Search products by service type
curl -A "MyApp/1.0" "https://tkegexpat.com/api/1.1/obj/product:all?constraints=%5B%7B%22key%22%3A%22service_type%22%2C%22constraint_type%22%3A%22equals%22%2C%22value%22%3A%22company-incorporation%22%7D%5D&limit=100"
```

> **Note:** Always set a `User-Agent` header — requests with empty UA may be blocked.

## Covered jurisdictions

<details>
<summary>89 countries and regions (click to expand)</summary>

### Europe
Austria, Belgium, Bermuda, Bulgaria, Croatia, Cyprus, Czechia, Denmark, Estonia, Finland, France, Germany, Gibraltar, Greece, Hungary, Iceland, Ireland, Italy, Latvia, Liechtenstein, Lithuania, Luxembourg, Malta, Monaco, Netherlands, Norway, Poland, Portugal, Romania, Slovakia, Slovenia, Spain, Sweden, Switzerland, Turkey, Ukraine, United Kingdom

### East Asia
Chinese Mainland, Chinese Taipei, Hong Kong SAR, Japan, Macau SAR, Philippines, South Korea

### South & Southeast Asia
Malaysia, Singapore, Thailand, Vietnam

### North America
Canada, Guatemala, Mexico, Puerto Rico, United States

### Central America
Belize, Costa Rica, El Salvador, Nicaragua, Panama

### Caribbean
British Virgin Islands, Cayman Islands, Dominican Republic

### South America
Argentina, Bolivia, Brazil, Chile, Colombia, Ecuador, Paraguay, Peru, Uruguay

### Middle East / West Asia
Israel, Kazakhstan, United Arab Emirates

### Oceania
Australia, Marshall Islands, New Zealand

### Africa
Egypt, Seychelles, South Africa

### Regional groupings
ASEAN, EEA, EEU, EU, GCA (Greater China), GUK, GUS (Greater US), USAN, USMCA/NAFTA, Global

</details>

## Service types

| Service | Slug |
|---------|------|
| Company Incorporation | `company-incorporation` |
| Bank Account Opening | `bank-account-opening` |
| Accounting | `accounting` |
| Consulting | `consulting` |
| Ready-made Company | `ready-made-company` |
| Registered Address | `registered-address` |
| Nominee Director | `nominee-director` |
| Company Secretary | `company-secretary` |
| Company Dissolution | `company-dissolution` |
| Tax Registration | `tax-registration` |
| Special License | `special-license` |
| Annual Return | `annual-return` |
| Company Amendment | `company-amendment` |
| Administration Fee | `administration-fee` |
| Other Services | `other-services` |

## API endpoints

| Data | Endpoint |
|------|----------|
| Products | `/api/1.1/obj/product:all` |
| Supplies | `/api/1.1/obj/supply_all` |
| Included services | `/api/1.1/obj/supply_included_service` |
| Document requirements | `/api/1.1/obj/supply_requirement_document` |
| Conditional requirements | `/api/1.1/obj/supply_requirement` |
| FAQ | `/api/1.1/obj/frequent_questions` |
| Countries | `/api/1.1/obj/info_country` |
| Country features | `/api/1.1/obj/info_country_feature` |
| Provinces / states | `/api/1.1/obj/info-province-state` |
| Tax summary | `/api/1.1/obj/info_tax` |
| Tax features | `/api/1.1/obj/info_tax_feature` |
| VAT / sales tax rates | `/api/1.1/obj/info:tax:vatrate` |
| Company types | `/api/1.1/obj/info_company_type` |

Base URL: `https://tkegexpat.com`

See [`references/`](references/) for full field-level documentation.

## Multilingual support

All content fields support multiple languages via `*-NEW2` / `*_new2` fields, with structured multilingual text that can be parsed per locale.

## File structure

```
tkeg-api/
├── SKILL.md                              # Skill entry point and routing
├── references/
│   ├── product-service-api.md            # Product/supply/pricing data structures
│   └── business-data-api.md              # Country/tax/company-type data structures
└── cache/
    ├── product-service-cache.md          # Product name → ID lookup
    └── info-country-cache.md             # Country name → ID lookup (89 countries)
```

## About TKEG Expat

[TKEG Expat](https://tkegexpat.com) provides business services for expatriates and international entrepreneurs — company incorporation, accounting, tax registration, and more across 89 jurisdictions worldwide.

## License

This skill definition and documentation are provided for public use. The underlying API data is served by TKEG Expat.
