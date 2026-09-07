# Adjacent, non-identical word edits (CF-15, 3-way)

**Base:** `adjacent-word-edits-base.dita` -> **User A:** `adjacent-word-edits-user-a.dita`, **User B:** `adjacent-word-edits-user-b.dita`

A and B are derived directly and independently from the base. Neither file has review markup.

Both users edit `p-far`, in the **same text node**, at different words. CF-01 in the main set only
covers a full sentence rewrite on both sides, so it never shows whether the merge granularity is the
word or the whole block. This case does.

## The change

| Location | Base | User A | User B |
| --- | --- | --- | --- |
| `p-far` | *validates the user credentials and issues signed access tokens* | *validates* -> *checks* (word 4) | *signed* -> *short-lived* (word 10) |

## Expected

* **Clean merge, zero conflicts**: *checks the user credentials and issues short-lived access
  tokens*. Both edits survive.
* If this is reported as a conflict, the merge granularity is the block, not the word.
* Every other paragraph in the file is identical in all three versions and must be reported
  nowhere.
* Reversing A and B must produce the same result.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`.
