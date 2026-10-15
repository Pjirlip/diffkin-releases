# Targets

A target is one website or environment, for example your production site and its staging counterpart. Comparisons run between a target's crawls and its baselines, or between two targets.

## Adding a target

**Targets → Add target…** opens a six-step wizard. The target is created after the first step; every later step saves when you continue, so you can stop and resume.

1. **Name and URL.** The base URL is where crawls start and defines the scope (host, optionally subdomains, port). Choose *Public website* unless the site runs on your machine or in a private network; then choose *Local development*, which lifts the private-network protection for this target only. See [Network Policy](network-policy.md).
2. **Authentication.** HTTP Basic Auth and custom request headers. Passwords and secret header values go to your operating system's secure store; the project only keeps references and asks for the values again after an import on another machine. See [Authentication](authentication.md).
3. **Crawl settings.** Sitemap URL, depth, URL limit, parallel captures, delay, timeouts, retries, include/exclude path patterns, subdomains and route identity (trailing slash, ignored query parameters). See [Crawling](crawling.md).
4. **Capture profiles.** Which profiles to capture with. Free uses one built-in desktop profile; several profiles per crawl and custom profiles are Diffkin Plus features. See [Capture Profiles](capture-profiles.md).
5. **Ignore and mask rules.** CSS selectors to exclude from the comparison (*ignore*) or to paint over in the stored screenshot (*mask*), for the screenshot, the DOM or both, for this target or all targets, optionally limited to a route pattern. See [Capture Profiles → Ignore and mask rules](capture-profiles.md#ignore-and-mask-rules).
6. **Test connection.** Checks that credentials are present, looks for a sitemap, navigates to the base URL under the network policy and shows a preview capture with timings, redirects, blocked requests and warnings. Fix what the test reports before the first crawl.

## Editing and removing

**Edit** on a target reopens the wizard at any step. Changing the base URL or route identity affects how future crawls match routes to existing baselines: a route that no longer matches is reported as missing on one side. Removing a target deletes its credentials from the secure store; its crawl runs, captures, comparisons and baselines stay in the project database until the project data is deleted.

## Production vs. staging

Create one target per environment with the same route identity settings, then compare **Target → target** under *Compare*. Routes are matched by their canonical path, so `https://www.example.com/pricing` and `https://staging.example.com/pricing` are the same route. Credentials and network policy are per target, so a protected staging site and a public production site live side by side.

## Checklist for stable captures

- Mask or ignore consent banners, chat widgets, ads and rotating content.
- Use a capture profile with frozen time when pages show dates or countdowns.
- Keep the same capture profile and browser runtime on both sides; a capture-configuration mismatch is flagged on every page diff.
- Prefer a sitemap over link discovery for large sites; it makes the route set explicit and crawls faster.
