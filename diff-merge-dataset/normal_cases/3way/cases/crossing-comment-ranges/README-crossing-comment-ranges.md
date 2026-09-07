# Crossing comment ranges (CF-23, 3-way)

**Base:** `crossing-comment-ranges-base.dita` -> **User A:** `crossing-comment-ranges-user-a.dita`, **User B:** `crossing-comment-ranges-user-b.dita`

A and B are derived directly and independently from the base. The base has no review markup, and
neither user changed the document text.

CF-22 puts two comments on the *same* range, which nests trivially. This case puts them on ranges
that **partially overlap**: A's range starts before B's and ends inside it. Neither range contains
the other, so there is no way to nest them.

Each input file is individually well formed - the problem only appears when both sets of PIs land in
one document. The main 3-way README asks globally for balanced PIs; this case is the one that
actually forces the issue.

## Cases

| ID | Location | User A range | User B range | Overlap |
| --- | --- | --- | --- | --- |
| CF-23a | `p-cross` | *Tokens are signed with the key* (words 1-6) | *the key configured in the properties file* (words 5-10) | words 5-6 shared, inside one text node |
| CF-23b | `p-boundary` | *the property auth.signing* - starts in the paragraph text, **ends inside `<codeph>`** | *signing.key in the* - starts inside `<codeph>`, **ends in the paragraph text** | *signing* shared; both ranges also cross the `<codeph>` boundary, in opposite directions |
| CF-23c | `ol-steps` | the whole `<li id="li-step-1">` | from mid `li-step-1` to mid `li-step-2` | crossing at block level: A's range ends where B's is still open |
| CF-23d | `p-nested` | the whole sentence | *every login attempt*, strictly inside it | **control**: properly nestable, must merge with no special handling |

## Expected

For CF-23a, CF-23b and CF-23c there is no result that keeps both ranges exactly as drawn. Any of
these is an acceptable resolution, as long as it is deliberate:

* nest by adjusting one boundary, and say which range was adjusted;
* emit two PI pairs that share one comment `@id`, so the range is split but the thread stays single;
* report a conflict and let the reviewer choose.

These are failures:

* PIs emitted in crossing order - `start-a`, `start-b`, `end-a`, `end-b` - which is not a legal
  Oxygen comment structure even though the XML still parses;
* an `oxy_comment_start` with no matching `oxy_comment_end`, or the reverse;
* a range silently reduced to zero characters;
* a comment dropped without a report;
* any change to the document text, including the `<codeph>` content in CF-23b.

## Global expectations

* Base, A and B have identical character content. Any reported text difference is a bug in how the
  PIs are being read.
* Eight comment threads exist across A and B; the merged file must account for all eight, even if
  some ranges were adjusted.
* CF-23d must merge with no adjustment and no conflict: it proves the tool is not simply refusing
  every pair of overlapping comments.
* Open the merged file in the Review Panel: every thread must highlight a non-empty range, and no
  thread may highlight text that neither reviewer selected.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`.
