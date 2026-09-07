# Text deleted against text modified (CF-16, 3-way)

**Base:** `text-delete-vs-edit-base.dita` -> **User A:** `text-delete-vs-edit-user-a.dita`, **User B:** `text-delete-vs-edit-user-b.dita`

A and B are derived directly and independently from the base. Neither file has review markup.

CF-03 in the main set covers delete-vs-modify at element level, on a `<note>`. This case covers it
at **text** level: A removes a fragment, B edits words inside that same fragment. There is no correct
automatic answer, so what is tested is the *granularity* and the *honesty* of the report.

## The change

| Location | Base | User A | User B |
| --- | --- | --- | --- |
| `p-del-overlap` | *Collect the audit log before opening a ticket.* | deletes the words *the audit log* | *audit log* -> *audit archive* |

## Expected

* **One narrow conflict**, naming the overlapping words - not the enclosing paragraph.
* *Collect the audit archive before opening a ticket.* and *Collect before opening a ticket.* are
  both resolutions. *Collect the before opening a ticket* is a failure.
* Every other paragraph, the list and the table are identical in all three versions and must be
  reported nowhere.
* Resolving the conflict in favour of A must give `text-delete-vs-edit-user-a.dita`; in favour of B,
  `text-delete-vs-edit-user-b.dita`.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`.
