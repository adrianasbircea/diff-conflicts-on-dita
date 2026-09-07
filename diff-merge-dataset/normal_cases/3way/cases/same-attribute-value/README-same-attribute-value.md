# Same attribute change on both sides (ID-05, 3-way)

**Base:** `same-attribute-value-base.dita` -> **User A:** `same-attribute-value-user-a.dita`, **User B:** `same-attribute-value-user-b.dita`

A and B are derived directly and independently from the base. Neither file has review markup.

The main 3-way set has convergent changes only for text (ID-01, ID-02) and for a table cell (ID-03).
Attributes are only ever tested as a divergent conflict (CF-04). This case fills the gap: both users
applied the **same value** to the **same attribute**, which is not a conflict.

## The change

| Location | Attribute | Base | User A | User B |
| --- | --- | --- | --- | --- |
| `table-features` | `@frame` | `all` | `topbot` | `topbot` |
| `p-intro` | - | *validates* | *checks* | untouched |

The edit to `p-intro` is scaffolding, not part of the case: without it the two files would be
identical and the merge would be a no-op instead of a real merge.

## Expected

* **Zero conflicts.** `frame="topbot"` is present exactly once; the attribute is not written twice
  on the element, and the element does not end up carrying both the base and the new value.
* `p-intro` reads *checks*.
* Every other element and attribute is identical in all three versions and must be reported
  nowhere.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`.
