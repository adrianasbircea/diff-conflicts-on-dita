# Same attribute change on both sides (ID-05, 3-way)

**Base:** `same-attribute-value-base.dita` -> **User A:** `same-attribute-value-user-a.dita`, **User B:** `same-attribute-value-user-b.dita`

A and B are derived directly and independently from the base. Neither file has review markup.

The main 3-way set has convergent changes only for text (ID-01, ID-02) and for a table cell
(ID-03). Attributes are only ever tested as a divergent conflict (CF-04). This case fills the gap:
both users applied the **same value** to the **same attribute**, which is not a conflict.

Each user also has one independent text edit, so the two files are not identical.

## Identical in A and B - must not conflict

| ID | Location | Change in both |
| --- | --- | --- |
| ID-05a | `table-features` | `@frame` `all` -> `topbot` (value change) |
| ID-05b | `shortdesc` | `@outputclass="lead"` added (insertion) |
| ID-05c | `p-legacy` | `@rev="1.0"` removed (deletion) |
| ID-05d | `note-warn` | `@type` `warning` -> `danger` (value change that also changes how the note renders) |
| ID-05e | `row-sec` | `@rowsep="0"` added on a row |
| ID-05f | `e-sec-status` | `@align="center"` added on a cell |

## Independent edits - must both survive

| ID | Location | Change |
| --- | --- | --- |
| A-I01 | `p-intro` | *validates* -> *checks* (A only) |
| B-I01 | `li-prereq-2` | *revision 42* -> *revision 47* (B only) |

## Expected result

* Zero conflicts.
* `frame="topbot"`, `outputclass="lead"`, `type="danger"`, `rowsep="0"`, `align="center"` all
  present exactly once; no `@rev` anywhere.
* `p-intro` reads *checks*, `li-prereq-2` reads *revision 47*.
* No attribute is written twice on the same element, and no element ends up with both the base and
  the new value recorded.

## Trap to watch for

ID-05c is the one most likely to fail: a deletion applied twice can be implemented as "remove the
attribute, then remove it again", which some merge engines report as an error or as a conflict
against a missing node. The correct result is simply an element with no `@rev`.

ID-05b and ID-05f test the same thing for insertions: adding the same attribute with the same value
on both sides is one insertion, not two.

## Opening in Web Author

See the end of `../../../2way/README-2way.md`.
