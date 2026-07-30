---
name: Map LanzaTech leadership and board
description: Retrieve LanzaTech's leadership team and board of directors as structured records from the
  custom content types on its public WordPress REST API.
api: openapi/lanzatech-wordpress-openapi.yml
operations:
  - listEmployee
  - getEmployeeById
  - listBoardMember
  - getBoardMemberById
  - listMedia
  - getMediaById
---

# Map LanzaTech leadership and board

LanzaTech models its own people as first-class WordPress content types, so leadership and governance are
queryable rather than scrape-only. Both collections are public and need no authentication.

## Steps

1. **List the board.** Call `listBoardMember`
   (`GET /wp/v2/board-member?per_page=100&_fields=id,slug,title,content,featured_media,link`). There are
   14 records. `title.rendered` is the person's name; `content.rendered` is their HTML bio.

2. **List the leadership team.** Call `listEmployee`
   (`GET /wp/v2/employee?per_page=100&_fields=id,slug,title,content,featured_media,link`). There are 4
   records. Note that this is a curated subset of the executive team, not an org chart - do not present
   it as complete.

3. **Fetch a single profile.** Use `getBoardMemberById` (`GET /wp/v2/board-member/{id}`) or
   `getEmployeeById` (`GET /wp/v2/employee/{id}`) when you need the full record for one person.

4. **Resolve headshots.** Each profile carries a `featured_media` integer. Either add `_embed` to the
   list call and read `_embedded['wp:featuredmedia'][0].source_url`, or call `getMediaById`
   (`GET /wp/v2/media/{id}?_fields=source_url,alt_text,media_details`) with that id. A `featured_media`
   of `0` means no image is set.

## Doing it in one request

`_embed` avoids the N+1:

```
GET /wp/v2/board-member?per_page=100&_embed&_fields=id,title,content,link,_links,_embedded
```

## Cautions

- **Currency.** These records are only as fresh as the last website edit. Check `modified` on each item
  before treating a directorship as current, and cross-check anything material against the SEC filings
  at `https://ir.lanzatech.com/` - the website is marketing collateral, the filings are the record.
- **Author fields do not resolve.** `/wp/v2/users` is blocked at the Sucuri WAF and returns HTML 403, so
  the `author` id on any record is a dead end.
- **Bios are HTML.** `content.rendered` contains markup and often inline styles from the page builder.
  Strip tags before quoting.
- **Bad ids 404 with the WordPress envelope**, not RFC 9457:
  `{"code":"rest_post_invalid_id","message":"Invalid post ID.","data":{"status":404}}`.

## Related

Company-level research flow: `skills/lanzatech-research-company.md`.
Entity graph and item counts: `data-model/lanzatech-data-model.yml`.
