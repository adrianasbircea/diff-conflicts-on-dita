# A7 - Empty document on one side

**Priority:** high value
**Files:** `A7-base.dita` -> `A7-modified.dita`
**Change:** an empty `<body>` is filled with a whole section.
**Fed to the AI:** an empty left fragment and a large right fragment.
**Expected:** treat it as new content rather than a rewrite, suggest taking the right side as a
whole, and keep the explanation short.
**Fails if:** it reports many separate changes, or tries to align sentences that have no left-hand
counterpart.
