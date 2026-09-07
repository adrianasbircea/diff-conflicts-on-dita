# Column added on both sides (CF-20, 3-way)

**Base:** `column-vs-column-base.dita` -> **User A:** `column-vs-column-user-a.dita`, **User B:** `column-vs-column-user-b.dita`

A and B are derived directly and independently from the base. Neither file has review markup.

TB-06 in the main set adds a column in a 2-way diff, where there is nothing to reconcile. Here both
users widen the same table. A column is not a single node: it is `@cols` on the `<tgroup>`, a new
`<colspec>`, and one new `<entry>` in **every** row, head included. All of those have to be merged
consistently or the table stops being a valid grid.

## The change

| Location | User A | User B |
| --- | --- | --- |
| `table-endpoints`, `tgroup/@cols` | `3` -> `4` | `3` -> `4` |
| new `<colspec>` | `colname="e4" colwidth="1*"` | `colname="e4" colwidth="1.2*"` |
| `row-ep-head` | new entry *Since* | new entry *Authentication* |
| `row-token`, `row-introspect`, `row-revoke` | `4.0`, `4.1`, `4.2` | `Basic`, `Bearer`, `Bearer` |

## Expected

* `@cols` is `4` on both sides, so **the count alone must not conflict**. The conflict is about
  *which* fourth column.
* The colspecs conflict: two different ones claim `colnum="4"` and the name `e4`. A merge that keeps
  both leaves 5 colspecs against `cols="4"` - invalid.
* The header cell and the three body cells belong to that same conflict. **Four separate conflicts
  that can be resolved in different directions is a failure**: taking A's header with B's body cells
  produces a nonsense table.
* If the tool resolves it by keeping **both** columns, then `@cols` must become `5`, a fifth colspec
  must be generated, and every row must get 5 entries. That is a legitimate resolution, and also the
  one most likely to be implemented half-way.
* The first three columns are untouched and must not be reported at all.
* After the conflict is resolved, in either direction: `@cols` equals the number of `<colspec>`
  elements, every row accounts for exactly that many columns, `@colnum` values stay consecutive from
  1, `@colname` values are unique inside the `tgroup`, and no `<entry>` references a `@colname` that
  has no `<colspec>`.
* `table-limits` is untouched by both users and must be reported nowhere.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`.
