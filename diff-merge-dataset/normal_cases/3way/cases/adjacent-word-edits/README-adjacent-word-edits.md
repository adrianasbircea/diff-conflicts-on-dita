# Adjacent, non-identical word edits (CF-15, 3-way)

**Base:** `adjacent-word-edits-base.dita` -> **User A:** `adjacent-word-edits-user-a.dita`, **User B:** `adjacent-word-edits-user-b.dita`

A and B are derived directly and independently from the base. Neither file has review markup.

Every paragraph here is touched by **both** users, in the **same text node**, but at different
words. CF-01 in the main set only covers a full sentence rewrite on both sides, so it never shows
whether the merge granularity is the word or the whole block. This case does.

`p-same` is the control: the same word changed on both sides, which must be a conflict. If every
paragraph in this file conflicts, the merge granularity is the block, not the word.

## Cases

| ID | Location | Base | User A | User B | Expected |
| --- | --- | --- | --- | --- | --- |
| CF-15a | `p-far` | *validates the user credentials and issues signed access tokens* | *validates* -> *checks* (word 4) | *signed* -> *short-lived* (word 10) | **clean merge**: *checks the user credentials and issues short-lived access tokens* |
| CF-15b | `p-next` | *Restart the service after ...* | *Restart* -> *Stop and restart* (word 1) | *the service* -> *each service instance* (words 2-3) | **clean merge or a narrow conflict**: the two edits touch consecutive words. If the tool merges, the result must read *Stop and restart each service instance after every configuration change* - not a garbled splice |
| CF-15c | `p-same` | *3600* | *7200* | *1800* | **conflict** on that one number only; the rest of the sentence merges |
| CF-15d | `p-inline` | *Set `auth.signing.key` to the path of the PEM file.* | *Set* -> *Always set*, before the `<codeph>` | *PEM file* -> *PEM key file*, after the `<codeph>` | **clean merge**: edits sit in two different text nodes, separated by an inline element. The `<codeph>` must be untouched and must not be duplicated |
| CF-15e | `p-ends` | *The service is stateless.* | sentence appended at the end | sentence prepended at the start | **clean merge**: *Requests are never pinned to an instance. The service is stateless. Any instance can serve any request.* Both insertions survive, in the right positions |

## Global expectations

* Exactly one conflict in the whole file: CF-15c.
* CF-15d must not report the `<codeph>` as changed; if it does, the differ is splitting mixed
  content on element boundaries rather than diffing across them.
* CF-15e is the ordering test: an insert at the start and an insert at the end of the same text node
  must not be reported as competing for the same position.
* Reversing A and B must produce the same set of conflicts.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`.
