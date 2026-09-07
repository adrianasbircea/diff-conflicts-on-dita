# C9 - Reused phrase inlined while its definition changes

**Priority:** discriminator
**Mode:** 3-way
**Files:** `C9-base.dita`, `C9-user-a.dita`, `C9-user-b.dita`
**Conflict block:** `c9-p-launch`
**Change:** A renames the reusable phrase `c9-ph-prod` to *Acme Publisher Pro* and adds *or from the
desktop shortcut* to the sentence, keeping the `conref`. B rewrites the sentence, replaces the
`conref` with the literal *Acme Publisher*, and adds a first-start note.
**Out-of-block signal:** the updated `<ph id="c9-ph-prod">` in `c9-sec-names`, and the two sibling
paragraphs that still resolve through the `conref`.
**Expected with whole files:** keep the `conref` and fold in B's wording; the literal string freezes
a name that was just retired and would leave one paragraph out of step with the other two.
**Predicted with block only:** prefers B's literal text because it reads complete on its own, or
declares the `conref`-vs-literal choice a matter of style.
**Score on:** the suggested text keeps `<ph conref="#c9-launch/c9-ph-prod"/>` and does not hardcode
*Acme Publisher*.
