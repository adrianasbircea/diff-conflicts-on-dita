# H3 - The two sides are different languages

**Priority:** high value
**Files:** `H3-base.dita` -> `H3-modified.dita`
**Change:** the right-hand file is the German translation of the left-hand one. `@xml:lang` changes
from `en-us` to `de-de`. The meaning is the same.
**Fed to the AI:** an English fragment and a German fragment.
**Expected:** recognise a translation rather than an edit, say that the content is equivalent, and
refuse to "merge" the two into one paragraph. The right question for the user is which language this
file is supposed to hold.
**Fails if:** it treats the German text as a rewrite to accept, produces a mixed-language paragraph,
or reports a long list of word-level differences.
