# C1 - Document-wide terminology rename

**Priority:** discriminator
**Mode:** 3-way
**Files:** `C1-base.dita`, `C1-user-a.dita`, `C1-user-b.dita`
**Conflict block:** `c1-p-insert`
**Change:** A renames *Widget* -> *Device* in the title and all 8 body paragraphs. B touches only
`c1-p-insert`, adding *firmly* and keeping *Widget*.
**Out-of-block signal:** the 7 other paragraphs A renamed.
**Expected with whole files:** *Insert the Device firmly into the slot.* - A's term plus B's adverb,
stated as a deliberate rename.
**Predicted with block only:** one side wins whole, most likely B, reintroducing the retired term.
**Score on:** does the suggested text contain *Device* and *firmly*.
