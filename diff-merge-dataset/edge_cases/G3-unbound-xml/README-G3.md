# G3 - Unbound XML, no framework

**Priority:** high value
**Files:** `G3-base.xml` -> `G3-modified.xml` (not DITA, by design - there is no DTD, no schema and
no DITA vocabulary to lean on)
**Change:** the `audit-sink` dependency is promoted from `kind="soft"` to `kind="hard"`. Nothing
else in the document changes.
**Fed to the AI:** two `<service>` fragments in a vocabulary the model has never seen.
**Expected:** explain the change in terms of the element and attribute names actually present, and
say plainly that the meaning of `kind="hard"` is defined outside the document.
**Fails if:** it invents DITA semantics, asserts a meaning for `kind` as if it knew it, or declines
to answer because the vocabulary is unknown.
