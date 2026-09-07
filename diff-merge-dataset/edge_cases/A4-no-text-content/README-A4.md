# A4 - Block with no text content

**Priority:** high value
**Files:** `A4-base.dita` -> `A4-modified.dita`
**Change:** `a4-img` points at a different file and uses a different `@scale`.
**Fed to the AI:** two `<image/>` fragments, neither with any text content.
**Expected:** explain that the referenced image and its scale changed, and state that there is no
text to type - the suggestion is the markup itself.
**Fails if:** it produces a "use this text" answer, or describes what the images depict (it cannot
see them).
