# E5 - Plain edit next to a pre-existing tracked change

**Priority:** high value
**Files:** `E5-base.dita` -> `E5-modified.dita`
**Change:** *host* -> *build agent*. The tracked insertion *or later* is present and identical in
both files - it is not part of this change.
**Fed to the AI:** two `<li>` fragments, both containing `oxy_insert_start` / `oxy_insert_end` PIs.
**Expected:** report only the *host* to *build agent* edit, mention that the item also carries an
unresolved tracked insertion that this change does not touch, and keep the PIs intact in the
suggested text.
**Fails if:** it reports *or later* as added or removed, drops the PIs from the suggestion, or
proposes the PI text as literal prose.
