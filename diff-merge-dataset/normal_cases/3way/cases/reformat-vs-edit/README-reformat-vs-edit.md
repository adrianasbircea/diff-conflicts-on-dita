# Reformatting against editing (CF-11, 3-way)

**Base:** `reformat-vs-edit-base.dita` -> **User A:** `reformat-vs-edit-user-a.dita`, **User B:** `reformat-vs-edit-user-b.dita`

A and B are derived directly and independently from the base. Neither file has review markup.

**User A changed no content at all.** A only ran a different pretty-printer: 4-space indentation,
a narrower wrap column, and a different attribute order. **User B changed only content** and kept
the base formatting.

This is the single most common false conflict in a real repository, and the reason a line-based
merge is unusable for XML.

## Only in User A - formatting, must merge as a no-op

| ID | Location | Change |
| --- | --- | --- |
| A-F01 | whole file | indentation 2 -> 4 spaces at every level |
| A-F02 | whole file | text re-wrapped at a narrower column |
| A-F03 | `topic` | attribute order `id, xml:lang` -> `xml:lang, id` |
| A-F04 | `p-intro`, `p-arch` | attribute order `id, outputclass` -> `outputclass, id` |
| A-F05 | `li-prereq-2` | text wrapped onto a second line |

## Only in User B - content, must all survive

| ID | Location | Change |
| --- | --- | --- |
| B-C01 | `p-intro` | *validates* -> *verifies* |
| B-C02 | `p-arch-note` | new `<p>` added after `p-arch` |
| B-C03 | `li-prereq-2` | *revision 42* -> *revision 47* |

## Expected

| Case | Expected |
| --- | --- |
| CF-11a | **No conflict anywhere.** A contributes nothing, so the merge is B's content. |
| CF-11b | `p-intro`: B's *verifies* wins; A's re-wrap of the same paragraph must not be treated as a competing edit. |
| CF-11c | `li-prereq-2`: B's *47* wins, even though A re-wrapped exactly that item (A-F05). |
| CF-11d | `p-arch-note` is inserted, at the right place, although A re-indented its two neighbours. |
| CF-11e | Attribute order differences produce no entry at all - not even a "changed attributes" marker. |

## Global expectations

* The number of reported differences between base and A is **zero**.
* A merge that reports a conflict here fails the case, whichever side it picks.
* The merged file may use either indentation style; the test is on content, not on layout.
* Reversing the roles (A edits, B reformats) must give the same result.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`.
