# B7 - XML comment changed

**Priority:** high value
**Files:** `B7-base.dita` -> `B7-modified.dita`
**Change:** the `<!-- ... -->` authoring note was replaced. Published content is identical.
**Fed to the AI:** two fragments differing only inside an XML comment.
**Expected:** identify it as an XML comment, an authoring note that is not published, and not a
review comment; state that the topic content is unchanged.
**Fails if:** it calls it a review comment, or reports a change to the paragraph.

Contrast with the review comments in `../base/2way` (cases C-01 to C-12), which are carried by
`oxy_comment_*` PIs and are real review markup. Both look like "a comment changed" in the raw XML
and must be told apart.
