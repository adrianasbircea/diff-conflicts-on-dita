# 2-Way Isolated Cases

One directory per case, each with its own file pair and its own README. **Each pair contains exactly
one change**, so a failure points straight at one behaviour. Unlike
`../../../base/2way/2way-base.dita` and `../../../base/2way/2way-modified.dita`, which pack every
case into one document, these files are small and carry a single theme.

Conventions are the ones from `../../../base/2way/README-2way.md`: DITA topic with a DOCTYPE, an
`@id` on every element worth referring to, Oxygen review PIs where a case needs them, and a README
that states the change and what counts as a pass. Element ids are reused from the master base
wherever the case covers the same content, so ids like `p-intro` or `p-arch` mean the same thing
here.

| Case | Directory | The one change | Review markup |
| --- | --- | --- | --- |
| N-01 | `formatting-noise/` | the file is re-serialised by a different pretty-printer - indentation, wrap, attribute order, quote style, character references, self-closing tags - with no content change at all | none |
| X-01 | `xml-comments/` | a whole `<p>`, with its `<apiname>` child, is commented out | none |
| TB-07 | `spanning-cells/` | two cells merged into one horizontal span: `namest`/`nameend` added, the sibling cell gone | none |

## Why these three

The master 2-way set covers text, elements, attributes, lists, tables, comments and tracked changes,
but its base uses no spanning cells, no XML comments, and it is formatted identically to the
modified file. Those are the three constructs a differ meets constantly in real documents and never
meets in the master set.

## Running them

Each README ends with the Web Author URL for that case. The path pattern is
`samples%3A%2F%2Fsamples%2Fdiff-merge%2Fnormal_cases%2F2way%2Fcases%2F<case-dir>%2F<case>-modified.dita`.

For a quick check that nothing is broken before opening anything:

```
xmllint --noout --nonet normal_cases/2way/cases/*/*.dita
```
