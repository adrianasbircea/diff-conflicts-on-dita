# G2 - Not well-formed XML

**Priority:** must pass
**Files:** `G2-base.dita` -> `G2-modified.dita`
**Change:** *Retry with backoff.* added to `g2-p-503`. Both files are broken in the same way -
`g2-p-503` is never closed - so they open only in text mode.
**Fed to the AI:** two fragments that do not parse as XML.
**Expected:** explain the sentence that was added, and separately point out that the paragraph is
missing its closing tag in both versions. Do not silently repair the markup in the suggestion.
**Fails if:** it refuses to answer, returns nothing, or "helpfully" closes the tag in the suggested
text - that would be a second, unrequested edit the user did not review.
