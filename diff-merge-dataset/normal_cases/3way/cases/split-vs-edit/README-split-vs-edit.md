# Split and join against editing (CF-14, 3-way)

**Base:** `split-vs-edit-base.dita` -> **User A:** `split-vs-edit-user-a.dita`, **User B:** `split-vs-edit-user-b.dita`

A and B are derived directly and independently from the base. Neither file has review markup.

**User A only moved boundaries**, without changing a character of text: one paragraph split in two,
two paragraphs joined into one, one list item split in two. **User B only edited text**, in the very
sentences that A moved across those boundaries.

The hard part is that A's split makes one of B's two edits belong to a paragraph that does not exist
in the base, and A's join makes one of B's edits belong to a paragraph that no longer exists in A.

## User A - boundaries only

| ID | Location | Change |
| --- | --- | --- |
| A-S01 | `p-split` | split after sentence 1; sentences 2 and 3 move into a new `p-split-2` |
| A-S02 | `p-join-1`, `p-join-2` | joined: both sentences end up in `p-join-1`, `p-join-2` is gone |
| A-S03 | `li-split` | split after sentence 1; sentence 2 moves into a new `li-split-2` |

## User B - text only

| ID | Location | Change |
| --- | --- | --- |
| B-S01 | `p-split`, sentence 1 | *validates* -> *checks* |
| B-S02 | `p-split`, sentence 2 | *one hour* -> *two hours* |
| B-S03 | `p-join-2` | *Any instance* -> *Every instance* |
| B-S04 | `li-split`, sentence 1 | *revision 42* -> *revision 47* |
| B-S05 | `li-split`, sentence 2 | *with the service* -> *with the service and with the installer* |

## Expected

| ID | Case | Expected |
| --- | --- | --- |
| CF-14a | split, edit before the split point | `p-split` reads *checks* (B-S01 lands in the first paragraph) |
| CF-14b | split, edit after the split point | `p-split-2` reads *two hours* (B-S02 follows the sentence into the new paragraph); it must not be dropped, and must not be applied to `p-split` |
| CF-14c | join, edit in the second block | the joined `p-join-1` reads *The service is stateless. Every instance can serve any request.* B-S03 survives even though its element is gone |
| CF-14d | list item split, edits on both sides | `li-split` reads *revision 47*, `li-split-2` reads *and with the installer* |
| CF-14e | no duplication | the merged file contains each sentence exactly once. *Any / Every instance can serve any request* must not appear twice, in `p-join-1` and in a surviving `p-join-2` |
| CF-14f | `li-keep` | unchanged by both; must not be reported at all |

## Global expectations

* Base vs A has zero text differences; base vs B has five.
* Best result: clean merge on all five of B's edits. A conflict on CF-14b or CF-14c is acceptable if
  reported precisely; **silently losing B-S02 or B-S03 is the failure this case is built to catch.**
* Sentence and word counts in the merged file must match: 3 sentences from `p-split`, 2 from the
  join, 2 from the list item.
* If the tool reports the whole paragraph as rewritten in A, the split was not detected as a split.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`.
