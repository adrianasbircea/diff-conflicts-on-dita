# Colliding comment identifiers (CF-24, 3-way)

**Base:** `comment-id-collision-base.dita` -> **User A:** `comment-id-collision-user-a.dita`, **User B:** `comment-id-collision-user-b.dita`

A and B are derived directly and independently from the base. The base has no review markup, and
neither user changed the document text.

Comment ids are generated per document, and generators are usually sequential or short. Two
reviewers who start from the same clean file will both produce `cmt-1`, `cmt-2`, `cmt-3` - for
completely different threads, in completely different places. The merge is then holding two distinct
threads that claim the same identity, and replies are attached by `@parentID`, which means a
mis-resolved collision silently moves a reply from one reviewer's thread to the other's.

Nothing in this file set is a content conflict. Everything here is an identity problem.

## Colliding ids

| ID | Comment id | User A uses it for | User B uses it for | Expected |
| --- | --- | --- | --- | --- |
| CF-24a | `cmt-1` | a thread on `p-intro`, **with a reply** (`parentID="cmt-1" mid="1"`) | a thread on `note-warn`, **with its own reply** (`parentID="cmt-1" mid="1"`) | two separate threads in the merge, each with exactly one reply. The reply written in A's branch must stay on `p-intro`; the reply written in B's branch must stay on `note-warn`. One id must be renamed, **and every `@parentID` that pointed at it must be renamed with it** |
| CF-24b | `cmt-2` | a text range inside `li-prereq-2` | the whole cell `e-sec-status` | two threads, on two different anchors, with distinct ids after the merge |
| CF-24c | `cmt-3` | the whole `p-config-lead`, comment text *Comment from A's branch ...* | the whole `p-config-lead`, comment text *Comment from B's branch ...* | **the same id and the same anchor, but different comment text and different authors.** These are two remarks, not one: the merge must keep both. Deduplicating on `@id` here loses a reviewer's comment silently |

## Global expectations

* After the merge, every `@id` on an `oxy_comment_start` is unique in the document.
* Every `@parentID` resolves to exactly one existing thread, and `@mid` values are unique within
  their thread.
* Six threads and two replies survive: three from A (one with a reply), three from B (one with a
  reply). A count of five, or of eight, both indicate a mishandled collision.
* Zero text differences: base, A and B have identical character content.
* Check in the Review Panel, not only in the source. The failure mode this case is built for looks
  correct in the XML - balanced PIs, valid ids - and shows up as a reply sitting under the wrong
  remark.
* Renaming is the expected fix. Whichever side is renamed, the choice must be applied consistently
  to the thread and all of its replies, and must not collide with an id already used by the other
  side.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`.
