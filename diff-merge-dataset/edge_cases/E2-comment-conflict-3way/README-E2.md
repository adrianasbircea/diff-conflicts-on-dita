# E2 - Two reviewers comment on the same text (3-way)

**Priority:** high value
**Files:** `E2-base.dita` -> `E2-user-a.dita`, `E2-user-b.dita`
**Change:** the base has no review markup. A and B each anchor a different comment on the same
`200`. Neither changes the text.
**Fed to the AI:** three fragments; A and B differ from the base only by their comment PIs.
**Expected:** state that the text is identical in all three versions and that the conflict is
between two review comments, not between two edits; summarise both comments and note that they
disagree in substance (A asks about profiles, B says the value is outdated); recommend keeping both
comments rather than dropping one.
**Fails if:** it reports a content conflict on the number 200, or silently keeps one comment.
