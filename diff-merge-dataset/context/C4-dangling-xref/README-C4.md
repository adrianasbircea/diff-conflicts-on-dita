# C4 - Cross-reference target renamed elsewhere

**Priority:** discriminator
**Mode:** 3-way
**Files:** `C4-base.dita`, `C4-user-a.dita`, `C4-user-b.dita`
**Conflict block:** `c4-p-ref`
**Change:** A extends the sentence and keeps `href="#c4-net/c4-step-verify"`. B renames the step id
to `c4-step-link-check` and updates the href, keeping the base wording.
**Out-of-block signal:** `c4-step-verify` no longer exists in B - the id lives in `c4-ol-steps`.
**Expected with whole files:** A's wording with B's href - the only combination that both reads
right and resolves.
**Predicted with block only:** picks one side whole; choosing A leaves a broken link.
**Score on:** the suggested href resolves to an id present in the merged file.
