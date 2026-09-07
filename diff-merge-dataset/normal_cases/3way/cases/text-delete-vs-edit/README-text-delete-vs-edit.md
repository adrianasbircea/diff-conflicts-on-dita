# Text deleted against text modified (CF-16, 3-way)

**Base:** `text-delete-vs-edit-base.dita` -> **User A:** `text-delete-vs-edit-user-a.dita`, **User B:** `text-delete-vs-edit-user-b.dita`

A and B are derived directly and independently from the base. Neither file has review markup.

CF-03 in the main set covers delete-vs-modify at element level, on a `<note>`. This case covers it
at **text** level: A removes a fragment, B edits words inside that same fragment. There is no
correct automatic answer, so what is tested is the *granularity* and the *honesty* of the report:
the conflict must name the deleted fragment, not the enclosing paragraph.

Every difference in this file set is a conflict by construction, at five different granularities:
a sentence, a phrase, a phrase inside a longer edit, a whole list item, and a table cell. The only
control is `li-keep` and the table header, which neither user touched and which must therefore be
reported nowhere.

## Cases

| ID | Location | User A | User B | Expected |
| --- | --- | --- | --- | --- |
| CF-16a | `p-del-sentence` | deletes the whole second sentence (*The record contains the client IP address.*) | extends that sentence (*... and the user agent*) | **conflict on the second sentence only**. Sentences 1 and 3 are identical in all three files and must not appear in the conflict |
| CF-16b | `p-del-overlap` | deletes the words *the audit log* | changes *audit log* -> *audit archive* | **conflict on the overlapping words**. A result reading *Collect the audit archive before ...* or *Collect before ...* is a resolution; *Collect the before opening a ticket* is a failure |
| CF-16c | `p-del-neighbour` | deletes *the metrics snapshot and* | changes *metrics snapshot* -> *metrics bundle* | **conflict**: B edits the exact phrase A removed, even though the surrounding words differ |
| CF-16d | `li-del` | deletes the whole `<li>` | changes *service user* -> *service account* inside it | **conflict**: element deleted vs text modified. Either the item is dropped or it survives with B's wording, but a surviving item must be complete and valid |
| CF-16e | `e-lim-rate-notes` | empties the cell (`<entry/>`) | extends the cell text (*... and per tenant*) | **conflict on that cell only**. `e-lim-rate-name` merges untouched, and the row must keep 2 entries in every outcome |
| CF-16f | `li-keep`, `row-lim-head` | unchanged | unchanged | reported nowhere |

## Global expectations

* Five conflicts, all narrow. A sixth conflict, or one that names a whole paragraph, list or table,
  is a failure.
* CF-16e must never produce a row with one entry: emptying a cell and deleting a cell are different
  operations, and only the first one happened.
* After resolving every conflict in favour of A, the merged file must equal `text-delete-vs-edit-user-a.dita`
  except for whitespace; resolving every one in favour of B must equal `text-delete-vs-edit-user-b.dita`. This is
  the cheapest way to check the resolutions are wired correctly.

## Opening in Web Author

See the end of `../../../2way/README-2way.md`.
