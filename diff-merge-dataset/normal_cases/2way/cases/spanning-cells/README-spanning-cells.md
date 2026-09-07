# Spanning table cells (TB-07, 2-way)

**Base:** `spanning-cells-base.dita` -> **Modified:** `spanning-cells-modified.dita`

Spans are missing from the main 2-way set: no `morerows`, no `namest` / `nameend` anywhere. This
change alters the *grid*, so the number of `<entry>` elements in the row no longer matches the
number of columns - the case a naive positional cell matcher gets wrong.

Every entry carries an explicit `@colname` so the intended column is unambiguous. Both tables have
`cols="3"` in base and in modified: the column count never changes here (that is CF-20).

No review markup in either file.

## The change

| Location | Change |
| --- | --- |
| `row-merge` | two cells merged into one: `e-merge-name` gains `namest="c1" nameend="c2"`, `e-merge-desc` is gone, the two texts are combined |

## Expected

* One span added plus one deleted cell - not "row rewritten" and not "table rewritten".
* The `Active` cell in column `c3` of the same row must not be reported as changed.
* Every other row of both tables is untouched and must be reported nowhere.
* After accepting or rejecting the change, every row must still add up to exactly 3 columns once
  spans are expanded. A row with too many or too few entries is a failure even if the diff itself
  looked right.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`, replacing the last path segment with
`normal_cases%2F2way%2Fcases%2Fspanning-cells%2Fspanning-cells-modified.dita`.
