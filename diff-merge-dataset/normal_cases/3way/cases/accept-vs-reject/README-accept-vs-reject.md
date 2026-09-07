# Accept against reject, on a base that already has review markup (CF-21, 3-way)

**Base:** `accept-vs-reject-base.dita` -> **User A:** `accept-vs-reject-user-a.dita`, **User B:** `accept-vs-reject-user-b.dita`

A and B are derived directly and independently from the base.

**This is the only file set in the dataset whose base carries review markup.** Everywhere else the
base is clean and the tracked changes exist only in the derived files, so the most common real
situation is never tested: a document goes out for review with changes already tracked, two
reviewers open it, one accepts and the other rejects.

The base contains four tracked changes and two comment threads. A accepted every tracked change and
worked on the comments; B rejected every tracked change and removed one comment.

## In the base

| Marker | Location | What it is |
| --- | --- | --- |
| `oxy_delete` | `p-intro` | tracked deletion of *behind a load balancer* |
| `oxy_insert` | `li-step-1` | tracked insertion of *and wait for the open connections to drain* |
| `oxy_attributes` | `table-limits` | tracked attribute change `@colsep` `0` -> `1`; the element already carries the new value |
| `oxy_insert` | `p-trouble-metrics` | tracked insertion of a whole paragraph |
| `cmt-base-warn` | `note-warn` | open comment thread, no reply |
| `cmt-base-config` | `p-config-lead` | comment thread with one reply (`mid="1"`) |

## Conflicts

| ID | Location | User A accepted | User B rejected | Expected |
| --- | --- | --- | --- | --- |
| CF-21a | `p-intro` | text stays out: *scaled horizontally.* | text returns: *scaled horizontally behind a load balancer.* | conflict on that fragment. Both sides removed the `oxy_delete` PI, so a diff that only looks for review PIs sees nothing and silently picks one wording |
| CF-21b | `li-step-1` | *and wait for the open connections to drain* kept | the inserted text removed | conflict on the item text |
| CF-21c | `table-limits/@colsep` | `1` | `0` | conflict on the attribute. The `oxy_attributes` PI is gone in both, so the only trace of the disagreement is the attribute value itself |
| CF-21d | `p-trouble-metrics` | paragraph kept | paragraph gone | conflict: element kept vs element removed |
| CF-21e | `cmt-base-warn` | `flag="done"` added | the whole comment deleted | conflict on the comment: resolved vs removed. Whichever wins, the PIs must stay balanced and `note-warn` must keep its text |

## Must merge without conflict

| ID | Location | User A | User B | Expected |
| --- | --- | --- | --- | --- |
| CF-21f | `cmt-base-config` | a second reply added (`mid="2"`) | thread untouched | the thread survives with two replies, in order, each with its own author and timestamp |
| CF-21g | `p-config-lead` | text untouched | *Restart the service* -> *Restart every service instance* | B's text wins, and A's new reply stays anchored to the same paragraph |

## Global expectations

* Five conflicts (CF-21a..e), two clean merges (CF-21f, CF-21g).
* **The merged file must contain no `oxy_insert_*`, `oxy_delete` or `oxy_attributes` PI at all**:
  both users finished reviewing, so every tracked change is resolved in both branches. A merge that
  reintroduces a tracked change is wrong even if the text is right.
* Comment PIs must stay balanced: one `oxy_comment_end` per `oxy_comment_start`, with matching
  `@mid` values, and replies still nested inside their parent thread.
* `<?oxy_options track_changes="on"?>` is present in the base and in A, absent in B. That difference
  alone must not be reported as a document change.
* Reversing A and B must produce the same five conflicts.

## Variant worth adding later

The harder version of CF-21b: instead of rejecting, B *edits inside* the tracked insertion. Then
accepting keeps B's wording as part of an insertion that A already accepted, and rejecting has to
remove text B wrote.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`.
