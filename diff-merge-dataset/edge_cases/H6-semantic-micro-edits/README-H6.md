# H6 - Tiny edit with large meaning

**Priority:** must pass
**Files:** `H6-base.dita` -> `H6-modified.dita`
**Change:** one token, in `h6-p-encrypt`: *is exposed* -> *is not exposed*. The two surrounding
paragraphs are identical in both files.
**Fed to the AI:** two `<p>` fragments differing by one word.
**Expected:** state the change in meaning, not the characters - the security claim is inverted, the
key is now said to stay out of the application log.
**Fails if:** it describes the edit mechanically ("a word was inserted") or rates it as minor
because the edit is small.
