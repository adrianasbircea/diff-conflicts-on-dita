# C10 - Whole-file reformat around one real edit

**Priority:** discriminator
**Mode:** 2-way
**Files:** `C10-base.dita` -> `C10-modified.dita`
**Conflict block:** `c10-p-run`
**Change:** the file is reindented from 2 to 4 spaces, every paragraph is rewrapped, and the
attributes are reordered on every element. The only content edit is 30 -> 90 days in `c10-p-keep`.
**Out-of-block signal:** the same reflow and attribute reordering on every other element, which
marks the pattern as mechanical.
**Expected with whole files:** report that the file was reformatted, that `c10-p-run` has no content
change, and that the single real edit is the retention period.
**Predicted with block only:** treats the rewrap and the attribute order as a change worth
explaining, or invents a semantic difference to justify the highlight.
**Score on:** does the answer say there is no content change in this block.
