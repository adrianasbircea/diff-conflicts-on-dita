# 2-Way Diff/Merge Test Dataset

**Base:** `2way-base.dita` -> **Modified:** `2way-modified.dita`

The base has no review markup; all comments and tracked changes are in the modified file. Review
markup uses the standard Oxygen PIs (`oxy_comment_start/_end` with `id`, `parentID`, `flag="done"`;
`oxy_insert_start/_end`; `oxy_delete`; `oxy_attributes`), so the Review Panel reads it as-is.

Case IDs (`T-` text, `E-` element, `AT-` attribute, `L-` list, `TB-` table, `C-` comment,
`K-` tracked change) are only labels for tests and bug reports; *Location* holds the real `@id`.

## 1. Text

| ID | Location | Change | Expected |
| --- | --- | --- | --- |
| T-01 | `shortdesc` | one word: *available* -> *supported* | word-level change |
| T-02 | `p-intro` | first sentence reworded | partial sentence change |
| T-03 | `p-arch` | second half of the paragraph rewritten | partial paragraph change |
| T-04 | `p-config-lead` | sentence added (*A hot reload ... is not supported.*) | text insertion |
| T-05 | `p-trouble-end` | text removed (*collect the audit log and*) | text deletion |

## 2. Elements

| ID | Location | Change | Expected |
| --- | --- | --- | --- |
| E-01 | `sec-migration` | new `<section>` after `sec-config` | one inserted block |
| E-02 | `note-config` | `<note>` deleted | one deleted block |
| E-03 | `note-warn` | `<note>` moved to the end of `sec-overview` | move (delete+insert acceptable) |
| E-04 | `p-legacy` | `<p>` moved to the end of `sec-trouble` | move |
| E-05 | `cb-config` | codeblock changed (ttl 3600 -> 7200, new property) | line-level change in the code |
| E-06 | `dd-slow` | `<dd>` rewritten | changed element |
| E-07 | `ul-dev-details` -> `ol-dev-details` | nested `<ul>` becomes `<ol>`, same items | nesting change, children unchanged |
| E-08 | `li-prod-2` | moved one level up, into `ul-profiles` | nesting change |

## 3. Attributes

| ID | Location | Change | Expected |
| --- | --- | --- | --- |
| AT-01 | `table-features` | `@frame` `all` -> `topbot` | value change |
| AT-02 | `p-intro` | `@outputclass="lead"` added | attribute insertion |
| AT-03 | `p-arch` | `@outputclass` removed | attribute deletion |
| AT-04 | `p-legacy` | `@rev` removed | attribute deletion on a moved element |
| AT-05 | `dt-slow` -> `dt-slow-tokens` | `@id` changed | not a delete + insert |
| AT-06 | `row-log` | `@rowsep="0"` added | change on the row, not the table |
| AT-07 | `e-sec-status` | `@align="center"` added | change on the cell |

## 4. Lists

| ID | Location | Change | Expected |
| --- | --- | --- | --- |
| L-01 | `li-prereq-5` | item added at the end of `ul-prereq` | inserted item |
| L-02 | `li-prereq-3` | item deleted | deleted item |
| L-03 | `li-prereq-2` | item modified (*revision 42* -> *47*) | word-level change |
| L-04 | `ol-steps` | `li-step-4` and `li-step-3` swapped | reorder, not 2 rewrites |

## 5. Tables

| ID | Location | Change | Expected |
| --- | --- | --- | --- |
| TB-01 | `table-features` / `row-metrics` | row added | inserted row |
| TB-02 | `table-limits` / `row-lim-burst` | row deleted | deleted row |
| TB-03 | `e-authz-desc` | cell content modified | only that cell changed |
| TB-04 | `e-auth-desc` | text added in a cell (*two hours*, *SSO logins*) | insertion inside the cell |
| TB-05 | `e-log-desc` | text deleted in a cell | deletion inside the cell |
| TB-06 | `table-endpoints` | `@cols` 3 -> 4, new `colspec`, new *Since* cell in 5 rows | added cells, rows not rewritten |

Attribute changes on the table, on a row and on a cell: AT-01, AT-06, AT-07.

## 6. Review comments

| ID | Comment | Anchored on | Case |
| --- | --- | --- | --- |
| C-01 | `cmt-p-intro` | `p-intro`, whole element | on a paragraph; region also holds a tracked deletion |
| C-02 | `cmt-p-arch-sel` | text fragment in `p-arch` | on a text selection, no tracked change inside |
| C-03 | `cmt-cb-config` | `cb-config` | on an element |
| C-04 | `cmt-title-config` | `title-config` | on a title |
| C-05 | `cmt-li-step-2` | `li-step-2` | on a list item |
| C-06 | `cmt-e-authz-status` | `e-authz-status`, whole cell | **on a table cell** |
| C-07 | `cmt-row-log` | all entries of `row-log` | **on a table row** |
| C-08 | `cmt-e-auth-desc-text` | *two hours* inside `e-auth-desc` | on text inside a cell |
| C-09 | `cmt-cfg-1`, `cmt-cfg-2` | two fragments of `p-config-lead` | adjacent comments |
| C-10 | `cmt-li-prereq-1` | `li-prereq-1` | thread with a reply (`parentID`) |
| C-11 | `cmt-note-warn` | `note-warn` | already resolved (`flag="done"`) |
| C-12 | `cmt-p-trouble-end` | `p-trouble-end` | region contains a tracked insertion |

C-06 and C-07 are on purpose two separate cases, in two different rows, so cell vs row anchoring can
be compared in the Review Panel.

## 7. Tracked changes

| ID | Location | Change | Accept | Reject |
| --- | --- | --- | --- | --- |
| K-01 | `p-intro` | text deletion | text stays out | text returns |
| K-02 | `li-prereq-4` | *256* deleted + *384* inserted | *384 bits* | *256 bits* |
| K-03 | `li-step-1` | insertion in a list item | text kept | text removed |
| K-04 | `p-features-lead` | element deletion (`oxy_delete`) | paragraph gone | paragraph restored in place |
| K-05 | `p-trouble-metrics` | element insertion | paragraph kept | paragraph removed |
| K-06 | `e-sec-desc` | insertion in a table cell | *mandatory TLS 1.3,* kept | cell back to base text |
| K-07 | `row-lim-clock` | insertion of a whole row | row kept | 3 body rows left |
| K-08 | `li-prod-1` | deletion in a list item | text stays out | text returns |
| K-09 | `table-limits` | attribute change `@colsep` 0 -> 1 (`oxy_attributes`) | `colsep="1"` | `colsep="0"` |
| K-10 | `p-trouble-end` | insertion inside a commented region | text kept, comment preserved | text removed, comment must survive |
| K-11 | `sec-config` | K-02 + K-03 in one section | Accept All on the section | Reject All on the section |

## Global expectations

* Accept all -> no `oxy_insert_*` / `oxy_delete` / `oxy_attributes` left; comments untouched.
* Reject all -> tracked parts return to the base wording; the plain edits (1-5) stay.
* Accept/Reject must never drop or re-anchor a comment.
* No case should collapse into "whole document changed".

## Opening in Web Author

Copy the folders under
`<install>/tomcat/work/Catalina/localhost/oxygen-xml-web-author/samples/diff-merge/` and open:

```
http://localhost:8080/oxygen-xml-web-author/app/oxygen.html?url=samples%3A%2F%2Fsamples%2Fdiff-merge%2F2way%2F2way-modified.dita
```

Verified on WA 28.1 with a scratch copy: the Review Panel lists 25 entries with the intended anchors,
`flag="done"` is honoured, and Reply / Mark as Done / Accept / Reject all behave as the tables say.
One gap to decide on: K-09, the tracked **attribute** change, is applied in the document but is not
listed as a Review Panel entry, unlike text and element changes.
