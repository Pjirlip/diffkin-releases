# Visual diff

Diffkin compares two screenshots of the same route pixel by pixel and reports several signals instead of a single similarity number. Every result records the algorithm versions and the settings that produced it, so a comparison can be reproduced or recomputed later.

## What is compared

- Both screenshots must have the **same width**. Different widths mean different viewports or capture profiles; such a pair is reported as a *dimension mismatch* instead of a misleading score.
- **Heights may differ.** Full-page captures grow or shrink when content changes. The rows that exist in only one screenshot are counted as changed and reported separately as *uncomparable pixels*, so you can tell "the page got longer" apart from "pixels inside the page changed".
- Regions covered by **ignore rules** are excluded entirely; they are reported as *ignored pixels*. Areas covered by **mask rules** are already replaced in the stored screenshots and therefore compare as unchanged.

## Signals

| Signal | Meaning |
| --- | --- |
| Changed pixels, changed percentage | Pixels whose colour distance exceeds the threshold, relative to the compared area. |
| Regions | Clusters of changed pixels with their bounding box, area and the share of changed pixels inside the box. The largest region is listed first. |
| Noise | Small clusters below the minimum region size. They stay in the pixel count and in the mask but get no region box. |
| Anti-aliased pixels | With anti-aliasing detection enabled: pixels that differ only by edge rendering. They are shown in yellow and not counted as changed. |
| SSIM | Structural similarity of the compared area, between 0 (nothing in common) and 1 (identical). It complements the pixel evidence. |
| Dimensions | Width and height of both screenshots and the counts of compared, uncomparable and ignored pixels. |

The difference mask and the diff image are stored alongside the numbers. The diff image shows a faded copy of the first screenshot with changed pixels in red, anti-aliased pixels in yellow, region boxes in blue and ignored regions outlined in gray.

## Settings

| Setting | Default | Effect |
| --- | --- | --- |
| Threshold | 0.1 | Colour distance (0–1) above which a pixel counts as changed. The default ignores very subtle tints, for example a light gray block on white; lower it for stricter comparison, raise it to tolerate more rendering noise. |
| Anti-aliasing detection | off | Reclassifies edge-rendering differences as anti-aliasing. Helpful when text shifts by fractions of a pixel between captures; thin strokes are not always recognised, which is why it is opt-in. |
| Cluster distance | 8 px | Changed pixels closer than this belong to the same region. Larger values give fewer, bigger regions. |
| Minimum region size | 8 px | Clusters with fewer changed pixels are reported as noise. |

Settings are part of every stored result. Changing them changes the numbers, not the stored screenshots, so comparisons can be rerun with different settings at any time.

## Limits

Screenshots up to 16 384 × 65 536 px and 120 million pixels are supported. Larger images are rejected before they are decoded.
