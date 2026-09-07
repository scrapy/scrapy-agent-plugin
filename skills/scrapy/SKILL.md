---
name: scrapy
description: Use when working on a Scrapy project — writing or debugging spiders, running or inspecting crawls.
---

# Scrapy

## LLM-friendly documentation

The official documentation for Scrapy and many related libraries is LLM-friendly: `llms.txt` provides a table of
contents, and all pages are available in Markdown; just replace `.html` with `.md` in the URL. Fetch this documentation
when needed.

For example:

```
https://docs.scrapy.org/llms.txt
https://web-poet.readthedocs.io/en/stable/page-objects/fields.md
```

## Using the Scrapy MCP server

This plugin configures the Scrapy MCP server (https://github.com/scrapy/scrapy-mcp-official), which connects to running
Scrapy jobs via the RemoteControl extension. Only locally running jobs that use Scrapy 2.19.0+ are supported and
returned by the `list_jobs` tool.

Some tasks where this MCP server can be useful:

- listing running crawl processes
- investigating why a crawl is slow, doesn't make requests, or makes requests without producing items
- checking the progress and statistics of a crawl
- modifying the state or even the code of a crawl to fix runtime issues or change its configuration without restarting
  it

## Differences in API and behavior between Scrapy versions

Code written for one Scrapy version may be broken when running with a different one because the Scrapy API and behavior
evolves over time, and your training data may be outdated compared to the Scrapy version used in the project. Always
check which Scrapy version is used by a project or a running spider and always consult the Scrapy documentation for the
specific version when writing code or troubleshooting problems with existing code.

Here are the most important changes between recent Scrapy versions, covering 2.8.0–2.18.0.

- **Start requests:** since 2.12 `start_requests()` may yield items; since 2.13 spiders should define
  `async def start()` instead of `start_requests()`; since 2.16 `start_requests()` is never called, so a spider that
  defines only it produces no requests and no warning.
- **Component instantiation:** since 2.14 Scrapy never calls `from_settings()` methods of components (use
  `from_crawler()`) and `scrapy.utils.misc.create_instance()` is removed in favor of `build_from_crawler()`.
- **Component search methods:** 2.12 adds the `get_addon()`, `get_downloader_middleware()`, `get_extension()`,
  `get_item_pipeline()` and `get_spider_middleware()` methods to `Crawler`.
- **Crawler attribute initialization:** since 2.18 `crawler.engine`, `extensions`, `logformatter`,
  `request_fingerprinter` and `stats` raise `RuntimeError` when read before the crawl starts instead of being `None`.
- **Add-ons:** since 2.10 third-party components can be configured through the `ADDONS` setting.
- **Spider middleware async output:** since 2.16 `process_spider_output()` must be an async generator or be paired with
  a `process_spider_output_async()` method; `process_start_requests()` is replaced by the async `process_start()` in
  2.13 and removed in 2.16.
- **Reactor defaults:** since 2.13 the default `TWISTED_REACTOR` is the asyncio one.
- **Process and runner classes:** 2.14 adds `AsyncCrawlerProcess` and `AsyncCrawlerRunner`, coroutine-based counterparts
  of `CrawlerProcess` and `CrawlerRunner`.
- **httpx-based download handler:** 2.15 adds `HttpxDownloadHandler`; it gains proxy support in 2.16, HTTP/2 support and
  SOCKS proxies in 2.17.
- **Compression:** since 2.18 Brotli and Zstandard support is always available (`brotli` and, on Python 3.13 and lower,
  `backports.zstd` are required dependencies), so `Accept-Encoding` always advertises `br` and `zstd` and such responses
  are always decoded; on older Scrapy versions the `brotli` and `zstandard` packages must be installed explicitly for
  this.
- **scrapy.utils.url re-exports:** the functions re-exported from w3lib, including `canonicalize_url`,
  `safe_url_string`, `add_or_replace_parameter`, `url_query_parameter`, `url_query_cleaner`, `parse_url`, `is_url`,
  `any_to_uri`, `file_uri_to_path`, `path_to_file_uri`, `parse_data_uri` and `safe_download_url`, are removed in 2.16;
  always import them from `w3lib.url`.

## Additional useful libraries

There are many third-party libraries that may be helpful when writing spiders; here are some examples:

- **Better code structure:** `web-poet` and `scrapy-poet` (separation of crawling and extraction code),
  `scrapy-spider-metadata` (structured spider arguments)
- **Better data extraction:** `extruct` (getting structured data embedded in HTML), `dateparser` (parsing date strings),
  `price-parser` (parsing price strings), `number-parser` (parsing number strings), `clear-html` (cleaning and
  normalizing HTML), `html-text` (extracting text from HTML), `zyte-parsers` (extracting some data like ratings/review
  counts from HTML)
- **Help with bans:** `scrapy-playwright` (using a local headless browser), `scrapy-zyte-api` (using Zyte API)
- **QA:** `spidermon` (verifying crawl success and data quality)
