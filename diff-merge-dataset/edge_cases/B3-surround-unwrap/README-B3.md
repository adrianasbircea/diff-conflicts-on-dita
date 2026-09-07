# B3 - Surround (text wrapped in inline markup)

**Priority:** must pass
**Files:** `B3-base.dita` -> `B3-modified.dita`
**Change:** *OK* and *Cancel* are wrapped in `<uicontrol>`. The plain text is identical.
**Fed to the AI:** two `<p>` fragments that read the same.
**Expected:** report that two button names were tagged as UI controls, with no wording change; the
suggestion is markup, not text.
**Fails if:** it reports a text change, or proposes the sentence without the new elements, which
would silently undo the tagging.
