# 3-Way Isolated Cases

One directory per case, each with its own base / user-a / user-b triplet and its own README. In
every case A and B are derived **directly and independently** from that case's base - B is never
derived from A.

Conventions are the ones from `../README-3way.md` and `../../2way/README-2way.md`: DITA topic with a
DOCTYPE, an `@id` on every element worth referring to, Oxygen review PIs, *Ana Popescu* as user A
and *Dan Ionescu* as user B, and a README whose tables give *Base*, *User A*, *User B* and
*Expected*. Element ids are reused from the master base wherever the case covers the same content.

Case ids continue the master numbering: `CF-` for conflicts (the master set ends at CF-10), `ID-`
for changes that are identical on both sides and must **not** conflict (the master set ends at
ID-03).

## Structure and text

| Case | Directory | A does | B does | Expect |
| --- | --- | --- | --- | --- |
| CF-11 | `reformat-vs-edit/` | reformats the file, no content change | edits content, keeps the formatting | no conflict at all |
| CF-12 | `insert-vs-insert/` | inserts a paragraph, a list item, a table row | inserts different ones at the same anchors | conflicts, plus one `@id` collision and one identical insertion that must not duplicate |
| ID-04 | `delete-vs-delete/` | deletes five things | deletes the same five | no conflict |
| CF-13 | `wrap-vs-edit/` | wraps and unwraps, block and inline | edits the text inside | four clean merges, one conflict |
| CF-14 | `split-vs-edit/` | splits and joins blocks | edits text across the boundaries | B's edits must land in the right block |
| CF-15 | `adjacent-word-edits/` | edits one word per sentence | edits a different word in the same sentence | one conflict only, the control case |
| CF-16 | `text-delete-vs-edit/` | removes fragments | edits words inside them | narrow conflicts, not whole-block ones |

## Attributes, identity and references

| Case | Directory | A does | B does | Expect |
| --- | --- | --- | --- | --- |
| ID-05 | `same-attribute-value/` | six attribute changes | the same six, same values | no conflict |
| CF-17 | `attribute-delete-vs-edit/` | removes attributes | changes their values, and the reverse | one conflict per attribute, one clean merge on the same element |
| CF-18 | `id-renamed-both-sides/` | renames ids and its own `@href` | renames the same ids differently, and edits inside | subtree matching must survive the rename |
| CF-19 | `link-target-deleted/` | works on the links | deletes what they point at | merges with no conflict and three broken links, which must be reported |
| CF-20 | `column-vs-column/` | adds a column | adds a different column, and a row to the other table | the merged table must stay a valid grid |

## Review markup

| Case | Directory | A does | B does | Expect |
| --- | --- | --- | --- | --- |
| CF-21 | `accept-vs-reject/` | accepts every tracked change in the base | rejects every one | five conflicts; **the only case whose base already has review markup** |
| CF-22 | `same-range-comments/` | comments five ranges | comments the same ranges | both threads survive, PIs well nested |
| CF-23 | `crossing-comment-ranges/` | comments a range | comments a partially overlapping range | no crossing PIs may be emitted |
| CF-24 | `comment-id-collision/` | generates `cmt-1`, `cmt-2`, `cmt-3` | generates the same ids for other threads | ids renamed, replies stay on the right thread |
| CF-25 | `tracked-vs-plain-edit/` | edits with track changes on | edits the same text with them off | surviving tracked changes must still reject to the **base** wording |

## What these add over the master set

* The master base has no review markup at all, so accept-vs-reject was untested. CF-21 covers it.
* Every insertion of A in the master set lands away from every insertion of B, so add/add was never
  exercised. CF-12 covers it.
* Convergent changes were tested for text and for a cell, never for attributes or deletions. ID-04
  and ID-05 cover those.
* Conflict granularity was only ever tested with full rewrites on both sides. CF-15 and CF-16 test
  whether the unit is the word or the block.
* Nothing tested what a merge does to `@id`, to link targets, or to the table grid. CF-18, CF-19 and
  CF-20 do.
* Comment identity and comment nesting were assumed correct. CF-22, CF-23 and CF-24 attack them.

## Running them

Each README ends with a pointer to the Web Author URL pattern. For a quick check before opening
anything:

```
xmllint --noout --nonet 3way/cases/*/*.dita
```

A cheap regression check for every case: resolve all conflicts in favour of A and diff the result
against `<case>-user-a.dita`, then do the same for B. Both READMEs that rely on this say so
explicitly.
