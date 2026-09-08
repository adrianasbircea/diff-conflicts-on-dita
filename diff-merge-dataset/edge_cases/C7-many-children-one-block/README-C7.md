# C7 - One block, 27 changes inside it

**Priority:** high value
**Files:** `C7-base.dita` -> `C7-modified.dita`
**Change:** 27 of the 34 property lines in `c7-cb` change value; 7 are untouched.
**Fed to the AI:** one `<codeblock>` fragment per side, each with a large number of second-level
differences under a single parent.
**Expected:** a short summary organised by theme - tokens, rate limits, pool, TLS, logging, cache,
metrics - naming the security-relevant ones (RS256 to RS512, TLSv1.2 to TLSv1.3, audit logging
turned on) and saying which lines are unchanged. The suggestion is to take the right-hand block.
**Fails if:** it prints a line-by-line list of all 27 changes, or truncates silently and describes
only the first few.
