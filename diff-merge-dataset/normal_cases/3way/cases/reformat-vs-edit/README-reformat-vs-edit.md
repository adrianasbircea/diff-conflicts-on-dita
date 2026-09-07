# Reformatting against editing (CF-11, 3-way)

**Base:** `reformat-vs-edit-base.dita` -> **User A:** `reformat-vs-edit-user-a.dita`, **User B:** `reformat-vs-edit-user-b.dita`

A and B are derived directly and independently from the base. Neither file has review markup.

**User A changed no content at all.** A only ran a different pretty-printer: 4-space indentation, a
narrower wrap column, and a different attribute order. **User B changed only content** and kept the
base formatting.

This is the single most common false conflict in a real repository, and the reason a line-based
merge is unusable for XML.

## The change

| Location | User A | User B |
| --- | --- | --- |
| whole file | reindented, re-wrapped, attribute order changed on every element - no content change | untouched |
| `p-intro` | re-wrapped, `id, outputclass` -> `outputclass, id` | *validates* -> *verifies* |

## Expected

* **No conflict anywhere.** A contributes nothing, so the merge is B's content.
* `p-intro` reads *verifies*, even though A re-wrapped exactly that paragraph. A's re-wrap must not
  be treated as a competing edit.
* Attribute order differences produce no entry at all - not even a "changed attributes" marker.
* The number of reported differences between base and A is **zero**.
* The merged file may use either indentation style; the test is on content, not on layout.
* Reversing the roles (A edits, B reformats) must give the same result.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`.
