# Edge-case sets for the AI diff explainer

One folder per case. Each set has two DITA files (`-base` / `-modified`), or three for the 3-way
cases (`-base` / `-user-a` / `-user-b`), plus a short README with the change, what the feature
receives, and what counts as a pass.

The feature under test receives **only the XML of the changed block on each side** - no diff type,
no descriptor list, no ancestor path, no document-level status. Several sets below are deliberately
built around information that is *not* in the fragment; there the pass criterion is a calibrated
answer that names the uncertainty, not a lucky guess. Those are marked **blind**.

Case IDs follow the review catalogue: A degenerate inputs, B changes with no visible text,
C cardinality mismatches, D whitespace, E review markup, G mode and document type, H language,
I adversarial. The numbering is not contiguous, because cases already covered by `../base` or
`../context` were dropped - see *Covered elsewhere* at the end.

| Set | Priority | Tests | Blind |
| --- | --- | --- | --- |
| `A1-identical-blocks` | must pass | identical fragments, must not invent a change | |
| `A4-no-text-content` | high value | block with no text at all (`<image/>`) | |
| `A6-root-element-changed` | high value | root renamed `topic` to `concept`, nothing else | |
| `A7-empty-document` | high value | whole document empty on one side | |
| `A9-missing-base` | must pass | 3-way requested, ancestor unavailable | blind |
| `B3-surround-unwrap` | must pass | text wrapped in `<uicontrol>` | |
| `B7-xml-comment` | high value | XML comment, not a review comment | |
| `C2-paragraph-split` | high value | one `<p>` becomes two | |
| `C7-many-children-one-block` | high value | 26 changes inside one `<codeblock>` | |
| `D3-codeblock-whitespace` | must pass | whitespace significant, pairs with context C10 | |
| `E2-comment-conflict-3way` | high value | two reviewers, same anchor, no text change | |
| `E5-pre-existing-tracked-changes` | high value | edit next to untouched `oxy_insert_*` PIs | |
| `G1-author-vs-text-mode` | high value | same edit, both diff pipelines | |
| `G2-not-well-formed` | must pass | fragments that do not parse | |
| `G3-unbound-xml` | high value | unknown vocabulary, no DITA semantics | |
| `G8-filtered-ancestor` | high value | `@props` on the ancestor, not in the fragment | blind |
| `H1-rtl-content` | high value | RTL text with embedded Latin markup | |
| `H3-different-languages` | high value | the two sides are a translation pair | |
| `H6-semantic-micro-edits` | must pass | one-token edits that invert meaning | |
| `I1-prompt-injection-in-text` | must pass | instructions hidden in document text | |
| `I2-prompt-injection-in-comment` | high value | instructions hidden in a comment PI | |
| `I3-secrets-in-change` | high value | a live-looking credential is introduced | |

`G3-unbound-xml` uses `.xml` rather than `.dita` on purpose: the case is a document with no
framework behind it.

Two sets hold documents that are not schema-valid, deliberately: `G2-not-well-formed` (does not
parse at all) and `A6-root-element-changed` (parses, but `<body>` is not allowed inside
`<concept>`). Everything else is valid DITA.

## Covered elsewhere

These cases were dropped from this folder because `../base` or `../context` already provides a
fixture for them. Use those instead.

| Dropped | Covered by | Note |
| --- | --- | --- |
| pure insertion | `base/2way` E-01 | inserted `<section>` |
| pure deletion | `base/2way` E-02 | deleted `<note>` |
| attribute-only change | `base/2way` AT-01, AT-06, AT-07 | value change on table, row, cell |
| element renamed, content identical | `base/2way` E-07 | `<ul>` becomes `<ol>`, same items |
| generated `@id` churn | `base/2way` AT-05 | `dt-slow` to `dt-slow-tokens` |
| attribute reordering, reflow | `context/C10-global-reformat` | whole-file reformat |
| table column added | `base/2way` TB-06 | `@cols` on the ancestor |
| list item inserted mid-list | `context/C6-list-renumbering`, `base/2way` L-01 | plus a stale numeric reference |
| content moved | `base/2way` E-03, E-04, `base/3way` CF-07 | deletion half of a move |
| conref / keyref indirection | `context/C9-conref-definition-changed` | definition changed elsewhere |
| review comment on one side only | `base/2way` C-01 to C-12 | base file has no review markup |
| content edit and comment in one block | `base/2way` `p-intro` | T-02 + AT-02 + C-01 + K-01 together |

Two gaps opened by those removals, if you want them back as minimal sets:

* **`&#160;` versus a literal no-break space.** `context/C10` covers attribute reordering and reflow
  but not entity-versus-character equivalence.
* **A reference whose target is outside the file set.** `context/C9` keeps the `conref` target in the
  same document; nothing tests a `keyref` or `conref` that cannot be resolved at all.
