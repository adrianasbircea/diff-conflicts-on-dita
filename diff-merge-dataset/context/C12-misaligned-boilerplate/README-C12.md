# C12 - Repeated boilerplate, edited in different occurrences

**Priority:** discriminator
**Mode:** 3-way
**Files:** `C12-base.dita`, `C12-user-a.dita`, `C12-user-b.dita`
**Conflict block:** whichever disclaimer pair the aligner produces
**Change:** the same two-sentence disclaimer appears after all three sections. A edits the second
occurrence (*before publication or distribution*), B edits the third (*within five working days*).
**Out-of-block signal:** the other two occurrences of the identical text. The disclaimers carry no
`@id` on purpose, as repeated boilerplate usually does, so alignment has to work from text alone and
can pair A's edit with the wrong occurrence.
**Expected with whole files:** three occurrences, two independent edits in different places, no real
conflict - keep both, each in its own section.
**Predicted with block only:** one conflict between two disclaimer variants; produces a merged
sentence and applies it to a single occurrence, losing one edit or moving it to the wrong section.
**Score on:** occurrence 1 unchanged, occurrence 2 carries A's wording, occurrence 3 carries B's.
**Note:** if the tool aligns on position rather than text and reports no conflict at all, that is
also a useful result - record it, the case then stops discriminating.
