# Delete against delete (ID-04, 3-way)

**Base:** `delete-vs-delete-base.dita` -> **User A:** `delete-vs-delete-user-a.dita`, **User B:** `delete-vs-delete-user-b.dita`

A and B are derived directly and independently from the base. Neither file has review markup.

Both users removed exactly the same list item. A convergent deletion is **not** a conflict: the
content is gone in both branches, so the merge has nothing to choose between. A tool that reports
delete/delete as a conflict makes every real cleanup commit unmergeable.

## The change

| Location | User A | User B |
| --- | --- | --- |
| `li-prereq-3` | deletes the item | deletes the same item |
| `p-intro` | *validates* -> *checks* | untouched |

The edit to `p-intro` is scaffolding, not part of the case: without it the two files would be
identical and the merge would be a no-op instead of a real merge.

## Expected

* **Zero conflicts.** `li-prereq-3` is absent, removed once, and `ul-prereq` has 3 items in the
  order 1, 2, 4.
* `p-intro` reads *checks*.
* Everything else - the note, the tables, the other sections - is identical in all three versions
  and must be reported nowhere.

## Trap to watch for

A tool that matches nodes by position rather than by identity will pair A's `li-prereq-4` with B's
`li-prereq-3` slot and report a spurious modification.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`.
