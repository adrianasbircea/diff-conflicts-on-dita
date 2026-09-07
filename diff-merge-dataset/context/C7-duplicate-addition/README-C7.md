# C7 - Same warning added by both sides, in different places

**Priority:** discriminator
**Mode:** 3-way
**Files:** `C7-base.dita`, `C7-user-a.dita`, `C7-user-b.dita`
**Conflict block:** the inserted `c7-note-limit`
**Change:** A adds a `<note type="caution">` with the 40 psi limit. B adds the same limit as a
second sentence inside `c7-p-lead`.
**Out-of-block signal:** B's sentence in the adjacent paragraph, which the block view presents as a
separate, unrelated change.
**Expected with whole files:** report that the two sides added the same warning twice and recommend
keeping one - the note being the better home for it.
**Predicted with block only:** a clean, non-conflicting safety addition - keep it. The merged topic
then states the 40 psi limit twice, two lines apart.
**Score on:** *40 psi* appears once in the result.
