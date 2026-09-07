# C13 - Delete-vs-modify where the other side already made the edit

**Priority:** discriminator
**Mode:** 3-way
**Files:** `C13-base.dita`, `C13-user-a.dita`, `C13-user-b.dita`
**Conflict block:** `c13-p-retry`
**Change:** A rewrites `c13-p-retry` in place to add the backoff detail. B deletes it from
*Error responses* and opens a *Retry policy* section whose first paragraph already states the same
backoff rule, plus a jitter paragraph.
**Out-of-block signal:** `c13-sec-retry`, a new section further down B's file.
**Expected with whole files:** B's restructure already carries A's improvement - accept the deletion,
keep B's section, note that nothing of A's is lost.
**Predicted with block only:** a textbook delete-vs-modify - keep A's improved paragraph, which
leaves the same backoff rule stated in two sections.
**Score on:** *exponential backoff* appears once in the result.
**Not a duplicate of** `base/3way` CF-03, a delete-vs-modify conflict where the deleting side does
not re-create the content elsewhere. That re-creation is the whole case here.
