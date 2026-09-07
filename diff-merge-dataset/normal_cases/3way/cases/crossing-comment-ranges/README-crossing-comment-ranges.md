# Crossing comment ranges (CF-23, 3-way)

**Base:** `crossing-comment-ranges-base.dita` -> **User A:** `crossing-comment-ranges-user-a.dita`, **User B:** `crossing-comment-ranges-user-b.dita`

A and B are derived directly and independently from the base. The base has no review markup, and
neither user changed the document text.

CF-22 puts two comments on the *same* range, which nests trivially. This case puts them on ranges
that **partially overlap**: A's range starts before B's and ends inside it. Neither range contains
the other, so there is no way to nest them.

Each input file is individually well formed - the problem only appears when both sets of PIs land in
one document.

## The change

| Location | User A range | User B range | Overlap |
| --- | --- | --- | --- |
| `p-cross` | *Tokens are signed with the key* (words 1-6) | *the key configured in the properties file* (words 5-10) | words 5-6, inside one text node |

## Expected

There is no result that keeps both ranges exactly as drawn. Any of these is acceptable, as long as
it is deliberate:

* nest by adjusting one boundary, and say which range was adjusted;
* emit two PI pairs that share one comment `@id`, so the range is split but the thread stays single;
* report a conflict and let the reviewer choose.

These are failures:

* PIs emitted in crossing order - `start-a`, `start-b`, `end-a`, `end-b` - which is not a legal
  Oxygen comment structure even though the XML still parses;
* an `oxy_comment_start` with no matching `oxy_comment_end`, or the reverse;
* a range silently reduced to zero characters;
* a comment dropped without a report;
* any change to the document text.

Base, A and B have identical character content: any reported text difference is a bug in how the PIs
are being read. `p-boundary`, `ol-steps` and `p-nested` are untouched by both and must be reported
nowhere.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`.
