# Comparisons, baselines and review

A comparison takes two sets of captures and produces one **page diff** per route and capture profile. Three kinds exist:

| Kind | Side A | Side B | Typical use |
| --- | --- | --- | --- |
| Baseline → current | the accepted baselines of a target | a crawl run of that target | "Did anything change since I last accepted the site?" |
| Target → target | a crawl run of one target | a crawl run of another target | Production versus staging |
| Run → run | an earlier crawl run | a later crawl run of the same target | Before/after a deployment |

Routes are matched by their canonical path and capture profile. The host is not part of the path, so `/pricing` on production matches `/pricing` on staging. Only capture profiles that both sides share are compared.

## What a page diff tells you

Every page diff carries several signals rather than one score:

- **Presence**: the route exists on both sides, only on A (*missing on B*) or only on B (*added on B*). A route that exists on one side only is reported as such; Diffkin never fakes a screenshot comparison for it.
- **Status**: *compared*, *dimension mismatch* (different screenshot widths, usually different profiles; the DOM is still compared), *missing*, *no data* (a capture failed) or *failed* (the diff itself failed, with the error).
- **Visual evidence**: changed pixels and percentage, number of regions, the largest region, pixels filtered as noise, SSIM, plus the mask and the diff image. See [Visual diff](visual-diff.md).
- **DOM evidence**: nodes added, removed and moved, text and attribute changes, accessibility changes. See [DOM diff](dom-diff.md).
- **HTTP status** of both captures.
- **Capture configuration mismatch**: the two captures were made with different capture settings (profile, rules or browser runtime). Differences between such captures may stem from the configuration rather than the site.
- **Severity** with the findings that produced it.
- **Review decision**.

The comparison summary counts pages by severity, presence, errors and review state, so an overview can show "3 major, 12 changed, 2 missing, 40 unreviewed" at a glance.

## Severity

Severity is derived from the evidence; it is a classification for sorting and filtering, not the truth itself. Each page lists the findings behind its severity, for example `16.7 % of pixels changed in 1 region(s)` or `HTTP status 200 → 500`.

| Finding | Severity |
| --- | --- |
| Route missing on B | major |
| Route added on B | changed |
| Capture failed / no evidence | major |
| HTTP status changed to or from an error | major (otherwise changed) |
| Screenshot widths differ | major |
| Changed pixels ≥ 10 % of the page | major |
| Largest changed region ≥ 25 % of the page | major |
| Changed pixels ≥ 1 % | changed |
| Changed pixels below 1 % | minor |
| Only isolated pixels (noise) | minor |
| DOM nodes added, removed or moved | changed (≥ 50 nodes: major) |
| Accessibility attribute changed | changed |
| Other text or attribute changes | minor |

The thresholds (percentages, region share, node count) are project settings. Changing them and running **reclassify** re-derives the severities of an existing comparison from its stored evidence; nothing is captured or diffed again and review decisions stay.

## Reviewing

Each page diff has one review decision: **unreviewed**, **accepted**, **rejected** or **ignored**. *Ignored* is a deliberate decision and is shown as such; it is never the same as *unchanged*. A decision can be reset to unreviewed.

Accepting a page can also promote its current capture (side B) to the **baseline** of that route and profile:

- *Accept page*: the current capture becomes the baseline; a previous baseline is kept in the history as replaced.
- *Accept selected*: the same for several pages.
- *Accept entire crawl*: every page of the comparison that has a current capture and is not yet accepted. The result lists how many captures were promoted, how many baselines were retired because their route disappeared, and how many pages had nothing to promote (failed captures).

In a baseline → current comparison, accepting a route that is *missing on B* retires its baseline. Baselines can also be set directly from any captured page and removed; the history of every route shows which captures were in force when.

## Recomputing

A comparison records the versions of all algorithms and the settings it used. After an upgrade, **recompute** starts a new comparison of the same sources with the current algorithms; the old comparison and its decisions stay untouched. A comparison that was still running when Diffkin was closed is shown as *interrupted*; the pages it finished are kept.
