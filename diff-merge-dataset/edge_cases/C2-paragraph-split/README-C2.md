# C2 - Paragraph split

**Priority:** high value
**Files:** `C2-base.dita` -> `C2-modified.dita`
**Change:** `c2-p` is split into two paragraphs. No wording changes.
**Fed to the AI:** one `<p>` on the left, two `<p>` elements on the right.
**Expected:** report a structural split with the wording preserved, and give the two paragraphs as
the replacement for the single one.
**Fails if:** it reports the second sentence as deleted, or as new text, or tries to match the two
right-hand paragraphs against one left-hand paragraph as two separate rewrites.
