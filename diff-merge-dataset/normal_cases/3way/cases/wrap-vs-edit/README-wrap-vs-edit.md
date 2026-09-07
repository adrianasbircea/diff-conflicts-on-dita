# Wrap and unwrap against editing (CF-13, 3-way)

**Base:** `wrap-vs-edit-base.dita` -> **User A:** `wrap-vs-edit-user-a.dita`, **User B:** `wrap-vs-edit-user-b.dita`

A and B are derived directly and independently from the base. Neither file has review markup.

**User A never changed a character of text.** A only changed the nesting: two paragraphs went into a
new section, a note was unwrapped, and two inline elements were added or removed. **User B never
changed the nesting.** B only edited the text of the same elements A re-nested.

A wrap moves an element to a new parent without changing its content, and it is the case where a
diff most easily reports "old subtree deleted, new subtree inserted" and loses the other side's
edit entirely.

## User A - nesting only

| ID | Location | Change |
| --- | --- | --- |
| A-W01 | `p-body-1`, `p-body-2` | both wrapped into a new, deliberately untitled `<section id="sec-a-arch">`, so that A adds no text of any kind |
| A-W02 | `note-wrap` | note removed, `p-in-note` promoted to a direct child of `sec-config` |
| A-W03 | `p-inline-wrap` | *auth.signing.key* wrapped in `<codeph>` |
| A-W04 | `p-inline-unwrap` | the `<b>` around *authctl* removed |
| A-W05 | `p-inline-clash` | *auth.token.ttl* wrapped in `<codeph>` |

## User B - text only

| ID | Location | Change |
| --- | --- | --- |
| B-W01 | `p-body-1` | *validates* -> *verifies*, *signed access tokens* -> *short-lived signed access tokens* |
| B-W02 | `p-in-note` | *is not supported* -> *is not supported in any release* |
| B-W03 | `p-inline-wrap` | *before the first restart* -> *before every restart* |
| B-W04 | `p-inline-unwrap` | *tool* -> *command-line tool* |
| B-W05 | `p-inline-clash` | *auth.token.ttl* -> *auth.token.ttl.seconds* - **the exact text A wrapped** |

## Expected

| ID | Case | Expected |
| --- | --- | --- |
| CF-13a | block wrap vs edit | clean merge: `p-body-1` sits inside `sec-a-arch` **and** carries B's new wording. Not a delete + insert that drops B-W01 |
| CF-13b | unwrap vs edit | clean merge: `p-in-note` is a direct child of `sec-config` and reads *in any release*. The `<note>` is gone once, not twice, and `p-config-tail` keeps its position |
| CF-13c | inline wrap vs edit outside the wrap | clean merge: `<codeph>auth.signing.key</codeph>` **and** *before every restart* |
| CF-13d | inline unwrap vs edit next to it | clean merge: no `<b>`, text reads *authctl command-line tool* |
| CF-13e | inline wrap vs edit of the wrapped text | **real conflict**: A put a `<codeph>` around a string B replaced. Either result is defensible, but the merge must not produce `<codeph>auth.token.ttl</codeph>.seconds`, nor duplicate the property name, nor leave an empty `<codeph/>` |

## Global expectations

* The number of text differences between base and A is zero; between base and B, five.
* Only CF-13e may be reported as a conflict. Four clean merges and one conflict is the pass
  condition.
* `p-body-2` is untouched by both users. If it shows up as changed, the wrap was diffed as a
  subtree replacement.
* No result may contain an unbalanced or empty inline element.

## Opening in Web Author

See the end of `../../../2way/README-2way.md`.
