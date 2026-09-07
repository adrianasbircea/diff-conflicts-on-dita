# The same `@id` renamed differently on both sides (CF-18, 3-way)

**Base:** `id-renamed-both-sides-base.dita` -> **User A:** `id-renamed-both-sides-user-a.dita`, **User B:** `id-renamed-both-sides-user-b.dita`

A and B are derived directly and independently from the base. Neither file has review markup.

`@id` is not an ordinary attribute: most XML diff engines use it as the primary key for matching
elements between two trees. When both sides change it to different values, the key is gone in both
branches and a naive matcher reports the whole subtree as deleted on one side and inserted on the
other - losing every edit inside it.

Each user also repointed the `<xref>` that targets the section, because a rename that leaves the
reference behind is a different case (CF-19).

## The change

| Location | Base | User A | User B |
| --- | --- | --- | --- |
| the second `<section>` | `id="sec-limits"` | `id="sec-rate-limits"` | `id="sec-service-limits"` |
| `p-see` | `@href` ends in `/sec-limits` | repointed at `sec-rate-limits` | repointed at `sec-service-limits` |
| `p-limits`, inside that section | *applied per client identifier* | untouched | *... and per tenant* |

## Expected

* **One conflict**, on the `@id` - and with it the `@href`, which must resolve to whichever id wins.
  A per-attribute resolution that treats them independently will happily produce an unresolvable
  link; the tool should either keep them consistent or warn.
* **B's edit to `p-limits` must merge cleanly.** This is the payload of the case: if it is lost, the
  rename broke subtree matching and the section was diffed as a delete plus an insert.
* `title-limits`, `p-limits-2`, `p-arch` and the list are identical everywhere and must not be
  reported.
* After the conflict is resolved, run a validation and a link check: no duplicate `@id`, and every
  `@href` resolves to an element that exists in the merged file.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`.
