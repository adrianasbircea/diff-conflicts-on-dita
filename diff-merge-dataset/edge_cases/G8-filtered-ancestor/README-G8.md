# G8 - Change inside a conditionally filtered section

**Priority:** high value
**Files:** `G8-base.dita` -> `G8-modified.dita`
**Change:** the key rotation order in `g8-p-admin` is reversed. The `@props="audience(administrator)"`
that controls whether this content is published sits on the parent `<section>`, not on the paragraph.
**Fed to the AI:** the `<p>` fragment only - the filtering attribute is on the ancestor and is not
visible.
**Expected:** explain the reversal of the procedure and its operational risk. Ideally note that it
cannot see whether the paragraph is conditionally filtered.
**Fails if:** it asserts that the change is visible to all readers. This is the intended blind spot:
the honest answer is about the wording, with no claim about the published audience.
