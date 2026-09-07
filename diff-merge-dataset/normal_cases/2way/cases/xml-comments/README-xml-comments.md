# Plain XML comments (X-01, 2-way)

**Base:** `xml-comments-base.dita` -> **Modified:** `xml-comments-modified.dita`

Plain `<!-- -->` comments are **not** Oxygen review comments - they are document nodes, and they
must show up in the diff like any other node. The base carries comments in several positions
(between blocks, inside mixed content, inside a list); all of them are identical in both files
except the one change below.

No review markup in either file.

## The change

| Location | Change |
| --- | --- |
| `p-commented-out` | the whole `<p>`, with its `<apiname>` child, is commented out |

## Expected

* One element deletion **plus** one comment insertion. Never "text changed": the paragraph is gone
  and a comment appeared in its place.
* The markup inside the comment must not be parsed as elements - no `<apiname>` may show up as a
  node in the modified tree.
* The comments the two files share must be reported nowhere, including the one inside `p-arch`'s
  mixed content and the ones in the list.
* Merging must never move a comment across an element boundary it did not cross in either input.

A comment whose text is reworded, with nothing else changing, is covered by
`../../../../edge_cases/B7-xml-comment` and is deliberately not repeated here.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`, replacing the last path segment with
`normal_cases%2F2way%2Fcases%2Fxml-comments%2Fxml-comments-modified.dita`.
