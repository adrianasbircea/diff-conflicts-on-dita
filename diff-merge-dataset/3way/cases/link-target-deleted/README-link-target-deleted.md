# Link target deleted by the other user (CF-19, 3-way)

**Base:** `link-target-deleted-base.dita` -> **User A:** `link-target-deleted-user-a.dita`, **User B:** `link-target-deleted-user-b.dita`

A and B are derived directly and independently from the base. Neither file has review markup.

This is the case where a merge is **correct and still wrong**. A and B never touch the same element,
so a structural merge reports no conflict at all - and produces a document with three unresolvable
links. Referential integrity is not something a node-by-node diff can see.

## User A - works on the links, deletes nothing

| ID | Location | Change |
| --- | --- | --- |
| A-X01 | `p-see-limits` | link text *Limits* -> *Service limits*; `@href` unchanged, still points at `sec-limits` |
| A-X02 | `p-see-new` | a new paragraph with a **new** `<xref>` to `p-limits`, a paragraph inside `sec-limits` |

## User B - deletes the targets, touches no link

| ID | Location | Change |
| --- | --- | --- |
| B-X01 | `sec-limits` | the whole section deleted, together with `title-limits` and `p-limits` |
| B-X02 | `li-prereq-2` | the list item deleted |

## Expected

| ID | Case | Structural result | What must also happen |
| --- | --- | --- | --- |
| CF-19a | `p-see-limits` -> deleted `sec-limits` | merges with no conflict: A edited the link text, B deleted the section | the merge must **report a broken reference**. Silently producing a dead `@href` is the failure this case exists for |
| CF-19b | `p-see-new` -> deleted `p-limits` | A's whole paragraph is an insertion, so it merges cleanly | the new link is dead the moment it is created; the report must name it too, not only the pre-existing links |
| CF-19c | `p-see-item` -> deleted `li-prereq-2` | merges with no conflict | broken reference to a **list item**, not a section: the check must cover any `@id`, not just block-level anchors |
| CF-19d | `p-see-stable` -> `sec-endpoints` | untouched by both | must **not** be flagged. A checker that warns about every link is as useless as one that warns about none |

## Global expectations

* Zero structural conflicts. That is the correct structural answer, and it must not be presented as
  "merged cleanly" without qualification.
* Exactly three broken references after the merge: CF-19a, CF-19b, CF-19c.
* If the tool offers to fix them, the only safe automatic options are removing the `<xref>` wrapper
  and keeping its text, or keeping the link and re-inserting B's deleted target. Rewriting the
  `@href` to some other id must never be automatic.
* Resolving CF-19a in favour of A (keep `sec-limits`) must make CF-19a and CF-19b both resolvable
  again, without touching `p-see-item`.

## Related cases

* Renaming, rather than deleting, a link target is CF-18.
* A `conref` or `conkeyref` whose target is deleted behaves the same way, with a worse symptom: the
  content simply does not render. Worth adding as a variant of this case once conrefs exist in the
  dataset.

## Opening in Web Author

See the end of `../../../2way/README-2way.md`.
