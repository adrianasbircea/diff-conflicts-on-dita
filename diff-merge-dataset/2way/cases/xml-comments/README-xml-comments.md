# Plain XML comments (X-01, 2-way)

**Base:** `xml-comments-base.dita` -> **Modified:** `xml-comments-modified.dita`

Plain `<!-- -->` comments, in every position the DTD allows: between blocks, inside mixed content,
inside a list. These are **not** Oxygen review comments - they are document nodes, and they must
show up in the diff like any other node. No review markup in either file.

| ID | Location | Change | Expected |
| --- | --- | --- | --- |
| X-01a | before `sec-overview` | comment text modified (*release 4.1* -> *4.2*) | modified comment, one entry; not delete + insert |
| X-01b | before `p-intro` | comment added, next to an existing one | one inserted comment; the existing one untouched |
| X-01c | after `p-deleted-comment` | comment deleted | one deleted comment |
| X-01d | `p-arch` | comment inside mixed content deleted (*property renamed in 4.0*) | deletion inside the paragraph, text unchanged |
| X-01e | `p-arch` | comment added inside mixed content, between two words (*was: every*) | insertion inside the paragraph, text unchanged |
| X-01f | `p-moved-comment` / `p-deleted-comment` | *TODO: document the readiness endpoint* moved after the second paragraph | move, not delete + insert |
| X-01g | `p-commented-out` | the whole `<p>`, with its `<apiname>` child, is commented out | element deletion **plus** comment insertion; the markup inside the comment must not be parsed as elements |
| X-01h | `ul-prereq`, `li-prereq-1` | comments unchanged | no change reported inside the list |

## Global expectations

* Comment nodes are diffed on their own, with the same granularity as text: a reworded comment is
  one modification, not a delete plus an insert.
* A comment inside mixed content must not force the parent paragraph to be reported as rewritten.
* X-01g must not be reported as "text changed": the paragraph is gone and a comment appeared.
* Merging must never move a comment across an element boundary it did not cross in either input.

## Opening in Web Author

See the end of `../../README-2way.md`, replacing the last path segment with
`2way%2Fcases%2Fxml-comments%2Fxml-comments-modified.dita`.
