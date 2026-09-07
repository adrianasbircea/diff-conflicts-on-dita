# Delete against delete (ID-04, 3-way)

**Base:** `delete-vs-delete-base.dita` -> **User A:** `delete-vs-delete-user-a.dita`, **User B:** `delete-vs-delete-user-b.dita`

A and B are derived directly and independently from the base. Neither file has review markup.

Both users removed exactly the same five things. A convergent deletion is **not** a conflict: the
content is gone in both branches, so the merge has nothing to choose between. A tool that reports
delete/delete as a conflict makes every real cleanup commit unmergeable.

Each user also has one small edit of their own, so the two files are not identical and the merge is
a real merge rather than a no-op.

## Deleted in A and in B, identically - must merge with no conflict

| ID | Location | Deletion |
| --- | --- | --- |
| ID-04a | `note-config` | the whole `<note>` element |
| ID-04b | `li-prereq-3` | one list item, from the middle of `ul-prereq` |
| ID-04c | `p-arch` | the `@outputclass="architecture"` attribute |
| ID-04d | `row-lim-burst` | one table row, from the middle of the `tbody` |
| ID-04e | `sec-legacy` | a whole `<section>` subtree, including `p-legacy`, its `@rev`, its `<apiname>` child and `p-legacy-2` |

## Independent edits - must both survive

| ID | Location | Change |
| --- | --- | --- |
| A-D01 | `p-intro` | *validates* -> *checks* (A only) |
| B-D01 | `li-prereq-1` | *A JDK 21 runtime* -> *A JDK 21 runtime, or later* (B only) |

## Expected result

* Zero conflicts.
* `note-config`, `li-prereq-3`, `row-lim-burst` and `sec-legacy` are absent, each removed once.
* `p-arch` has no `@outputclass`, and is not otherwise reported as changed.
* `p-intro` reads *checks*; `li-prereq-1` reads *or later*.
* `ul-prereq` has 3 items, in the order 1, 2, 4. `table-limits` has 2 body rows, TTL then rate.

## Trap to watch for

A tool that matches nodes by position rather than by identity will pair A's `li-prereq-4` with B's
`li-prereq-3` slot and report a spurious modification. Same for `row-lim-rate`, which moves up one
position in both files for the same reason.
