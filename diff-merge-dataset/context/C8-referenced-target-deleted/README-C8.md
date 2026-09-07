# C8 - Deletion orphans a reference from elsewhere

**Priority:** discriminator
**Mode:** 2-way
**Files:** `C8-base.dita` -> `C8-modified.dita`
**Conflict block:** the deletion of `c8-sec-tuning`
**Change:** the whole *Tuning for large deployments* section is removed. Nothing else is edited.
**Out-of-block signal:** `c8-p-pointer`, untouched in the first section, still links to
`c8-sec-tuning`.
**Expected with whole files:** report that accepting the deletion breaks the link in
`c8-p-pointer`, and require that the reference be removed or repointed in the same edit.
**Predicted with block only:** a self-contained section removal - accept it, leaving a dangling
`<xref>`.
**Score on:** every `href` in the result resolves to an existing id.
**Distinct from C4:** C4 is about choosing the right href when the anchor moved; here the anchor is
deleted outright and the reference is not part of the change at all.
