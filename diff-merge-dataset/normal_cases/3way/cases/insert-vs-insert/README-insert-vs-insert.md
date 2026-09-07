# Insert against insert, same anchor (CF-12, 3-way)

**Base:** `insert-vs-insert-base.dita` -> **User A:** `insert-vs-insert-user-a.dita`, **User B:** `insert-vs-insert-user-b.dita`

A and B are derived directly and independently from the base. Neither file has review markup.

In the main 3-way set every insertion from A lands in a different part of the document than every
insertion from B, so the add/add case is never exercised. Here every insertion of A shares its
anchor with an insertion of B.

## Conflicts

| ID | Location | User A inserts | User B inserts | Expected |
| --- | --- | --- | --- | --- |
| CF-12a | between `p-intro` and `p-arch` | `p-a-scaling` | `p-b-security` | conflict, or both kept in a stated order; never one silently dropped, and never the two paragraphs spliced into one |
| CF-12b | end of `ul-prereq` | `li-prereq-4`, *A signing key of at least 256 bits is available.* | `li-prereq-4`, *The audit collector is reachable on port 9443.* | **`@id` collision**: two different items claim the same id. The merge must keep both items and rename one id, or report the collision. A result containing two `id="li-prereq-4"` is invalid XML and a hard failure |
| CF-12d | end of the `table-limits` `tbody` | `row-lim-burst` | `row-lim-clock` | conflict or both rows kept; the table must still be `cols="2"` with 2 entries per row |

## Must not produce a conflict

| ID | Location | Change in both | Expected |
| --- | --- | --- | --- |
| CF-12c | after `p-config-lead` | the identical `<note id="note-restart" type="important">`, same text, same attributes | the note appears **once** in the merge: no conflict, no duplicate |

## Global expectations

* CF-12c is the control case: identical insertions at an identical anchor are one insertion.
* CF-12b must be checked in the merged file, not only in the merge preview. Run a validation pass
  and confirm there is no duplicate `@id`.
* If the tool resolves CF-12a by keeping both paragraphs, the order must be deterministic and the
  same when the inputs are swapped.
* No merge result may interleave the text of the two insertions inside one element.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`.
