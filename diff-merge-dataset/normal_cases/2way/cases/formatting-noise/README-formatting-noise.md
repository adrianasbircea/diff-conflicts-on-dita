# Reformatting noise (N-01, 2-way)

**Base:** `formatting-noise-base.dita` -> **Modified:** `formatting-noise-modified.dita`

The modified file is the same document re-indented and re-wrapped by a different editor profile.
Nothing in the content changed, except the one case explicitly marked *real change* below. This is
the most frequent real-world diff: one author saves with another pretty-printer and the whole file
looks modified.

No review markup in either file.

## Noise - must NOT be reported as a change

| ID | Location | Difference | Expected |
| --- | --- | --- | --- |
| N-01a | whole file | indentation 2 -> 4 spaces, every level | no change |
| N-01b | `shortdesc`, `p-intro`, `p-config-lead` | text re-wrapped at a different column | no change |
| N-01c | `p-arch`, `p-link` | element serialised on a single long line | no change |
| N-01d | `p-intro` | attribute order `id, outputclass` -> `outputclass, id` | no change |
| N-01e | `p-intro` | `outputclass` quoted with `'` instead of `"` | no change |
| N-01f | `p-nbsp` | `&#160;` -> `&#xA0;` (decimal vs hex, same character) | no change |
| N-01g | `p-link` | `&amp;` -> `&#38;` inside `@href` | no change |
| N-01h | `img-topology` | `<image ...></image>` -> `<image .../>` | no change |
| N-01i | `img-topology` | attribute order changed on the same element | no change |
| N-01j | `cb-untouched` | reformatting must not touch a `xml:space="preserve"` block | no change |

## Real change - must be reported

| ID | Location | Difference | Expected |
| --- | --- | --- | --- |
| N-01k | `p-arch` | double space after *local disk.* -> single | reported only when whitespace is not normalised; must never be the only reason the paragraph is flagged as rewritten |

## Global expectations

* With whitespace normalisation on: no change at all.
* With whitespace normalisation off: N-01k only; N-01a..N-01j still produce nothing.
* No case may collapse into "whole document changed".

Significant whitespace inside `xml:space="preserve"` is deliberately **not** tested here: it is
covered by `../../../../edge_cases/D3-codeblock-whitespace`, which pairs with
`../../../../context/C10-global-reformat` on exactly that discrimination. N-01j only checks that the
reformat leaves a preserve block alone.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`, replacing the last path segment with
`2way%2Fcases%2Fformatting-noise%2Fformatting-noise-modified.dita`.
