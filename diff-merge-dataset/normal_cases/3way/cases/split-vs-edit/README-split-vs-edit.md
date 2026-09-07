# Split against editing (CF-14, 3-way)

**Base:** `split-vs-edit-base.dita` -> **User A:** `split-vs-edit-user-a.dita`, **User B:** `split-vs-edit-user-b.dita`

A and B are derived directly and independently from the base. Neither file has review markup.

**User A only moved a boundary**, without changing a character of text: one paragraph split in two.
**User B only edited text**, in the very sentences that A moved across that boundary.

The hard part is that A's split makes one of B's two edits belong to a paragraph that does not exist
in the base.

## The change

| Location | User A | User B |
| --- | --- | --- |
| `p-split`, sentence 1 | split after sentence 1 | *validates* -> *checks* |
| `p-split`, sentence 2 | moves into a new `p-split-2`, with sentence 3 | *one hour* -> *two hours* |

## Expected

* `p-split` reads *checks* - B's first edit lands in the first paragraph.
* `p-split-2` reads *two hours* - B's second edit follows its sentence into the new paragraph. It
  must not be dropped, and must not be applied to `p-split`.
* Best result: a clean merge on both of B's edits. A conflict is acceptable if reported precisely;
  **silently losing the *two hours* edit is the failure this case is built to catch.**
* The merged file contains each sentence exactly once, three in total from `p-split`.
* If the tool reports the whole paragraph as rewritten in A, the split was not detected as a split.
* `p-join-1`, `p-join-2`, `li-split` and `li-keep` are untouched by both and must be reported
  nowhere.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`.
