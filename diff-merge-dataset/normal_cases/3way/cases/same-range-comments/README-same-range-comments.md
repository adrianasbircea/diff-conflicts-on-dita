# Comment against comment, identical range (CF-22, 3-way)

**Base:** `same-range-comments-base.dita` -> **User A:** `same-range-comments-user-a.dita`, **User B:** `same-range-comments-user-b.dita`

A and B are derived directly and independently from the base. The base has no review markup.

**Neither user changed one character of the document.** Both only added a comment, and both comments
cover exactly the same range. Two people commenting the same paragraph is the normal outcome of
sending a document to two reviewers, and it must not be a conflict: both remarks are wanted.

What this case actually tests is PI nesting. Two `oxy_comment_start` PIs now have to open at the same
position and close at the same position, and the merge has to pick an order that stays well-formed.

## The change

| Location | User A | User B |
| --- | --- | --- |
| `p-config-lead`, whole element | thread `cmt-a-config` | thread `cmt-b-config` |

## Expected

* **Zero conflicts, zero text differences.** Base, A and B have identical character content; if the
  merge reports any text change, the comment PIs are being treated as content.
* Both threads present, both anchored to the whole paragraph. The two `oxy_comment_start` PIs must
  both sit before `<p`, and their `oxy_comment_end` PIs after `</p>`, properly nested - not
  interleaved.
* Two review entries after the merge, neither dropped and neither duplicated.
* Every `oxy_comment_start` has exactly one matching `oxy_comment_end`, and no PI pair crosses
  another pair's boundary.
* Open the merged file in the Review Panel and confirm each thread highlights the whole paragraph.

Comment `@id` values are distinct here by construction. The case where they collide is CF-24; the
case where the ranges only partially overlap is CF-23.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`.
