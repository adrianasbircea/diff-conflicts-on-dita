# B3 - Surround (text wrapped in inline markup)

**Priority:** must pass
**Files:** `B3-base.dita` -> `B3-modified.dita`
**Change:** *OK* is wrapped in `<uicontrol>`. The plain text is identical, and *Cancel* is left
untagged on purpose, so the fragment holds one wrap and one control.
**Fed to the AI:** two `<p>` fragments that read the same.
**Expected:** report that one button name was tagged as a UI control, with no wording change; the
suggestion is markup, not text.
**Fails if:** it reports a text change, proposes the sentence without the new element - which would
silently undo the tagging - or claims *Cancel* was tagged too.
