# C6 - Inserted step invalidates a numeric reference

**Priority:** discriminator
**Mode:** 2-way
**Files:** `C6-base.dita` -> `C6-modified.dita`
**Conflict block:** the inserted `c6-step-ground` in `c6-ol-steps`
**Change:** a grounding step is inserted at position 2. Nothing else is edited.
**Out-of-block signal:** `c6-p-repeat`, in another section, says *Repeat steps 4 through 6* - now
off by one.
**Expected with whole files:** accept the step and state that `c6-p-repeat` must become *steps 5
through 7*.
**Predicted with block only:** accepts the step as a clean, safe improvement and says nothing about
the stale reference.
**Score on:** does the answer mention `c6-p-repeat` or the renumbering.
**Not a duplicate of** `base/2way` L-01, which inserts a list item with no numeric reference to it
anywhere else in the document.
