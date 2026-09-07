# Wrap against editing (CF-13, 3-way)

**Base:** `wrap-vs-edit-base.dita` -> **User A:** `wrap-vs-edit-user-a.dita`, **User B:** `wrap-vs-edit-user-b.dita`

A and B are derived directly and independently from the base. Neither file has review markup.

**User A never changed a character of text.** A only changed the nesting: two paragraphs went into a
new section. **User B never changed the nesting.** B only edited the text of one of those paragraphs.

A wrap moves an element to a new parent without changing its content, and it is the case where a
diff most easily reports "old subtree deleted, new subtree inserted" and loses the other side's edit
entirely.

## The change

| Location | User A | User B |
| --- | --- | --- |
| `p-body-1`, `p-body-2` | both wrapped into a new, deliberately untitled `<section id="sec-a-arch">`, so that A adds no text of any kind | untouched |
| `p-body-1` | untouched | *validates* -> *verifies*, *signed access tokens* -> *short-lived signed access tokens* |

## Expected

* **Clean merge, no conflict**: `p-body-1` sits inside `sec-a-arch` **and** carries B's new wording.
  Not a delete plus an insert that drops B's edit.
* The number of text differences between base and A is zero.
* `p-body-2` is untouched by both users. If it shows up as changed, the wrap was diffed as a subtree
  replacement.
* The paragraphs outside the wrap, and the `<note>` in `sec-config`, are identical everywhere and
  must be reported nowhere.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`.
