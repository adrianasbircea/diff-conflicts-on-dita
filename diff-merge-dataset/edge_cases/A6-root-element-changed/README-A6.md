# A6 - Root element changed, nothing else

**Priority:** high value (added on request; unrated in the original catalogue)
**Files:** `A6-base.dita` -> `A6-modified.dita`
**Change:** the document element is renamed `topic` -> `concept`, and the DOCTYPE is updated to
match, as it must be. `@id`, `@xml:lang` and every child element are untouched.
**Fed to the AI:** the outermost element. This is the one position in a document with no ancestor
and no preceding sibling, so there is no surrounding context at all - the fragment is the whole
document, or just its start and end sentinel.

**Expected:**
* report a topic-type change (generic topic to concept), not a content change;
* say that no wording changed anywhere;
* flag that the file is now well-formed but **invalid**: `<concept>` does not allow `<body>`, so
  `a6-body` has to become `<conbody>` for the conversion to be complete;
* treat the DOCTYPE line as a consequence of the rename, not as the change itself.

**Fails if:** it reports a content or attribute change, describes the DOCTYPE as the substantive
edit, or accepts the rename as complete without noticing that `<body>` must become `<conbody>`.
That last point is what separates a real answer from a surface one.

**Note:** unlike every other set except `G2-not-well-formed`, the modified file here is deliberately
not schema-valid. That is the point of the case, not an oversight.
