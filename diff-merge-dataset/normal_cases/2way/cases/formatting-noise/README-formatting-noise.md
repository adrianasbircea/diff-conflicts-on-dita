# Reformatting noise (N-01, 2-way)

**Base:** `formatting-noise-base.dita` -> **Modified:** `formatting-noise-modified.dita`

The modified file is the same document saved by a different editor profile. **No content changed at
all** - every difference below is serialisation. This is the most frequent real-world diff: one
author saves with another pretty-printer and the whole file looks modified.

No review markup in either file.

## The change

| Location | Difference |
| --- | --- |
| whole file | indentation 2 -> 4 spaces, text re-wrapped at a different column, some elements on one long line |
| `p-intro` | attribute order `id, outputclass` -> `outputclass, id`, and `outputclass` quoted with `'` instead of `"` |
| `p-nbsp` | `&#160;` -> `&#xA0;` - decimal versus hex, the same character |
| `p-link` | `&amp;` -> `&#38;` inside `@href` |
| `img-topology` | `<image ...></image>` -> `<image .../>`, attribute order changed |
| `cb-untouched` | left alone: reformatting must not reach inside `xml:space="preserve"` |

## Expected

* **Zero differences reported.** Not one entry, not a "changed attributes" marker, and never
  "whole document changed".
* The two files are character-identical once parsed: same elements, same attribute values, same
  text. A differ that reports anything here is comparing serialisation, not the document.

Significant whitespace inside `xml:space="preserve"` is deliberately **not** tested here: it is
covered by `../../../../edge_cases/D3-codeblock-whitespace`, which pairs with
`../../../../context/C10-global-reformat` on exactly that discrimination.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`, replacing the last path segment with
`normal_cases%2F2way%2Fcases%2Fformatting-noise%2Fformatting-noise-modified.dita`.
