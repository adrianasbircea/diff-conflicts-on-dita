# G1 - Same edit, author mode and text mode

**Priority:** high value
**Files:** `G1-base.dita` -> `G1-modified.dita`
**Change:** one word on the middle line of `g1-p`: *three* -> *two* consecutive checks.
**Fed to the AI:** run the same pair twice - once in author mode, once after switching to text mode.
Author mode delivers the paragraph as a block; text mode is line based and delivers only the middle
line, without the `<p>` start tag.
**Expected:** the same explanation and the same suggestion in both modes. In text mode the answer
must still be about the failover threshold, not about "a line changed".
**Fails if:** the two modes give materially different explanations, or the text-mode answer degrades
because the fragment is a bare line rather than an element.
