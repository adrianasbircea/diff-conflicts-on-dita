# C3 - Change introduces a duplicate ID

**Priority:** discriminator
**Mode:** 2-way
**Files:** `C3-base.dita` -> `C3-modified.dita`
**Conflict block:** `c3-fig-detail` -> `c3-fig-install` in `c3-sec-detail`
**Change:** the figure title and text are edited, and its `@id` is changed from `c3-fig-detail` to
`c3-fig-install` - an id already used by the figure in `c3-sec-overview`.
**Out-of-block signal:** the earlier `<fig id="c3-fig-install">`.
**Expected with whole files:** accept the wording, flag the id collision, suggest a unique id.
**Predicted with block only:** treats the id change as harmless renaming and accepts it, producing
invalid XML.
**Score on:** no repeated `@id` value in the result; does the answer mention the collision.
**Not a duplicate of** `base/2way` AT-05, which changes an `@id` but collides with nothing.
