# Attribute deleted against attribute modified (CF-17, 3-way)

**Base:** `attribute-delete-vs-edit-base.dita` -> **User A:** `attribute-delete-vs-edit-user-a.dita`, **User B:** `attribute-delete-vs-edit-user-b.dita`

A and B are derived directly and independently from the base. Neither file has review markup.

CF-04 in the main set is modify-vs-modify on one attribute. This case is the other half: for every
attribute below, one user **removed** it and the other **changed its value**. The direction
alternates, so the tool cannot pass by always preferring one side.

Element content is byte-identical in all three files. Every reported difference must be an attribute
difference.

## Conflicts

| ID | Location | Attribute | Base | User A | User B | Expected |
| --- | --- | --- | --- | --- | --- | --- |
| CF-17a | `p-arch` | `@outputclass` | `architecture` | removed | `architecture-overview` | conflict on the attribute; the paragraph text merges silently |
| CF-17b | `p-legacy` | `@rev` | `1.0` | `2.0` | removed | conflict, opposite direction from CF-17a |
| CF-17c | `note-warn` | `@type` | `warning` | removed | `danger` | conflict; removing `@type` falls back to the DTD default `note`, so the three outcomes render differently and the resolution is not cosmetic |
| CF-17d | `table-limits` | `@frame` | `all` | `topbot` | removed | conflict on the table, not on its rows |
| CF-17e | `table-limits` | `@colsep` | `0` | unchanged | `1` | **not a conflict**: only B touched it. Must merge to `1` even though `@frame` on the same element does conflict |
| CF-17f | `e-lim-ttl-def` | `@align` | `left` | removed | `center` | conflict on one cell; the cell text `3600` and the sibling cell merge untouched |
| CF-17g | `x-limits` | `@scope` | `local` | removed | `external` | conflict on an attribute that changes how the link resolves |

## Global expectations

* Six conflicts (CF-17a..d, f, g), each naming a single attribute on a single element.
* CF-17e is the control: an element may carry both a conflicting and a cleanly merging attribute.
  A tool that conflicts on the whole element fails this case.
* No conflict may be reported on text, on `@id`, or on `@colname`.
* After resolving all conflicts in favour of A, the result must equal `attribute-delete-vs-edit-user-a.dita` except for
  `@colsep`, which is B's only change and must stay `1`.

## Opening in Web Author

See the end of `../../../2way/README-2way.md`.
