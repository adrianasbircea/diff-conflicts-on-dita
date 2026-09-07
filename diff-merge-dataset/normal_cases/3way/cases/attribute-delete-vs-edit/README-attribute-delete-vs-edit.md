# Attribute deleted against attribute modified (CF-17, 3-way)

**Base:** `attribute-delete-vs-edit-base.dita` -> **User A:** `attribute-delete-vs-edit-user-a.dita`, **User B:** `attribute-delete-vs-edit-user-b.dita`

A and B are derived directly and independently from the base. Neither file has review markup.

CF-04 in the main set is modify-vs-modify on one attribute. This case is the other half: one user
**removes** the attribute, the other **changes its value**.

Element content is byte-identical in all three files. Every reported difference must be an attribute
difference.

## The change

| Location | Attribute | Base | User A | User B |
| --- | --- | --- | --- | --- |
| `p-arch` | `@outputclass` | `architecture` | removed | `architecture-overview` |

## Expected

* **One conflict**, naming a single attribute on a single element.
* The paragraph text merges silently: no conflict may be reported on text, on `@id`, or on any
  other attribute of `p-arch`.
* A tool that conflicts on the whole element, or on the whole document, fails this case.
* Every other element is identical in all three versions and must be reported nowhere.

## Opening in Web Author

See the end of `../../../../base/2way/README-2way.md`.
