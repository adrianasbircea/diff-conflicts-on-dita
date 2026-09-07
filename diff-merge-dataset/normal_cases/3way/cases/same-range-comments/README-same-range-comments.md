# Comment against comment, identical range (CF-22, 3-way)

**Base:** `same-range-comments-base.dita` -> **User A:** `same-range-comments-user-a.dita`, **User B:** `same-range-comments-user-b.dita`

A and B are derived directly and independently from the base. The base has no review markup.

**Neither user changed one character of the document.** Both only added comments, and every comment
of A covers exactly the same range as a comment of B. Two people commenting the same sentence is the
normal outcome of sending a document to two reviewers, and it must not be a conflict: both remarks
are wanted.

What this case actually tests is PI nesting. Two `oxy_comment_start` PIs now have to open at the
same position and close at the same position, and the merge has to pick an order that stays
well-formed.

## Cases

| ID | Anchor | User A | User B | Expected |
| --- | --- | --- | --- | --- |
| CF-22a | `p-config-lead`, whole element | `cmt-a-config` | `cmt-b-config` | both threads present, both anchored to the whole paragraph. The two `oxy_comment_start` PIs must both sit before `<p`, and their `oxy_comment_end` PIs after `</p>`, properly nested - not interleaved |
| CF-22b | the word *startup* in `p-startup` | `cmt-a-frag` | `cmt-b-frag` | both threads on the same one-word text range. The word must appear once, and neither PI pair may end up wrapping zero characters |
| CF-22c | `li-step-2`, whole element | `cmt-a-step` | `cmt-b-step`, marked `flag="done"` | both present; A's stays open, B's stays resolved. A merge that normalises `flag` across the two threads is wrong |
| CF-22d | `e-sec-status`, whole cell | `cmt-a-cell` | `cmt-b-cell` **with a reply** (`mid="1"`) | three entries in the Review Panel for one cell: A's thread, B's thread, and B's reply nested under `cmt-b-cell`. The reply must not re-parent to A's thread |
| CF-22e | `note-warn` | `cmt-a-warn` | nothing | control: a one-sided comment merges with no conflict and no duplication |

## Global expectations

* Zero conflicts, zero text differences. Base, A and B have identical character content; if the
  merge reports any text change, the comment PIs are being treated as content.
* Nine review entries after the merge: 4 from A, 4 from B, 1 reply.
* Every `oxy_comment_start` has exactly one matching `oxy_comment_end`, `@mid` values match their
  `@parentID` thread, and no PI pair crosses another pair's boundary.
* Comment `@id` values are distinct here by construction. The case where they collide is CF-24.
* Open the merged file in the Review Panel and confirm the highlight of each thread covers the
  intended range: the whole paragraph for CF-22a, one word for CF-22b, one cell for CF-22d.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`.
