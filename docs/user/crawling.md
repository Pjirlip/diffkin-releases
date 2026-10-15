# Crawling

A crawl run visits the routes of one target, captures every route with the target's capture profiles and records the result in the project folder. Runs are listed per target with their status, counters and timing.

## Where routes come from

1. **Base URL**: the target's base URL is always the first route.
2. **Sitemap**: if the target has a sitemap URL configured, it is used. Otherwise Diffkin looks for `sitemap.xml` next to the base URL, at `/sitemap.xml` and in the `Sitemap:` lines of `/robots.txt`. Sitemap indexes and gzip-compressed sitemaps are supported. A sitemap that cannot be loaded is reported as a warning; the crawl continues with link discovery.
3. **Links**: every captured page is scanned for links in its rendered DOM, so routes that only exist after client-side rendering are found too.

Links are followed up to the configured **depth** (default 5; the base URL and sitemap entries are depth 0) and until the **URL limit** is reached (default 500). When the limit cuts discovery short, the run is marked as truncated.

## Scope

A URL is crawled when it

- uses `http` or `https`;
- is on the target's host (or a subdomain when *include subdomains* is enabled) and the same port;
- matches one of the *include patterns* when any are configured, and none of the *exclude patterns* (path globs such as `/docs/*`; `*` matches any characters, `?` exactly one).

A page that redirects outside the scope is recorded as skipped and nothing of it is stored. Redirects inside the scope are followed; the capture is recorded under the requested route together with the final URL.

## Route identity

Routes are identified by a canonical key that does not contain the host, so the same page on staging and production has the same key:

- fragments (`#section`), credentials and default ports are removed;
- `.`/`..` segments, duplicate slashes and `;jsessionid=…` path parameters are normalized;
- a trailing slash is removed by default (`/about/` and `/about` are one route); this is configurable per target (*strip*, *add*, *keep*);
- tracking and session parameters (`utm_*`, `fbclid`, `gclid`, `PHPSESSID`, …) are dropped and the remaining query parameters are sorted. The list of ignored parameters can be replaced per target, and the query string can be dropped entirely.

Two URLs with the same key are captured once per run.

## Limits and traps

Besides depth and the URL limit, Diffkin skips URLs that look generated: more than 10 query parameters, more than 32 path segments, a path segment repeated four times, or more than 2048 characters. Calendar pages, faceted navigation and similar endless link spaces are therefore bounded even when every page links to new ones.

Other bounds per target: concurrent workers (default 2), navigation timeout (30 s), capture timeout (60 s), retries for transient failures (2) and an optional delay between requests.

## Results

Every route/profile ends in one of three states:

| State | Meaning |
| --- | --- |
| **captured** | Screenshot, DOM snapshot and metadata are stored. The HTTP status is recorded; 4xx/5xx pages are captured and counted as HTTP errors. |
| **failed** | Navigation or capture failed after all retries (for example a page that never finishes loading, or a redirect loop). The error is recorded. |
| **skipped** | The route was not captured: redirected out of scope, or the crawl stopped before reaching it. |

A run finishes as **completed**, **completed with errors** (at least one failed route), **cancelled** or **failed** (the browser or the project could not be used). If Diffkin is closed while a crawl runs, the run shows as **interrupted** the next time the project is opened; completed captures of that run are kept.

## Cancelling

Cancelling a run stops new captures immediately. Captures in progress are given a moment to finish or abort, completed captures are kept, and routes still in the queue are recorded as skipped.

## Credentials and network policy

Basic Auth and custom headers configured for a target are sent with every request to the target's own origin and to nothing else. Which destinations a crawl may contact at all is governed by the target's [network policy](network-policy.md).
