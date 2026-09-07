# Link target deleted by the other user (CF-19, 3-way)

**Base:** `link-target-deleted-base.dita` -> **User A:** `link-target-deleted-user-a.dita`, **User B:** `link-target-deleted-user-b.dita`

A and B are derived directly and independently from the base. Neither file has review markup.

This is the case where a merge is **correct and still wrong**. A and B never touch the same element,
so a structural merge reports no conflict at all - and produces a document with an unresolvable
link. Referential integrity is not something a node-by-node diff can see.

## The change

| Location | User A | User B |
| --- | --- | --- |
| `p-see-limits` | link text *Limits* -> *Service limits*; `@href` unchanged, still points at `sec-limits` | untouched |
| `sec-limits` | untouched | the whole section deleted, with `title-limits` and `p-limits` |

## Expected

* **Zero structural conflicts.** That is the correct structural answer, and it must not be presented
  as "merged cleanly" without qualification.
* Exactly **one broken reference** after the merge: `p-see-limits` now points at an id that no
  longer exists. Silently producing a dead `@href` is the failure this case exists for.
* `p-see-item` and `p-see-stable` point at targets neither user touched and must **not** be flagged.
  A checker that warns about every link is as useless as one that warns about none.
* If the tool offers to fix it, the only safe automatic options are removing the `<xref>` wrapper and
  keeping its text, or re-inserting B's deleted target. Rewriting the `@href` to some other id must
  never be automatic.

## Related cases

Renaming, rather than deleting, a link target is CF-18.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`.
