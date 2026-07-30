---
name: Research LanzaTech from its own content API
description: Pull LanzaTech's own account of itself - pages, blog posts, press coverage and published
  reports - from the public WordPress REST API on lanzatech.com, without scraping HTML.
api: openapi/lanzatech-wordpress-openapi.yml
operations:
  - listTypes
  - listPages
  - getPagesById
  - listPosts
  - listNews
  - listDownload
  - listSearch
---

# Research LanzaTech from its own content API

LanzaTech has no developer program, but `lanzatech.com` serves a fully public WordPress REST API at
`https://lanzatech.com/wp-json`. No key, no token, no signup. Use it instead of scraping the site.

## Before you start

- Base URL: `https://lanzatech.com/wp-json`
- No authentication for reads. Do not send an `Authorization` header.
- Always pass `_fields` to trim the payload. Full post objects are several kilobytes each.
- Read `X-WP-Total` and `X-WP-TotalPages` from the response headers to know how much is there.
- `per_page` maxes out at 100. Asking for more returns `rest_invalid_param` with a 400.

## Steps

1. **Orient yourself.** Call `listTypes` (`GET /wp/v2/types`) to see what content this site publishes.
   You will find the standard WordPress types plus five LanzaTech-specific ones: `news`, `employee`,
   `board-member`, `download` and `testimonial`.

2. **Read the company's own positioning.** Call `listPages`
   (`GET /wp/v2/pages?per_page=100&_fields=id,slug,title,link`) to get all 16 pages, then
   `getPagesById` (`GET /wp/v2/pages/{id}?_fields=title,content,link`) on the ones that matter - About,
   Biorefining, SAF, Chemicals and the project pages. `content.rendered` is HTML; strip tags before
   summarising.

3. **Pull the blog.** Call `listPosts`
   (`GET /wp/v2/posts?per_page=100&orderby=date&order=desc&_fields=id,date,slug,title,excerpt,link`).
   There are 107 posts across 2 pages at `per_page=100`. Follow the `Link` header `rel="next"` rather
   than incrementing `page` blindly.

4. **Pull third-party coverage.** Call `listNews`
   (`GET /wp/v2/news?per_page=100&orderby=date&order=desc&_fields=id,date,title,link,content`). These 35
   items are press coverage LanzaTech chose to surface, which is a different signal from its own blog -
   treat it as curated, not comprehensive.

5. **Get the primary documents.** Call `listDownload`
   (`GET /wp/v2/download?per_page=100&_fields=id,title,content,link`). This is where the life-cycle
   assessments and published reports live. The PDFs themselves are on `lanzatech.com/wp-content/uploads/`
   and are linked from the rendered content.

6. **Search across everything.** Call `listSearch`
   (`GET /wp/v2/search?search={term}&per_page=100&_fields=id,title,url,type,subtype`) when you have a
   specific term - it spans 182 objects across posts and taxonomy terms in one call.

## Filtering the blog

`listPosts` accepts the full WordPress filter set:

- `search={term}` for full-text
- `after=2024-01-01T00:00:00` / `before=...` for ISO 8601 date windows
- `categories={id}` / `tags={id}` using ids from `listCategories` and `listTags` (4 categories, 141 tags)
- `orderby` accepts `author, date, id, include, modified, parent, relevance, slug, title` - anything else
  returns `rest_invalid_param`

## Error handling

Errors come back as the WordPress envelope, not RFC 9457 problem details:

```json
{"code":"rest_post_invalid_id","message":"Invalid post ID.","data":{"status":404}}
```

A Sucuri WAF sits in front of the site. It can return an HTML error page with no JSON body at all -
`/wp/v2/users` and unauthenticated writes both do this. **Check `Content-Type` before parsing**, and
treat a non-JSON body as an infrastructure rejection rather than an API error.

The full code list is in `errors/lanzatech-problem-types.yml`.

## What you will not get

`/wp/v2/users` is blocked at the WAF, so author names do not resolve - posts carry only a numeric
`author` id. `/wp/v2/comments` is registered but empty. There is no rate-limit header, so back off
politely on your own schedule.
