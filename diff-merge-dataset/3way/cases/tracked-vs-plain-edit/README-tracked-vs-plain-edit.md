# Tracked change against plain edit (CF-25, 3-way)

**Base:** `tracked-vs-plain-edit-base.dita` -> **User A:** `tracked-vs-plain-edit-user-a.dita`, **User B:** `tracked-vs-plain-edit-user-b.dita`

A and B are derived directly and independently from the base. The base has no review markup.

CF-08 in the main set is tracked-vs-tracked: both reviewers had track changes on. This case is the
asymmetric one, which is what actually happens in a team: **A worked with track changes on, B had
them off**, and they edited the same text.

The difficulty is that A's edits are proposals and B's are facts. A merge that resolves them by
comparing the resulting text throws away the distinction, and the surviving change stops being
accept/rejectable - or worse, stays rejectable but rejects back to B's wording instead of the base
wording.

## A - track changes on

| Marker | Location | Change |
| --- | --- | --- |
| `oxy_delete` | `p-intro` | tracked deletion of *validates user credentials and* |
| `oxy_insert` | `p-arch` | tracked insertion of *and no session data in memory* |
| `oxy_insert` | `li-step-1` | tracked insertion of *and wait for the connections to drain* |
| `oxy_delete` + `oxy_insert` | `e-lim-ttl-def` | tracked modification `3600` -> `7200` |
| `oxy_delete` | `p-trouble` | tracked deletion of *the audit log and* |

## B - track changes off

| Location | Change |
| --- | --- |
| `p-intro` | plain rewrite: *authenticates the user and returns signed access tokens* |
| `p-arch` | plain edit: *local disk* -> *local file system* |
| `li-step-1` | the whole list item plainly deleted |
| `e-lim-ttl-def` | plain edit: `3600` -> `1800` |
| `p-trouble` | plain edit: *audit log* -> *audit archive* |

## Cases

| ID | Location | Expected |
| --- | --- | --- |
| CF-25a | `p-intro` | **conflict**: A proposed deleting the fragment, B replaced it. If A's deletion survives, it must still be a tracked deletion whose *reject* restores the **base** wording *validates user credentials and* - not B's wording |
| CF-25b | `p-arch` | **clean merge**: A's insertion is at the end of the sentence, B edited *local disk* before it. Result: *no session data on the local file system and no session data in memory*, with A's part still marked as an insertion |
| CF-25c | `li-step-1` | **conflict**: A tracked an insertion inside an item that B deleted outright. Either the item is gone, or it survives with the insertion still rejectable. What must not happen: the item survives with A's inserted text merged in as plain content |
| CF-25d | `e-lim-ttl-def` | **conflict** on one cell: tracked `3600`->`7200` against plain `3600`->`1800`. Accepting A must give `7200`; rejecting A must give `3600`, never `1800` |
| CF-25e | `p-trouble` | **conflict**: B edited the exact words A tracked for deletion. A surviving `oxy_delete` must carry the base text in its `@content`, not *the audit archive* |
| CF-25f | `li-step-2`, `e-lim-ttl-name` | untouched by both, must not be reported |

## Global expectations

* Four conflicts (CF-25a, c, d, e) and one clean merge (CF-25b).
* Every tracked change that survives the merge must keep its own author and timestamp, and must
  still be individually acceptable and rejectable.
* **The reject test is the real check.** After merging, reject every surviving tracked change: the
  result must be the base wording plus B's plain edits, with no leftover PI. Accept every surviving
  tracked change instead: the result must have no PI either. If either pass leaves an
  `oxy_insert_*`, `oxy_delete` or a fragment of the other side's text, the merge lost the
  distinction between a proposal and a fact.
* `@content` on every surviving `oxy_delete` must be text that actually existed in the base.
* `<?oxy_options track_changes="on"?>` is present in A and absent in B; that alone is not a document
  change.

## Opening in Web Author

See the end of `../../../2way/README-2way.md`.
