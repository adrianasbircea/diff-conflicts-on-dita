# Column added on both sides (CF-20, 3-way)

**Base:** `column-vs-column-base.dita` -> **User A:** `column-vs-column-user-a.dita`, **User B:** `column-vs-column-user-b.dita`

A and B are derived directly and independently from the base. Neither file has review markup.

TB-06 in the main set adds a column in a 2-way diff, where there is nothing to reconcile. Here both
users widen the same table, and a column is not a single node: it is `@cols` on the `<tgroup>`, a new
`<colspec>`, and one new `<entry>` in **every** row, head included. All of those have to be merged
consistently or the table stops being a valid grid.

`table-limits` is the cheaper and more common variant: one user adds a column, the other adds a row,
and the cell where they cross does not exist in either input.

## `table-endpoints` - column against column

| ID | Location | User A | User B | Expected |
| --- | --- | --- | --- | --- |
| CF-20a | `tgroup/@cols` | `3` -> `4` | `3` -> `4` | **same value on both sides**, so `@cols` alone must not conflict. The conflict is about *which* fourth column, not about the count |
| CF-20b | new `<colspec>` | `colname="e4" colwidth="1*"` | `colname="e4" colwidth="1.2*"` | conflict: two different colspecs claim `colnum="4"` and the name `e4`. A merge that keeps both leaves 5 colspecs against `cols="4"` - invalid |
| CF-20c | `row-ep-head` | new entry *Since* | new entry *Authentication* | conflict on the header cell |
| CF-20d | `row-token`, `row-introspect`, `row-revoke` | new entry per row (`4.0`, `4.1`, `4.2`) | new entry per row (`Basic`, `Bearer`, `Bearer`) | conflict per row, or one conflict for the whole column. **Four separate conflicts that can be resolved in different directions is a failure**: taking A's header with B's body cells produces a nonsense table |
| CF-20e | the first three columns | untouched | untouched | must not be reported at all |

If the tool resolves CF-20b..d by keeping **both** columns, then `@cols` must become `5`, a fifth
colspec must be generated, and every row must get 5 entries. That is a legitimate resolution, and
also the one most likely to be implemented half-way.

## `table-limits` - column against row

| ID | Location | User A | User B | Expected |
| --- | --- | --- | --- | --- |
| CF-20f | `tgroup/@cols` | `2` -> `3`, new colspec `l3`, new *Notes* header | unchanged | merges: only A touched it |
| CF-20g | `tbody` | unchanged | new row `row-lim-burst` with 2 entries | merges: only B touched it |
| CF-20h | the crossing cell | - | - | **the payload of the case**: the merged table is `cols="3"`, so `row-lim-burst` needs a third entry that exists in neither input. The tool must synthesise an empty `<entry colname="l3"/>`. A merged row with 2 entries in a 3-column table is invalid, and a row silently dropped is worse |

## Global expectations

* After every conflict is resolved, in either direction, the following must hold for both tables:
  `@cols` equals the number of `<colspec>` elements, and every row accounts for exactly that many
  columns once spans are expanded.
* `@colnum` values must stay consecutive from 1, and `@colname` values unique inside the `tgroup`.
* No `<entry>` may reference a `@colname` that has no `<colspec>`.
* CF-20e is the control: widening a table must not mark its untouched cells as changed.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`.
