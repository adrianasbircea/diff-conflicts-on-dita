# H1 - Right-to-left content with embedded Latin markup

**Priority:** high value
**Files:** `H1-base.dita` -> `H1-modified.dita`
**Change:** the token lifetime in the Arabic paragraph changes from *ساعتين* (two hours) to
*ثلاثين دقيقة* (thirty minutes). The `<codeph>Retry-After</codeph>` is unchanged.
**Fed to the AI:** two RTL `<p>` fragments with a Latin inline element inside them.
**Expected:** identify the changed phrase correctly and leave the rest, including the `<codeph>`,
untouched in the suggestion. If the explanation is written in English it should still quote the
Arabic accurately.
**Fails if:** it misidentifies which span changed, reorders the bidi run, drops or relocates the
`<codeph>`, or mangles the Arabic in the suggested text.
