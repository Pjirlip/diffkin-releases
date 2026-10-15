# DOM diff

Besides pixels, Diffkin compares the rendered structure of a page: which elements exist, what they say and which attributes they carry. The result is a list of meaningful changes with a path for each, not a line-by-line diff of HTML source.

## What is compared

Every capture stores a snapshot of the rendered DOM. Before two snapshots are compared, both are normalized the same way:

- `script`, `noscript` and `template` elements are left out.
- Attributes that change on every response or build are left out: `nonce`, `data-reactid`, `data-react-checksum`, `data-v-*`, `_ngcontent-*`, `_nghost-*`.
- CSRF and request tokens in `<meta name="csrf-token">`, hidden `authenticity_token`/`_csrf`/`__RequestVerificationToken` inputs and similar fields are left out.
- Generated identifiers such as React's `:r1:`, `radix-…`, `mui-12`, `mat-input-0` or ids containing a UUID are left out, together with references to them in `for`, `aria-labelledby`, `aria-describedby` and the like. Stable ids are kept and used to match elements.
- Whitespace is collapsed. The `class` attribute is compared as a set of class names, `style` as a set of declarations, so `class="b a"` equals `class="a b"`.
- Elements covered by an **ignore rule** with DOM scope are compared as one opaque node; their content never produces a change.

`class`, `style`, `id` (when stable), text and accessibility attributes are always compared.

## Changes

| Kind | Meaning |
| --- | --- |
| Node added / removed | An element or text node exists on one side only. The entry shows the subtree size and a snippet. |
| Node moved | The same subtree, unchanged, appears at a different position or under a different parent. Diffkin reports a move only when the subtree is identical after normalization. |
| Text changed | A text node differs; old and new text are shown. |
| Attribute added / removed / changed | An attribute differs; name, old and new value are shown. Changes to `aria-*`, `role`, `alt`, `title`, `lang`, `tabindex`, `disabled`, `hidden`, `for`, `placeholder` and similar attributes are marked as accessibility changes. |

Each change names the node by a CSS-like path such as `html > body > main#main > section > ul > li:nth-of-type(3)`. Paths use stable ids where available and count only elements that take part in the comparison. Where the capture recorded element geometry, the change also carries the element's position on the screenshot so it can be highlighted.

The summary counts added, removed and moved nodes (including descendants), text, attribute and accessibility changes. Long lists are cut at the configured maximum; the counts stay exact.

## Settings

| Setting | Default | Effect |
| --- | --- | --- |
| Whitespace | collapse | `exact` compares text and attribute values as captured. |
| Ignored attributes | the list above | Names or patterns (`*`, `?`) excluded from the comparison, for example `data-track-*`. |
| Ignored tags | `script`, `noscript`, `template` | Elements excluded with their subtree. |
| Ignore volatile ids | on | Drops generated identifiers and references to them. |
| Ignore tokens | on | Drops CSRF/request tokens. |
| Detect moves | on | Reports identical relocated subtrees as moves instead of removal plus addition. |
| Snippet length | 160 | Maximum length of snippets and values. |
| Maximum changes | 5000 | Maximum number of entries kept per comparison. |

Changing settings changes the result, not the stored snapshots, so a comparison can be rerun with different settings at any time.

## Limits

Snapshots are bounded at capture time (20 000 nodes, 2 000 characters per text or attribute value); a snapshot that hit the limit is flagged in the summary. New ignore rules apply to captures made after the rule was added.
