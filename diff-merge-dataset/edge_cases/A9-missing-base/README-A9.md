# A9 - Three-way requested, base unavailable

**Priority:** must pass
**Files:** `A9-user-a.dita`, `A9-user-b.dita` (no base file, on purpose)
**Change:** token lifetime is *two hours* in A and *30 minutes* in B.
**Fed to the AI:** two fragments, in a flow the user started as a 3-way merge whose common ancestor
failed to load.
**Expected:** say that without the common version it cannot tell which side is the edit, show both
values, and ask instead of choosing.
**Fails if:** it asserts a direction ("changed from two hours to 30 minutes") or names one side as
authoritative. This input is indistinguishable from a plain 2-way compare, so overconfidence here is
overconfidence in every 3-way case.
