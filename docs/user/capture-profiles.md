# Capture profiles and stabilization

A capture profile defines the conditions under which Diffkin renders a page. Two captures are only comparable when they were made with the same profile, so every capture records a copy of its profile and a configuration hash.

Every project starts with the built-in profile **Desktop 1440×900** (device pixel ratio 1, light color scheme, locale `en-US`, timezone `UTC`).

## Emulated environment

| Setting | Effect |
| --- | --- |
| Viewport and device pixel ratio | Layout size and pixel density. A DPR of 2 doubles the screenshot resolution. |
| Color scheme | `prefers-color-scheme: light` or `dark`. |
| Locale | `Accept-Language`, `navigator.language` and `Intl` formatting. |
| Timezone | What `Date` and `Intl` report as the local timezone. |
| Fixed time | Freezes the clock a page sees through `Date` (see below). |

## How a page is captured

Diffkin runs the same sequence for every page and records how long each step took:

1. **Navigation**: the page is loaded; redirects and the HTTP status of the document are recorded.
2. **DOM ready**: the document has finished loading.
3. **Network idle**: no request has been in flight for the configured quiet period (default 500 ms).
4. **Fonts ready**: web fonts have finished loading.
5. **Images ready**: lazy images are forced to load, the page is scrolled through once so script-based lazy loading triggers, and all images are decoded.
6. **Stabilization**: animations and transitions are stopped, the text caret is hidden, the page is scrolled to the top.
7. **Wait selector** (optional): an element you specify must be present.
8. **Settle delay**: a final pause (default 250 ms).
9. **Screenshot** and **DOM snapshot**.

Network, font and image readiness are best effort: if one of them does not complete within its timeout, the capture still happens and the timeout is recorded in the capture metadata together with what was still pending (for example "2 request(s) still in flight"). A page that polls a server forever is therefore still captured.

The wait selector is different: if it does not appear in time the capture fails, because you declared the page unusable without it.

All timeouts are configurable per profile and bounded to 120 seconds.

## Motion and caret suppression

By default Diffkin stops CSS animations and transitions before the screenshot. Animations are fast-forwarded to their final state rather than removed, so elements that fade in stay visible. Infinite animations (spinners, pulses) are frozen at their final keyframe. The text caret of focused inputs is hidden. The page additionally sees `prefers-reduced-motion: reduce`.

Turn **Suppress motion** off in a profile if an animation itself is what you want to test.

Animations inside shadow DOM are not covered by the suppression stylesheet.

## Fixed time

When a profile sets a fixed time, pages see that instant whenever they read `new Date()` or `Date.now()`, which makes "rendered at 12:34" timestamps reproducible. Timers keep running so pages still load normally. Two things are not frozen: `performance.now()` and `Intl` date formatters that are called without an explicit date. Combine a fixed time with an ignore or mask rule if a page uses one of those.

## Ignore and mask rules

Rules select elements with a CSS selector and decide how they are treated:

| Action | Stored screenshot | Stored DOM | Comparison |
| --- | --- | --- | --- |
| **Ignore** | unchanged | element kept, content replaced by the rule marker | region/element excluded |
| **Mask** | region painted magenta before saving | element kept, content replaced by the rule marker | region/element excluded |

Use *ignore* for content that is legitimately dynamic (a clock, a rotating teaser) and *mask* for anything that must not be persisted (customer names, avatars, session ids). Masking happens before the screenshot is written to the project folder.

A rule can affect the visual comparison, the DOM comparison or both, and can be limited to a target, a capture profile and/or a route pattern such as `/blog/*` (`*` matches any characters, `?` exactly one).

Selectors are checked by the browser when a capture runs; an invalid selector fails the capture with a message naming the rule.

## Full-page screenshots and limits

Screenshots cover the whole scrollable page at the profile's viewport width. Pages taller than 20 000 CSS pixels are cut off at that height and the capture is marked as clipped. Images beyond 16 384 × 65 536 pixels or 120 million pixels are rejected.

## Capture metadata

Every capture stores, next to its screenshot and DOM snapshot:

- Diffkin and browser versions, operating system and architecture;
- the profile as used and the configuration hash;
- requested and final URL, HTTP status and redirect chain;
- the duration of every step and which steps timed out;
- the rules that applied, with the regions they covered;
- hashes and sizes of the stored objects.

Screenshots are re-encoded by Diffkin so that identical pixels always produce identical files. Identical screenshots are stored only once; the project folder keeps content-addressed objects under `.diffkin/objects/`.
