# 3-Way Isolated Cases

One directory per case, each with its own base / user-a / user-b triplet and its own README. In
every case A and B are derived **directly and independently** from that case's base - B is never
derived from A. **Each triplet contains exactly one change per side**, so a failure points straight
at one behaviour.

Conventions are the ones from `../../../base/3way/README-3way.md` and
`../../../base/2way/README-2way.md`: DITA topic with a DOCTYPE, an `@id` on every element worth
referring to, Oxygen review PIs where a case needs them, *Ana Popescu* as user A and *Dan Ionescu*
as user B, and a README that states the change and what counts as a pass. Element ids are reused
from the master base wherever the case covers the same content.

Case ids continue the master numbering: `CF-` for conflicts (the master set ends at CF-10), `ID-`
for changes that are identical on both sides and must **not** conflict (the master set ends at
ID-03).

Three cases carry one extra edit beyond the case itself, and say so in their README: ID-04 and ID-05
need it because both sides make the same change and the two files would otherwise be identical, and
CF-25 carries `<?oxy_options track_changes="on"?>` on the side that has track changes on.

## Structure and text

| Case | Directory | A does | B does | Expect |
| --- | --- | --- | --- | --- |
| CF-11 | `reformat-vs-edit/` | reformats the file, no content change | edits one word | no conflict at all |
| CF-12 | `insert-vs-insert/` | inserts a paragraph | inserts a different one at the same anchor | conflict, or both kept in a stated order |
| ID-04 | `delete-vs-delete/` | deletes a list item | deletes the same item | no conflict |
| CF-13 | `wrap-vs-edit/` | wraps two paragraphs into a new section | edits the text inside one of them | clean merge, B's edit survives the wrap |
| CF-14 | `split-vs-edit/` | splits a paragraph in two | edits a sentence on each side of the split point | both edits land in the right block |
| CF-15 | `adjacent-word-edits/` | edits word 4 of a sentence | edits word 10 of the same sentence | clean merge - the granularity test |
| CF-16 | `text-delete-vs-edit/` | removes a phrase | edits words inside it | one narrow conflict, not a whole-block one |

## Attributes, identity and references

| Case | Directory | A does | B does | Expect |
| --- | --- | --- | --- | --- |
| ID-05 | `same-attribute-value/` | sets `@frame` to `topbot` | sets the same attribute to the same value | no conflict |
| CF-17 | `attribute-delete-vs-edit/` | removes `@outputclass` | changes its value | one conflict, on that attribute alone |
| CF-18 | `id-renamed-both-sides/` | renames a section id and its `@href` | renames it differently, and edits inside | subtree matching must survive the rename |
| CF-19 | `link-target-deleted/` | edits the link text | deletes what it points at | merges with no conflict and one broken link, which must be reported |
| CF-20 | `column-vs-column/` | adds a column | adds a different column | the merged table must stay a valid grid |

## Review markup

| Case | Directory | A does | B does | Expect |
| --- | --- | --- | --- | --- |
| CF-21 | `accept-vs-reject/` | accepts the tracked change in the base | rejects it | one conflict; **the only case whose base already has review markup** |
| CF-22 | `same-range-comments/` | comments a paragraph | comments the same range | both threads survive, PIs well nested |
| CF-23 | `crossing-comment-ranges/` | comments a range | comments a partially overlapping range | no crossing PIs may be emitted |
| CF-24 | `comment-id-collision/` | generates `cmt-1` with a reply | generates the same id for another thread, with its own reply | id renamed, replies stay on the right thread |
| CF-25 | `tracked-vs-plain-edit/` | edits with track changes on | edits the same text with them off | a surviving tracked change must still reject to the **base** wording |

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
xmllint --noout --nonet normal_cases/3way/cases/*/*.dita
```

A cheap regression check for every case: resolve all conflicts in favour of A and diff the result
against `<case>-user-a.dita`, then do the same for B.
