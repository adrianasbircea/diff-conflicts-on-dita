# 3-Way Diff/Merge Test Dataset

**Base:** `3way-base.dita` -> **User A:** `3way-user-a.dita`, **User B:** `3way-user-b.dita`

A and B are derived **directly and independently** from the same base (B is not derived from A). The
base has no review markup; comments and tracked changes exist only in A and B, using the standard
Oxygen PIs (see `../2way/README-2way.md`). The base content is identical to `2way-base.dita`, so the
element IDs are the same in both scenarios.

Case IDs (`A-N` = only in A, `B-N` = only in B, `ID-` = identical in both, `CF-` = conflict) are
only labels for tests and bug reports; *Location* holds the real `@id`.

## Only in User A - expected to merge without conflict

| ID | Location | Change |
| --- | --- | --- |
| A-N01 | `p-arch` | paragraph modified (*... and no session data in memory*) |
| A-N02 | `p-arch-note` | new `<p>` added |
| A-N03 | `p-legacy` | `<p>` deleted |
| A-N04 | `shortdesc` | `@outputclass="lead"` added |
| A-N05 | `li-prereq-1` | list item modified (*or later*) |
| A-N06 | `li-step-5` | new step added to `ol-steps` |
| A-N07 | `e-lim-ttl-def` | table cell modified (3600 -> 7200) |
| A-N08 | `li-step-1` | tracked insertion in a list item |
| A-N09 | `e-sec-desc` | tracked deletion in a table cell |
| A-N10 | `row-lim-clock` | tracked insertion of a whole table row |
| A-N11 | `table-endpoints` | tracked attribute change `@frame` `all` -> `topbot` |
| A-N12 | `cmt-a-cell-auth-desc` | **comment on a table cell** (`e-auth-desc`) |
| A-N13 | `cmt-a-li-step-2` | comment on a list item, with a reply |
| A-N14 | `cmt-a-features-lead` | comment marked done (`flag="done"`) |

## Only in User B - expected to merge without conflict

| ID | Location | Change |
| --- | --- | --- |
| B-N01 | `p-config-lead` | paragraph rewritten (A only comments on it, see CF-09) |
| B-N02 | `li-prereq-3` | list item modified (port 443 behind a proxy) |
| B-N03 | `li-prereq-5` | new list item added |
| B-N04 | `p-trouble-extra` | new `<p>` added |
| B-N05 | `p-endpoints-lead` | `<p>` deleted |
| B-N06 | `note-warn` | `@type` `warning` -> `danger` |
| B-N07 | `e-lim-rate-def` | table cell modified (600 -> 1200) |
| B-N08 | `li-stage-1` | tracked deletion in a list item |
| B-N09 | `e-lim-rate-notes` | tracked insertion in a table cell |
| B-N10 | `table-limits` | tracked attribute change `@colsep` 0 -> 1 |
| B-N11 | `cmt-b-row-sec` | **comment on a table row** (`row-sec`), with a reply |
| B-N12 | `cmt-b-dd-slow` | comment on a whole element (`dd-slow`) |

## Identical in A and B - must not produce a conflict

| ID | Location | Change in both |
| --- | --- | --- |
| ID-01 | `li-prereq-2` | *revision 42* -> *revision 47* |
| ID-02 | `dt-401` | *Every request is answered with 401* -> *All requests ...* |
| ID-03 | `e-lim-pool-max` | 200 -> 400 |

## Conflicts

| ID | Location | Base | User A | User B | Expected |
| --- | --- | --- | --- | --- | --- |
| CF-01 | `p-intro` | *validates user credentials and issues signed access tokens* | *authenticates the user credentials and returns a signed access token* | *checks the user credentials and hands out signed access tokens* | conflict on the first sentence only; the rest of the paragraph is identical everywhere |
| CF-02 | `e-authz-status` | `Active` | `Deprecated` | `Beta` | conflict on that single cell; the rest of `row-authz` merges |
| CF-03 | `note-config` | `<note type="note">` | deletes the note | `@type` -> `tip`, text extended | delete-vs-modify conflict |
| CF-04 | `table-features/@frame` | `all` | `topbot` | `none` | attribute conflict; the rest of the table merges |
| CF-05 | `cb-config` | `auth.token.ttl=3600` | `7200` | `1800` + a new property line | conflict inside the code block |
| CF-06 | `row-log` | status `Planned`, owner `Observability team` | description + status `Active` | description + status `Active` + owner `Platform team` | conflict on `e-log-desc` and `e-log-owner`; `e-log-status` is `Active` in both and must merge |
| CF-07 | `li-profile-prod` | third item of `ul-profiles` | moves it to the first position | renames it *Production, multi-zone* | conflict, or a merge keeping both; the item must not be duplicated and its nested `<ul>` must survive |
| CF-08 | `p-deploy-lead` | plain sentence | tracked insertion *, and a custom profile ...* | tracked insertion *, and each one can be tuned ...* | conflict; the surviving insertion must still be a tracked change that can be accepted/rejected |
| CF-09 | `p-config-lead` | plain paragraph | adds `cmt-a-config-lead`, text unchanged | rewrites the text (B-N01) | ideally not a hard conflict: B's text wins, A's comment stays anchored |
| CF-10 | `p-arch` | *The service keeps no session data on the local disk ...* | rewrites exactly that fragment (A-N01) | adds `cmt-b-arch-sel` on that fragment | the commented text no longer exists in A - check whether the merge re-anchors or drops the comment, and that the comment PIs stay balanced |

## Global expectations

* Everything merges cleanly except CF-01..CF-08; CF-09 and CF-10 are review-anchoring cases, to be
  inspected rather than simply resolved.
* Tracked changes from A and from B both survive the merge, with their own author and timestamp, and
  stay individually acceptable/rejectable.
* Comments from A and from B are all present after the merge, with the reply threads
  (`cmt-a-li-step-2`, `cmt-b-row-sec`) and `flag="done"` on `cmt-a-features-lead` intact.
* No merge result may contain an unbalanced comment or insert PI.

## Opening in Web Author

See the end of `../2way/README-2way.md`. Verified on WA 28.1: base has no review entries at all,
A shows 9 (4 comment threads incl. 1 reply + 4 tracked changes), B shows 7 (3 threads incl. 1 reply
+ 3 tracked changes), with the intended anchors - B's row comment highlights all 4 cells of
`row-sec`, A's cell comment only `e-auth-desc`. As in the 2-way set, the tracked **attribute**
changes (A-N11, B-N10) are applied but not listed as Review Panel entries.
