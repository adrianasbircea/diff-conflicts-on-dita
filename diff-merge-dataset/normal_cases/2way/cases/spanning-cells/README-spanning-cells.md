# Spanning table cells (TB-07, 2-way)

**Base:** `spanning-cells-base.dita` -> **Modified:** `spanning-cells-modified.dita`

Spans are missing from the main 2-way set: no `morerows`, no `namest` / `nameend` anywhere. Every
change below alters the *grid*, so the number of `<entry>` elements in a row no longer matches the
number of columns - the case a naive positional cell matcher gets wrong.

Every entry carries an explicit `@colname` so the intended column is unambiguous. Both tables have
`cols="3"` in base and in modified: the column count never changes here (that is CF-20).

No review markup in either file.

## Horizontal spans - `table-span-h`

| ID | Location | Change | Expected |
| --- | --- | --- | --- |
| TB-07a | `row-merge` | two cells merged into one: `e-merge-name` gains `namest="c1" nameend="c2"`, `e-merge-desc` is gone, text combined | span added + one deleted cell; the `Active` cell in `c3` must not be reported as changed |
| TB-07b | `row-widen` | `e-widen-span` `nameend` `c2` -> `c3`; `e-widen-status` deleted | span widened + one deleted cell; not a rewritten row |
| TB-07c | `e-keep-span` | text changed, span untouched | content change in a spanned cell only; no attribute change |
| TB-07d | `row-split-h` | span removed: `e-split-span` replaced by `e-split-name` + `e-split-desc`, `@namest`/`@nameend` gone | cell split; the `Beta` cell in `c3` unchanged |

## Vertical spans - `table-span-v`

| ID | Location | Change | Expected |
| --- | --- | --- | --- |
| TB-07e | `row-v1` / `row-v2` | `@morerows` removed from `e-v-session`; `row-v2` gains `e-v2-group` | span removed + one inserted cell; `row-v2`'s other two cells unchanged |
| TB-07f | `row-v3` / `row-v4` | `@morerows="1"` added to `e-v3-group`; `e-v4-group` deleted | span added + one deleted cell; `row-v4` must not be reported as a new row |
| TB-07g | `row-v5` / `row-v7` | `@morerows` `1` -> `2`; `e-v7-group` deleted | span extent change on one attribute + one deleted cell |

## Global expectations

* Cells are matched by their position in the *rendered grid*, not by their index among siblings: in
  `row-v2` of the base, `e-v2-method` sits in column `m2`, not `m1`.
* A span change must be reported as an attribute change plus the cell insertion or deletion it
  implies - never as "row rewritten" or "table rewritten".
* After any accept or reject, every row must still add up to exactly 3 columns; a result where a row
  has too many or too few entries is a failure even if the diff itself looked right.
* Accepting TB-07a and rejecting TB-07d in the same session must leave both tables valid.

## Opening in Web Author

See the end of `../../README-2way.md`, replacing the last path segment with
`2way%2Fcases%2Fspanning-cells%2Fspanning-cells-modified.dita`.
