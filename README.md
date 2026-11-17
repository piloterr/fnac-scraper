<p align="center">
  <a href="https://www.piloterr.com"><img src="images/banner.png" alt="Fnac Scraper API by Piloterr – scrape Fnac data as JSON" width="100%"></a>
</p>

# Fnac Scraper API

[![Discord](https://img.shields.io/badge/Discord-Join%20the%20community-5865F2?logo=discord&logoColor=white)](https://discord.gg/CFj3ph8aHU) [![Get API key](https://img.shields.io/badge/Piloterr-Get%20a%20free%20API%20key-111827)](https://app.piloterr.com) [![API docs](https://img.shields.io/badge/Docs-OpenAPI-0EA5E9)](https://app.piloterr.com/openapi) [![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

The **Piloterr Fnac Scraper API** lets you extract public data from Fnac as **clean, structured JSON** — no proxies, headless browsers or HTML parsing to maintain. Endpoints available in production:

- **Fnac Product API** — Read one Fnac product from its URL or product id.
- **Fnac Search API** — Search Fnac for a keyword or a SearchResult URL.

Use it for price monitoring, assortment tracking, competitor analysis, product research or catalog enrichment.

| Endpoint | Path | Credits | What you get |
|---|---|---|---|
| [Fnac Product API](#fnac-product-api) | `/v2/fnac/product` | 2 | Read one Fnac product from its URL or product id. You get the price, brand, EAN, seller, and rating. |
| [Fnac Search API](#fnac-search-api) | `/v2/fnac/search` | 2 | Search Fnac for a keyword or a SearchResult URL. Each card has the product id, name, price, seller, and stock. |

## Table of contents

- [How it works](#how-it-works)
- [Fnac Product API](#fnac-product-api)
- [Fnac Search API](#fnac-search-api)
- [Code examples in other languages](#code-examples-in-other-languages)
- [Practical use cases](#practical-use-cases)
- [Why Piloterr?](#why-piloterr)
- [FAQ](#faq)
- [Official SDKs](#official-sdks)
- [Related scrapers](#related-scrapers)
- [Support](#support)

## How it works

1. **Create a free Piloterr account** and copy your API key from the [dashboard](https://app.piloterr.com).
2. **Send a `GET` (or `POST`) request** to `https://api.piloterr.com/v2/...` with the header `x-api-key: YOUR_API_KEY`.
3. **Receive structured JSON** — Piloterr handles rendering, proxies, retries and parsing.

```bash
export PILOTERR_API_KEY="your-api-key"
pip install requests        # Python
# TypeScript: Node.js 18+ (native fetch), run with `npx tsx file.ts`
```

> Every endpoint accepts both `GET` (query string) and `POST` (JSON body with the same fields).

## Fnac Product API

Read one Fnac product from its URL or product id. You get the price, brand, EAN, seller, and rating.

`GET | POST https://api.piloterr.com/v2/fnac/product` · **2 credits per request** · [Full reference](https://app.piloterr.com/openapi)

### Request parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `query` | string | ✅ | Product URL (/a{id} or /mp{id}) or the numeric Fnac id. A keyword or a search URL returns 400. |
| `return_page_source` | boolean | — | When true, returns the raw HTML instead of the parsed JSON. |

### Python example

```python
import json
import os
import requests

response = requests.get(
    "https://api.piloterr.com/v2/fnac/product",
    headers={"x-api-key": os.environ["PILOTERR_API_KEY"]},
    params={
        "query": "23428278",
    },
    timeout=120,
)
response.raise_for_status()
data = response.json()

print(json.dumps(data, indent=2, ensure_ascii=False)[:2000])
```

### TypeScript example

```typescript
type FnacProduct = {
  ean?: string;
  sku?: string;
  url?: string;
  name?: string;
  brand?: string;
  price?: number;
  images?: string[];
  rating?: number;
  seller?: string;
  currency?: string;
  in_stock?: boolean;
  condition?: string;
  categories?: string[];
  product_id?: string;
  description?: string;
  review_count?: number;
  price_display?: string;
};

const params = new URLSearchParams({
  "query": "23428278",
});

const response = await fetch(`https://api.piloterr.com/v2/fnac/product?${params}`, {
  headers: { "x-api-key": process.env.PILOTERR_API_KEY! },
});
if (!response.ok) throw new Error(`Piloterr error ${response.status}: ${await response.text()}`);

const data = (await response.json()) as FnacProduct;

console.log(JSON.stringify(data, null, 2).slice(0, 2000));
```

<details>
<summary><b>POST variant</b></summary>

```http
POST https://api.piloterr.com/v2/fnac/product
x-api-key: YOUR_API_KEY
Content-Type: application/json

{"query": "23428278"}
```
</details>

### Output example

Example response from the API reference, abridged — real values depend on the page you scrape.

```json
{
  "ean": "0195951416793",
  "sku": "9346245",
  "url": "https://www.fnac.com/Apple-iPhone-18-Pro-6-3-5G-Double-SIM-256-Go-Argent/a23428278/w-4",
  "name": "Apple iPhone 18 Pro 6,3\" 5G Double SIM 256 Go Argent",
  "brand": "Apple",
  "price": 1479,
  "images": [
    "https://static.fnac-static.com/multimedia/Images/FR/MDM/a3/73/c7/29848483/3756-1/tsp20261005171514/Apple-iPhone-18-Pro-6-3-5G-Double-SIM-…"
  ],
  "rating": 5,
  "seller": "example",
  "currency": "EUR",
  "in_stock": true,
  "condition": "neuf",
  "categories": [
    "Smartphones et Objets Connectés",
    "iPhone"
  ],
  "product_id": "23428278",
  "description": "L'iPhone 18 Pro est conçu pour livrer des performances sans précédent.",
  "review_count": 4,
  "price_display": "1 479 €"
}
```

### Response fields

| Field | Type | Description |
|---|---|---|
| `ean` | string |  |
| `sku` | string |  |
| `url` | string |  |
| `name` | string |  |
| `brand` | string |  |
| `price` | integer |  |
| `images` | string[] |  |
| `rating` | integer |  |
| `seller` | string |  |
| `currency` | string |  |
| `in_stock` | boolean |  |
| `condition` | string |  |
| `categories` | string[] |  |
| `product_id` | string |  |
| `description` | string |  |
| `review_count` | integer |  |
| `price_display` | string |  |

## Fnac Search API

Search Fnac for a keyword or a SearchResult URL. Each card has the product id, name, price, seller, and stock.

`GET | POST https://api.piloterr.com/v2/fnac/search` · **2 credits per request** · [Full reference](https://app.piloterr.com/openapi)

### Request parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `query` | string | ✅ | Keyword or a Fnac SearchResult URL. A product URL returns 400. |
| `page` | number | — | Page number, from 1. Default is 1. Page 2 is sent as PageIndex=2. |
| `return_page_source` | boolean | — | When true, returns the raw HTML instead of the parsed JSON. |

### Python example

```python
import json
import os
import requests

response = requests.get(
    "https://api.piloterr.com/v2/fnac/search",
    headers={"x-api-key": os.environ["PILOTERR_API_KEY"]},
    params={
        "query": "iphone",
    },
    timeout=120,
)
response.raise_for_status()
data = response.json()

print(json.dumps(data, indent=2, ensure_ascii=False)[:2000])
```

### TypeScript example

```typescript
type FnacSearch = {
  results?: {
    url?: string;
    name?: string;
    brand?: string;
    image?: string;
    price?: number;
    rating?: number;
    seller?: string;
    category?: string;
    currency?: string;
    in_stock?: boolean;
    position?: number;
    condition?: string;
    product_id?: string;
    offers_from?: number;
    review_count?: number;
    price_display?: string;
    offers_from_display?: string;
  }[];
  pagination?: {
    next?: string;
    page?: number;
    per_page?: number;
  };
};

const params = new URLSearchParams({
  "query": "iphone",
});

const response = await fetch(`https://api.piloterr.com/v2/fnac/search?${params}`, {
  headers: { "x-api-key": process.env.PILOTERR_API_KEY! },
});
if (!response.ok) throw new Error(`Piloterr error ${response.status}: ${await response.text()}`);

const data = (await response.json()) as FnacSearch;

console.log(JSON.stringify(data, null, 2).slice(0, 2000));
```

<details>
<summary><b>POST variant</b></summary>

```http
POST https://api.piloterr.com/v2/fnac/search
x-api-key: YOUR_API_KEY
Content-Type: application/json

{"query": "iphone"}
```
</details>

### Output example

Example response from the API reference, abridged — real values depend on the page you scrape.

```json
{
  "results": [
    {
      "url": "https://www.fnac.com/Apple-iPhone-18-Pro-6-3-5G-Double-SIM-256-Go-Argent/a23428278/w-4",
      "name": "Apple iPhone 18 Pro 6,3\" 5G Double SIM 256 Go Argent",
      "brand": "Apple",
      "image": "https://static.fnac-static.com/multimedia/Images/FR/MDM/a3/73/c7/29848483/1545-1/tsp20261005171514/Apple-iPhone-18-Pro-6-3-5G-Double-SIM-…",
      "price": 1479,
      "rating": 5,
      "seller": "example",
      "category": "iPhone",
      "currency": "EUR",
      "in_stock": true,
      "position": 1,
      "condition": "neuf",
      "product_id": "23428278",
      "offers_from": 1352,
      "review_count": 4,
      "price_display": "1 479 €",
      "offers_from_display": "1 352 €"
    }
  ],
  "pagination": {
    "next": "https://www.fnac.com/SearchResult/ResultList.aspx?PageIndex=2&Search=iphone&sft=1",
    "page": 1,
    "per_page": 20
  }
}
```

### Response fields

| Field | Type | Description |
|---|---|---|
| `results[]` | object[] |  |
| `results[].url` | string |  |
| `results[].name` | string |  |
| `results[].brand` | string |  |
| `results[].image` | string |  |
| `results[].price` | integer |  |
| `results[].rating` | integer |  |
| `results[].seller` | string |  |
| `results[].category` | string |  |
| `results[].currency` | string |  |
| `results[].in_stock` | boolean |  |
| `results[].position` | integer |  |
| `results[].condition` | string |  |
| `results[].product_id` | string |  |
| `results[].offers_from` | integer |  |
| `results[].review_count` | integer |  |
| `results[].price_display` | string |  |
| `results[].offers_from_display` | string |  |
| `pagination` | object |  |
| `pagination.next` | string |  |
| `pagination.page` | integer |  |
| `pagination.per_page` | integer |  |

## Code examples in other languages

Ready-to-run examples live in the [`examples/`](examples) folder. Each one reads your key from the `PILOTERR_API_KEY` environment variable.

| Language | Product | Search |
|---|---|---|
| cURL (Bash) | [`product`](examples/curl/product.sh) | [`search`](examples/curl/search.sh) |
| PHP | [`product`](examples/php/product.php) | [`search`](examples/php/search.php) |
| C# | [`product`](examples/csharp/Product.cs) | [`search`](examples/csharp/Search.cs) |
| Go | [`product`](examples/go/product.go) | [`search`](examples/go/search.go) |
| Java | [`product`](examples/java/ProductExample.java) | [`search`](examples/java/SearchExample.java) |
| Ruby | [`product`](examples/ruby/product.rb) | [`search`](examples/ruby/search.rb) |
| Node.js | [`product`](examples/nodejs/product.mjs) | [`search`](examples/nodejs/search.mjs) |

## Practical use cases

- **Price monitoring** — Track prices, promotions and availability over time to react to competitors.
- **Assortment & catalog tracking** — Follow new products, delisted items and category changes.
- **Competitor analysis** — Benchmark ranges, brands and positioning across retailers.
- **Product research** — Spot best-sellers and trends from search results.
- **Catalog enrichment** — Pull titles, images and attributes into your PIM or marketplace listings.

## Why Piloterr?

- **No infrastructure** — no proxies, headless browsers or CAPTCHA solving to run yourself.
- **Structured JSON** — stable, documented fields instead of brittle HTML selectors.
- **Pay per request** — transparent credit cost per endpoint, failed requests are retried for you.
- **One API key, hundreds of sources** — Fnac plus many other e-commerce, search and company data endpoints in the [Piloterr library](https://www.piloterr.com/library).
- **Any language** — plain HTTPS + JSON, with examples for Python, TypeScript, cURL, PHP, C#, Go, Java, Ruby and Node.js.

## FAQ

### Is it legal to scrape Fnac?

Collecting publicly available data is generally allowed in many jurisdictions, but rules depend on your country, the data you collect and how you use it. Avoid personal data unless you have a lawful basis, respect the website's terms where they apply, and get legal advice for your specific use case. This is not legal advice.

### Do I need proxies or a headless browser?

No. Piloterr handles rendering, proxy rotation and retries on its side. You only send an HTTPS request with your `x-api-key` header and receive JSON.

### How much does a request cost?

Credits per call: `/v2/fnac/product` **2**, `/v2/fnac/search` **2**. See the [Piloterr website](https://www.piloterr.com) for plans.

### What data does the Fnac API return?

Each endpoint returns structured JSON: **Fnac Product** — `ean`, `sku`, `url`, `name`, `brand`, `price`; **Fnac Search** — `results[]`, `results[].url`, `results[].name`, `results[].brand`, `results[].image`, `results[].price`.

### How do I paginate Fnac results?

Use the `page` parameter and keep requesting while the response indicates more results.

### Can I get the raw HTML instead of JSON?

Yes. Set `return_page_source=true` and the response becomes `{"result": "..."}` with the page source.

### Which languages are supported?

Any language that can make an HTTP request. This README shows Python and TypeScript; the [`examples/`](examples) folder contains cURL, PHP, C#, Go, Java, Ruby and Node.js.

## Official SDKs

Use Piloterr from your own language with a typed client:

| SDK | Package | Install |
| --- | --- | --- |
| Node.js | [`@piloterr/sdk`](https://www.npmjs.com/package/@piloterr/sdk) | `npm install @piloterr/sdk` |
| Python | [`piloterr`](https://pypi.org/project/piloterr/) | `pip install piloterr` |
| Go | [`piloterr-go`](https://pkg.go.dev/github.com/piloterr/piloterr-go) | `go get github.com/piloterr/piloterr-go` |
| .NET | [`Piloterr`](https://www.nuget.org/packages/Piloterr) | `dotnet add package Piloterr` |
| Ruby | [`piloterr`](https://rubygems.org/gems/piloterr) | `gem install piloterr` |
| PHP | [`piloterr/sdk`](https://packagist.org/packages/piloterr/sdk) | `composer require piloterr/sdk` |
| Java | [`piloterr-java`](https://github.com/piloterr/piloterr-java) | build from source, see its README |
| CLI | [`@piloterr/cli`](https://www.npmjs.com/package/@piloterr/cli) | `npm install -g @piloterr/cli` |
| MCP server | [`@piloterr/mcp`](https://www.npmjs.com/package/@piloterr/mcp) | `npx -y @piloterr/mcp` |

## Related scrapers

- [Jobs Scraper API](https://github.com/piloterr/jobs-scraper)
- [About You Scraper API](https://github.com/piloterr/aboutyou-scraper)
- [Action Scraper API](https://github.com/piloterr/action-scraper)
- [Aldi Scraper API](https://github.com/piloterr/aldi-scraper)
- [Alibaba Scraper API](https://github.com/piloterr/alibaba-scraper)
- [AliExpress Scraper API](https://github.com/piloterr/aliexpress-scraper)
- [All Piloterr scrapers on GitHub](https://github.com/piloterr)

## Support

- 💬 Join the [Piloterr Discord](https://discord.gg/CFj3ph8aHU) to ask questions and share what you build.
- 📚 Browse the [API reference (OpenAPI)](https://app.piloterr.com/openapi) and the [Piloterr library](https://www.piloterr.com/library).
- 🔑 [Get your API key](https://app.piloterr.com).

---

<p align="center">
  <a href="https://www.piloterr.com">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="images/logo-dark.svg">
      <img src="images/logo.svg" alt="Piloterr" height="32">
    </picture>
  </a>
  <br>
  <sub>Built by <a href="https://www.piloterr.com">Piloterr</a> — web scraping APIs for e-commerce, search, real estate and more, one API key.</sub>
</p>

<sub>Fnac is a trademark of its respective owner. This repository is not affiliated with or endorsed by Fnac. Examples are released under the MIT license.</sub>
