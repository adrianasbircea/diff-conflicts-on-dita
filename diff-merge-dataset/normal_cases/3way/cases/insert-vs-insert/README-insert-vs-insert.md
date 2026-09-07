# Insert against insert, same anchor (CF-12, 3-way)

**Base:** `insert-vs-insert-base.dita` -> **User A:** `insert-vs-insert-user-a.dita`, **User B:** `insert-vs-insert-user-b.dita`

A and B are derived directly and independently from the base. Neither file has review markup.

In the main 3-way set every insertion from A lands in a different part of the document than every
insertion from B, so the add/add case is never exercised. Here both insertions share one anchor.

## The change

| Location | User A inserts | User B inserts |
| --- | --- | --- |
| between `p-intro` and `p-arch` | `p-a-scaling`, on horizontal scaling | `p-b-security`, on TLS |

## Expected

* A conflict, or both paragraphs kept in a stated order. Never one silently dropped, and never the
  two paragraphs spliced into one element.
* If the tool keeps both, the order must be deterministic and the same when the inputs are swapped.
* `p-intro` and `p-arch` are untouched by both users and must not be reported.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`.
