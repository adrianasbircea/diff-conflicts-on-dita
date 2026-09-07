# 2-Way Isolated Cases

One directory per case, each with its own file pair and its own README. Unlike `../../../base/2way/2way-base.dita`
and `../../../base/2way/2way-modified.dita`, which pack every case into one document, these files are small and
carry a single theme, so a failure points straight at one behaviour.

Conventions are the ones from `../../../base/2way/README-2way.md`: DITA topic with a DOCTYPE, an `@id` on every
element worth referring to, Oxygen review PIs (`oxy_comment_start/_end`, `oxy_insert_start/_end`,
`oxy_delete`, `oxy_attributes`), and a README whose tables give *Location*, *Change* and *Expected*.
Element ids are reused from the master base wherever the case covers the same content, so ids like
`p-intro`, `li-prereq-2` or `table-limits` mean the same thing here.

| Case | Directory | Theme | Review markup |
| --- | --- | --- | --- |
| N-01 | `formatting-noise/` | Reformatting noise: indentation, re-wrap, attribute order, quote style, character references, self-closing tags - against one change that *is* real | none |
| X-01 | `xml-comments/` | Plain `<!-- -->` comments added, deleted, moved, inside mixed content, and a block commented out | none |
| TB-07 | `spanning-cells/` | Spanning cells: `namest`/`nameend` and `morerows` added, widened, removed, and edited - the grid stops matching the sibling count | none |

## Why these three

The master 2-way set covers text, elements, attributes, lists, tables, comments and tracked changes,
but its base uses no spanning cells, no XML comments, and it is formatted identically to the
modified file. Those are the three constructs a differ meets constantly in real documents and never
meets in this dataset.

## Running them

Each README ends with the Web Author URL for that case. The path pattern is
`samples%3A%2F%2Fsamples%2Fdiff-merge%2F2way%2Fcases%2F<case-dir>%2F<case>-modified.dita`.

For a quick check that nothing is broken before opening anything:

```
xmllint --noout --nonet 2way/cases/*/*.dita
```
