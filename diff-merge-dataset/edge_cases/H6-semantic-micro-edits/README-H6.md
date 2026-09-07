# H6 - Tiny edits with large meaning

**Priority:** must pass
**Files:** `H6-base.dita` -> `H6-modified.dita`
**Change:** three one-token edits, each in its own paragraph - `10` -> `100` days, *is exposed* ->
*is not exposed*, *may* -> *must*.
**Fed to the AI:** each paragraph separately, one change per request.
**Expected:** for each one, state the change in meaning, not the characters: retention grows tenfold,
the security claim is inverted, and an optional step becomes mandatory.
**Fails if:** it describes them mechanically ("a digit was added", "a word was inserted") or rates
them as minor because the edit is small.
