# Getting started: the desktop walkthrough

This page walks through the core workflow in the Diffkin desktop application: from an empty project to reviewed changes and accepted baselines, without a terminal.

## 1. Browser runtime

Open **Settings → Browser Runtime** and install the pinned browser build if it is not installed yet. Diffkin captures pages with this separate browser, never with its own window. See [Browser Runtime](browser-runtime.md).

## 2. Project

On **Projects**, create a project (name and a parent folder) or import an existing project folder. **Open** switches to the project workspace with the sections *Overview, Targets, Runs, Compare, Baselines, Settings*.

## 3. Targets

A target is one website or environment. **Targets → Add target…** starts the wizard:

1. **Name and URL** – the base URL where the crawl starts. Choose *Public website* unless the site runs on your machine or in a private network; then choose *Local development*, which lifts the private-network protection for this target only. See [Network Policy](network-policy.md).
2. **Authentication** – HTTP Basic Auth and custom request headers. Passwords and secret header values go to the operating system’s secure store; the project file only keeps references, so a project imported on another machine asks for them again. See [Authentication](authentication.md).
3. **Crawl settings** – sitemap URL, depth, URL limit, parallel captures, delays, timeouts, retries, include/exclude path patterns and route identity (trailing slash, query parameters). See [Crawling](crawling.md).
4. **Capture profiles** – which profiles to capture with. See [Capture Profiles](capture-profiles.md).
5. **Ignore and mask** – rules by CSS selector: *ignore* excludes elements from the comparison, *mask* additionally paints them over in the stored screenshot. Rules can apply to the screenshot, the DOM or both, to this target or all targets, and to a route pattern.
6. **Test connection** – checks that credentials are present, looks for a sitemap, navigates to the base URL under the network policy and shows a preview capture with timings, redirects, blocked requests and warnings.

Every step saves when you continue; you can return to any step from the step list or later via **Edit**.

## 4. Crawl

**Crawl now** on the overview, **Crawl** on a target or **Start crawl** under *Runs* starts a crawl as a background job. Progress is shown on the *Runs* and *Jobs* screens, where a running crawl can be cancelled; completed captures are kept. A finished run lists every page with its status, HTTP status, title and problems.

## 5. Compare

Under *Compare*, start a comparison:

- **Baselines → current crawl** – the accepted baselines of a target against a crawl of it.
- **Target → target** – a run of one target against a run of another, for example production against staging.
- **Run → run** – two runs of the same target.

The comparison opens as soon as its first pages are compared. The overview lists every route with presence, severity, changed pixels, number of regions, DOM changes and the review state. Use **Show** to filter (changed only, major only, unreviewed, accepted, rejected, ignored, missing routes, errors) and click a column header to sort. See [Comparisons, baselines and review](comparisons.md).

## 6. Review

Click a route to open the detail viewer:

- **Side by side** – both screenshots with synchronized scrolling.
- **Overlay** – reveal B over A with a slider, or blend both with the opacity mode.
- **Diff image** – changed pixels in red with region boxes; jump between regions with *Previous/Next region*.
- **DOM diff** – the list of structural changes with search and kind filter; selecting a change highlights its element on the screenshot when geometry was captured.

The side panel explains the severity with its findings and shows the capture environments of both sides. Routes that exist on one side only are shown as such, never as a fake comparison.

Decide per page with **Accept as baseline**, **Reject**, **Ignore** or **Reset**. On the overview, select several pages and use the same actions, or use **Accept entire crawl…**, which shows what will be promoted, retired and skipped before it changes anything.

## 7. Baselines and settings

*Baselines* lists the accepted captures per target with their history and lets you retire one. *Settings* holds the project name and the comparison thresholds (visual diff, DOM diff, severity); **Reclassify** on a comparison applies new thresholds to its stored evidence without capturing or diffing again.

## 8. Jobs

The global *Jobs* screen shows every crawl and comparison of this session with progress, duration and errors, and lets you cancel running jobs. The badge in the sidebar counts running jobs.

## 9. License

**Settings → License** shows the edition, lets you activate Diffkin Plus with the purchase email and license key, validate it, deactivate this device and see what Plus includes. Controls that need Plus, such as a second capture profile in the target wizard, explain this in place and link to the license screen. See [Licensing](licensing.md).

## 10. Updates

**Settings → Updates** checks the public release channel, shows the release notes of a newer version and installs it in place after your confirmation; a banner announces releases found by the start-up check. **Settings → About** lists the version history and the open source notices. See [Updates](updates.md).
