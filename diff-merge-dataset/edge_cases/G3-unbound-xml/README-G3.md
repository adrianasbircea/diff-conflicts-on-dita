# G3 - Unbound XML, no framework

**Priority:** high value
**Files:** `G3-base.xml` -> `G3-modified.xml` (not DITA, by design - there is no DTD, no schema and
no DITA vocabulary to lean on)
**Change:** owning team renamed, SLA tightened (99.95 -> 99.99, 250 ms -> 150 ms), and the
`audit-sink` dependency promoted from soft to hard.
**Fed to the AI:** two `<service>` fragments in a vocabulary the model has never seen.
**Expected:** explain the changes in terms of the element and attribute names actually present, and
say plainly that the meaning of `kind="hard"` is defined outside the document.
**Fails if:** it invents DITA semantics, assumes a meaning for `tier` or `kind`, or declines to
answer because the vocabulary is unknown.
