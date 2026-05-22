# Agenty AI Agent Skills

[Agenty AI](https://agenty.com/) provides a set of browser automation APIs that AI agents can call directly to interact with the web. Each skill maps to a single API endpoint. All requests are POST to `https://api.agenty.ai` with a JSON body.

Base URL: `https://api.agenty.ai`

Authentication: pass your API key as a Bearer token in the Authorization header.

```
Authorization: Bearer YOUR_API_KEY
```


## Screenshot

[Captures a screenshot](https://agenty.com/tools/screenshot) of any URL or HTML content using a real browser.

Endpoint: `POST https://api.agenty.ai/v1/screenshot`

```json
{
  "url": "https://example.com",
  "options": {
    "fullPage": true,
    "type": "png"
  },
  "responseType": "url"
}
```

<img width="1341" height="767" alt="image" src="https://github.com/user-attachments/assets/c3c5cbe5-0485-4f3e-872e-96f812dd292c" />

The response returns a hosted image URL when `responseType` is `url`, or a binary buffer when set to `buffer` (default).

Key parameters:

| Parameter | Type | Default | Description |
|---|---|---|---|
| `url` | string | — | Page URL to capture. Required if `html` is not set. |
| `html` | string | — | Raw HTML to render instead of a URL. |
| `options.fullPage` | boolean | `true` | Capture the full scrollable page. |
| `options.type` | string | `png` | Image format: `png` or `jpeg`. |
| `options.quality` | number | — | JPEG quality from 0 to 100. |
| `options.clip` | object | — | Crop to a region with `x`, `y`, `width`, `height`. |
| `options.omitBackground` | boolean | — | Transparent background (PNG only). |
| `responseType` | string | `buffer` | Return a binary `buffer` or a hosted `url`. |
| `manipulate` | object | — | Post-process with `resize`, `rotate`, `flip`, or `flop`. |
| `viewport` | object | — | Set `width`, `height`, `isMobile`, `deviceScaleFactor`. |
| `device` | string | — | Emulate a named device like `iPhone 14`. |
| `waitFor` | string or number | — | Wait for a selector or milliseconds before capturing. |
| `blockAds` | boolean | `false` | Block ad requests. |
| `blockTrackers` | boolean | `false` | Block tracking scripts. |

Example — mobile screenshot with resize:

```json
{
  "url": "https://agenty.ai",
  "device": "iPhone 14",
  "responseType": "url",
  "manipulate": {
    "resize": { "width": 390 }
  }
}
```


## PDF

[Generates a PDF](https://agenty.com/tools/pdf) from any URL or HTML content.

Endpoint: `POST https://api.agenty.ai/v1/pdf`

```json
{
  "url": "https://example.com",
  "options": {
    "format": "A4",
    "printBackground": true
  },
  "responseType": "url"
}
```

The response returns a hosted PDF URL when `responseType` is `url`, or a binary buffer (default).

Key parameters:

| Parameter | Type | Default | Description |
|---|---|---|---|
| `url` | string | — | Page URL to convert. Required if `html` is not set. |
| `html` | string | — | Raw HTML to render instead of a URL. |
| `options.format` | string | — | Page size: `A4`, `Letter`, `Legal`, `A3`, `A5`, etc. |
| `options.landscape` | boolean | — | Render in landscape mode. |
| `options.printBackground` | boolean | — | Include CSS backgrounds. |
| `options.margin` | object | — | Set `top`, `bottom`, `left`, `right` margins. |
| `options.displayHeaderFooter` | boolean | — | Show headers and footers. |
| `options.headerTemplate` | string | — | HTML string for the page header. |
| `options.footerTemplate` | string | — | HTML string for the page footer. |
| `options.pageRanges` | string | — | Print a range like `1-3`. |
| `options.scale` | number | — | Scale the page content. |
| `emulateMedia` | string | — | Render as `screen` or `print` media type. |
| `responseType` | string | `buffer` | Return a binary `buffer` or a hosted `url`. |
| `waitFor` | string or number | — | Wait for a selector or milliseconds before generating. |

Example — A4 landscape with custom margins:

```json
{
  "url": "https://example.com/report",
  "options": {
    "format": "A4",
    "landscape": true,
    "printBackground": true,
    "margin": { "top": "20mm", "bottom": "20mm" }
  },
  "responseType": "url"
}
```


## Scrape

Scrape given fields, data from a page after JavaScript execution.

Endpoint: `POST https://api.agenty.ai/v1/scrape`

```json
{
  "url": "https://example.com",
  "waitFor": 2000
}
```

Key parameters:

| Parameter | Type | Default | Description |
|---|---|---|---|
| `url` | string | — | Page URL to scrape. Required if `html` is not set. |
| `html` | string | — | Raw HTML to render instead. |
| `waitFor` | string or number | — | Wait for a CSS selector or milliseconds before returning. |
| `blockAds` | boolean | `true` | Block ad requests. |
| `blockTrackers` | boolean | `true` | Block tracking scripts. |
| `viewport` | object | — | Set width, height, and mobile emulation. |
| `cookies` | array | — | Inject cookies before page load. |
| `setExtraHTTPHeaders` | object | — | Add custom request headers. |
| `authenticate` | object | — | Set `username` and `password` for basic auth. |
| `goToOptions` | object | — | Set `timeout` and `waitUntil` strategy. |

Example — scrape a JavaScript-rendered page waiting for a specific element:

```json
{
  "url": "https://example.com/dashboard",
  "waitFor": ".data-table",
  "blockAds": true
}
```


## Content

Returns the full rendered HTML of a page. Optimized for reading the final DOM state after all scripts have run.

Endpoint: `POST https://api.agenty.ai/v1/content`

```json
{
  "url": "https://example.com"
}
```

Accepts the same parameters as scrape. Use this when you need a clean read of the page's rendered output for further processing.


## Markdown

[Converts a webpage to clean Markdown](https://agenty.com/tools/markdown) by stripping navigation, scripts, footers, and other boilerplate.

Endpoint: `POST https://api.agenty.ai/v1/markdown`

```json
{
  "url": "https://example.com/article",
  "removeExternalLinks": true,
  "relativeToAbsoluteLinks": true
}
```

Key parameters:

| Parameter | Type | Default | Description |
|---|---|---|---|
| `url` | string | — | Page URL to convert. Required if `html` is not set. |
| `html` | string | — | Raw HTML to convert. |
| `ignoreTags` | array | `['nav', 'footer', 'header', 'script', ...]` | HTML tags to strip before conversion. |
| `removeExternalLinks` | boolean | `true` | Remove links pointing to external domains. |
| `relativeToAbsoluteLinks` | boolean | `true` | Resolve relative hrefs to full URLs. |
| `removeDataImages` | boolean | `true` | Strip base64-encoded inline images. |
| `waitFor` | string or number | — | Wait for content to load before converting. |
| `blockAds` | boolean | `true` | Block ad requests. |

Example — convert a Wikipedia article to Markdown without external links:

```json
{
  "url": "https://en.wikipedia.org/wiki/Web_scraping",
  "removeExternalLinks": true,
  "relativeToAbsoluteLinks": true
}
```


## Links

Extracts all hyperlinks from a rendered page.

Endpoint: `POST https://api.agenty.ai/v1/links`

```json
{
  "url": "https://example.com"
}
```

Returns an array of href values found on the page after JavaScript execution. Accepts the same navigation parameters as scrape.

Example — extract all links from a blog index:

```json
{
  "url": "https://example.com/blog",
  "waitFor": ".post-list"
}
```


## Extract

Extracts structured data from a rendered page using selectors or instructions.

Endpoint: `POST https://api.agenty.ai/v1/extract`

```json
{
  "url": "https://example.com/products"
}
```

Accepts the same navigation parameters as scrape. Use this when you need to pull specific fields — product prices, headlines, table rows, or any repeating content — from a rendered page.

Example — extract data after scrolling and waiting for content:

```json
{
  "url": "https://example.com/listings",
  "waitFor": ".listing-card",
  "blockAds": true
}
```


## Redirects

Traces the full HTTP redirect chain for a URL.

Endpoint: `POST https://api.agenty.ai/v1/redirects`

```json
{
  "url": "https://bit.ly/some-link"
}
```

Returns each hop in the redirect chain including the URL, status code, and final destination. Useful for auditing shortened links, verifying canonical URLs, and debugging redirect loops.

Example:

```json
{
  "url": "https://example.com/old-page",
  "goToOptions": {
    "waitUntil": "domcontentloaded"
  }
}
```


## Sitemap

Discovers and [parses all sitemaps for a website](https://agenty.com/tools/sitemap-crawler).

Endpoint: `POST https://api.agenty.ai/v1/sitemap`

```json
{
  "url": "https://example.com",
  "findSitemaps": true
}
```

Key parameters:

| Parameter | Type | Default | Description |
|---|---|---|---|
| `url` | string | — | Root domain to discover sitemaps for. |
| `exclusions` | array | `[]` | URL patterns to exclude from results. |
| `findSitemaps` | boolean | `true` | Automatically find all linked sitemap files. |

Example — get all URLs excluding the blog:

```json
{
  "url": "https://example.com",
  "findSitemaps": true,
  "exclusions": ["/blog/"]
}
```


## Emails

Scrapes email addresses from one or more rendered pages.

Endpoint: `POST https://api.agenty.ai/v1/emails`

```json
{
  "url": "https://example.com/contact"
}
```

You can also pass multiple URLs in a single request (up to 10):

```json
{
  "urls": [
    "https://example.com/contact",
    "https://example.com/about",
    "https://example.com/team"
  ]
}
```

Key parameters:

| Parameter | Type | Description |
|---|---|---|
| `url` | string | Single page URL to scrape. Required if `urls` is not set. |
| `urls` | array | List of up to 10 URLs to scrape in one request. |


## Snapshot

Captures the accessibility tree of a page as a structured object.

Endpoint: `POST https://api.agenty.ai/v1/snapshot`

```json
{
  "url": "https://example.com"
}
```

Returns a structured list of all interactive and readable elements on the page — buttons, links, inputs, headings, and landmarks. Designed for AI agents that need to understand or navigate a page without parsing raw HTML.

Key parameters:

| Parameter | Type | Description |
|---|---|---|
| `url` | string | The page URL to snapshot. |


## Common Request Options

All navigation-based endpoints (screenshot, pdf, scrape, content, markdown, links, extract, redirects) accept these shared options:

| Parameter | Type | Description |
|---|---|---|
| `userAgent` | string | Override the browser user agent. |
| `viewport` | object | Set `width`, `height`, `isMobile`, `hasTouch`, `deviceScaleFactor`. |
| `device` | string | Emulate a named device like `iPhone 14` or `iPad Pro`. |
| `cookies` | array | Inject cookies with `name`, `value`, `domain`, and other standard fields. |
| `setExtraHTTPHeaders` | object | Add custom headers to every request the browser makes. |
| `authenticate` | object | Set `username` and `password` for HTTP basic auth. |
| `blockAds` | boolean | Block ad network requests. |
| `blockTrackers` | boolean | Block analytics and tracking scripts. |
| `rejectRequestPattern` | array | Block any requests matching these URL patterns. |
| `rejectResourceTypes` | array | Block resource types like `image`, `font`, `media`, `stylesheet`. |
| `requestInterceptors` | array | Match request patterns and return a custom response body. |
| `addScriptTag` | array | Inject script tags into the page before capturing. |
| `addStyleTag` | array | Inject style tags into the page before capturing. |
| `goToOptions.timeout` | number | Navigation timeout in milliseconds. Min 100, max 60000. Default 30000. |
| `goToOptions.waitUntil` | string | When to consider navigation done: `load`, `domcontentloaded`, `networkidle0`, `networkidle2`. |
| `waitFor` | string or number | Wait for a CSS selector to appear or a number of milliseconds before capturing. |
| `anonymous.proxy` | string or boolean | Route through a proxy for anonymous requests. |
| `anonymous.skipResourceTypes` | array | Skip loading these resource types when using anonymous mode. |


## Error Responses

All endpoints return standard HTTP status codes.

| Status | Meaning |
|---|---|
| 200 | Success |
| 400 | Bad request — check your request body for missing or invalid fields |
| 401 | Unauthorized — API key is missing or invalid |
| 422 | Validation error — the request body failed schema validation |
| 429 | Rate limit exceeded |
| 500 | Internal error — the browser operation failed |

Error responses include a JSON body with a `message` field describing the issue.

```json
{
  "message": "Either url or html is required"
}
```


## Migration from Other Providers

The Agenty AI schema is kept intentionally compatible with Browserless, Firecrawl, Cloudflare Browser Rendering, and other browser automation APIs. In most cases, switching providers requires only changing the base URL and API key. All parameter names and response shapes are the same.
