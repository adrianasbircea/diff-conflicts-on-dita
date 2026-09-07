# Tracked change against plain edit (CF-25, 3-way)

**Base:** `tracked-vs-plain-edit-base.dita` -> **User A:** `tracked-vs-plain-edit-user-a.dita`, **User B:** `tracked-vs-plain-edit-user-b.dita`

A and B are derived directly and independently from the base. The base has no review markup.

CF-08 in the main set is tracked-vs-tracked: both reviewers had track changes on. This case is the
asymmetric one, which is what actually happens in a team: **A worked with track changes on, B had
them off**, and they edited the same text.

The difficulty is that A's edit is a proposal and B's is a fact. A merge that resolves them by
comparing the resulting text throws away the distinction, and the surviving change stops being
accept/rejectable - or worse, stays rejectable but rejects back to B's wording instead of the base
wording.

## The change

| Location | Base | User A (track changes on) | User B (track changes off) |
| --- | --- | --- | --- |
| `p-intro` | *validates user credentials and issues signed access tokens* | tracked deletion of *validates user credentials and* | plain rewrite: *authenticates the user and returns signed access tokens* |

`<?oxy_options track_changes="on"?>` is present in A and absent in B; that alone is not a document
change.

## Expected

* **One conflict.** A proposed deleting the fragment, B replaced it.
* If A's deletion survives, it must still be a tracked deletion whose *reject* restores the **base**
  wording *validates user credentials and* - not B's wording. `@content` on a surviving `oxy_delete`
  must be text that actually existed in the base.
* A surviving tracked change keeps its own author and timestamp, and stays individually acceptable
  and rejectable.
* **The reject test is the real check.** After merging, reject every surviving tracked change: the
  result must be the base wording plus B's plain edit, with no leftover PI. Accept instead: the
  result must have no PI either. If either pass leaves an `oxy_insert_*`, an `oxy_delete` or a
  fragment of the other side's text, the merge lost the distinction between a proposal and a fact.
* `p-arch`, `li-step-1`, the table and the troubleshooting section are untouched by both and must be
  reported nowhere.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`.
