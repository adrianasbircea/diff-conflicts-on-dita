# Accept against reject, on a base that already has review markup (CF-21, 3-way)

**Base:** `accept-vs-reject-base.dita` -> **User A:** `accept-vs-reject-user-a.dita`, **User B:** `accept-vs-reject-user-b.dita`

A and B are derived directly and independently from the base.

**This is the only file set in the dataset whose base carries review markup.** Everywhere else the
base is clean and the tracked changes exist only in the derived files, so the most common real
situation is never tested: a document goes out for review with a change already tracked, two
reviewers open it, one accepts and the other rejects.

## The change

| Location | In the base | User A accepted | User B rejected |
| --- | --- | --- | --- |
| `p-intro` | `oxy_delete` of *behind a load balancer* | text stays out: *scaled horizontally.* | text returns: *scaled horizontally behind a load balancer.* |

`<?oxy_options track_changes="on"?>` is present in the base and in A, absent in B. That difference
alone must not be reported as a document change.

## Expected

* **One conflict**, on that fragment. Both sides removed the `oxy_delete` PI, so a diff that only
  looks for review PIs sees nothing and silently picks one wording - that is the failure this case
  exists for.
* **The merged file must contain no `oxy_delete` PI at all**: both users finished reviewing, so the
  tracked change is resolved in both branches. A merge that reintroduces it is wrong even if the
  text is right.
* Everything else in the document is identical in all three versions and must be reported nowhere.
* Reversing A and B must produce the same conflict.

## Variant worth adding later

The harder version: instead of rejecting, B *edits inside* the tracked deletion. Then accepting has
to drop text B wrote, and rejecting has to bring back text B deliberately removed.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`.
