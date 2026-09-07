# D3 - Whitespace inside a codeblock (whitespace is significant)

**Priority:** must pass
**Files:** `D3-base.dita` -> `D3-modified.dita`
**Change:** the Python sample in `d3-cb` is re-indented from 4 spaces to 2. No other change.
**Fed to the AI:** two `<codeblock xml:space="preserve">` fragments.
**Expected:** recognise that indentation is content here, because the block is a `<codeblock>` with
`xml:space="preserve"`, and that in Python it is syntactically load-bearing; report the re-indent as
a real change and say which style the left file should keep.
**Fails if:** it dismisses the change as formatting noise.

**Run it against `../context/C10-global-reformat`,** whose conflict block is reindented and
rewrapped with no content change. The raw difference looks the same in both; the correct answers are
opposite. A tool that passes only one of the two is pattern-matching on line breaks.

**Variant worth adding later:** move `xml:space="preserve"` to an ancestor element so it is no
longer visible in the fragment. The correct answer then is to flag the uncertainty.
