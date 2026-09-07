# The same `@id` renamed differently on both sides (CF-18, 3-way)

**Base:** `id-renamed-both-sides-base.dita` -> **User A:** `id-renamed-both-sides-user-a.dita`, **User B:** `id-renamed-both-sides-user-b.dita`

A and B are derived directly and independently from the base. Neither file has review markup.

`@id` is not an ordinary attribute: most XML diff engines use it as the primary key for matching
elements between two trees. When both sides change it to different values, the key is gone in both
branches and a naive matcher reports the whole subtree as deleted on one side and inserted on the
other - losing every edit inside it.

Two of the renamed ids are also link targets, so this case doubles as a referential integrity test.

## Conflicts

| ID | Location | Base `@id` | User A | User B | Expected |
| --- | --- | --- | --- | --- | --- |
| CF-18a | the second `<section>` | `sec-limits` | `sec-rate-limits` | `sec-service-limits` | conflict on `@id` only. The section must still be matched as **one** section in all three trees: its title, `p-limits` and `p-limits-2` must be diffed normally, not reported as a deleted plus an inserted subtree |
| CF-18b | `p-limits` (inside that section) | - | unchanged | text extended (*and per tenant*) | **must merge cleanly**, despite CF-18a on the parent. This is the payload of the case: if B's edit is lost, the rename broke subtree matching |
| CF-18c | second item of `ul-prereq` | `li-prereq-2` | `li-schema` | `li-db-schema` | conflict on `@id`; B's text change *revision 42* -> *47* must survive |
| CF-18d | `p-see` | - | `@href` -> `#.../sec-rate-limits` | `@href` -> `#.../sec-service-limits` | conflict on `@href`. Whichever `@id` wins CF-18a, the resolved `@href` must point at it - a merge that picks A's id and B's href produces a broken link |

## Must not produce a conflict

| ID | Location | User A | User B | Expected |
| --- | --- | --- | --- | --- |
| CF-18e | `p-arch` | `@id` renamed to `p-architecture` | text extended (*before they are returned*) | clean merge: id `p-architecture`, text with B's addition. Only one side touched each |

## Global expectations

* Four conflicts (CF-18a, c, d) plus their linkage, and exactly one clean merge (CF-18e).
* `p-limits-2`, `li-prereq-1`, `title-limits` and `title-overview` are identical everywhere and must
  not be reported.
* After every conflict is resolved, run a validation and a link check: no duplicate `@id`, and every
  `@href` resolves to an element that exists in the merged file.
* CF-18d is the trap: a per-attribute resolution that treats `@id` and `@href` independently will
  happily produce an unresolvable link. The tool should either keep them consistent or warn.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`.
