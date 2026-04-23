# PG03

Precinct website for PG03, built with [Hugo](https://gohugo.io/) and the [dot-org-hugo-theme](https://github.com/cncf/dot-org-hugo-theme).

## Local development

```bash
hugo server -D     # run a local dev server (shows drafts)
hugo               # build the site into ./public/
```

## Authoring news posts

News posts live in [content/en/news/](content/en/news/). Each post is a markdown file with YAML front matter at the top.

### Minimal post

```markdown
---
title: "Your post title"
description: "Short blurb used for SEO and list-view summaries."
date: 2026-04-23
draft: false
categories: ["county"]
---

Post body in Markdown.
```

Save as `content/en/news/your-slug.md`. The filename becomes the URL: `/news/your-slug/`.

### Front matter fields

| Field         | Required | Notes                                                                                |
| ------------- | -------- | ------------------------------------------------------------------------------------ |
| `title`       | yes      | Shown as the post heading and in list views.                                         |
| `description` | no       | Used in SEO meta tags and when the post has no explicit summary.                     |
| `date`        | yes      | ISO format (`YYYY-MM-DD`). Drives ordering in feeds. Future dates are hidden.        |
| `draft`       | no       | `true` to hide from production builds. Drafts show in `hugo server -D`.              |
| `categories`  | no       | List of category names. Drives which filtered feeds the post appears in.            |
| `pinned`      | no       | `true` floats the post to the top of every feed it appears in, with a "Pinned" pill. |
| `author`      | no       | A key from [data/authors.yaml](data/authors.yaml). Shown in the byline when set.     |

### Categories

Currently two categories are used: `county` and `state`. Tagging a post produces these feeds:

- `/news/` — every post, regardless of category
- `/categories/county/` — county news only
- `/categories/state/` — state news only

A post can belong to both (`categories: ["county", "state"]`); it will appear in all three feeds.

To add a new category, just use a new name in `categories: [...]`. Hugo will auto-generate `/categories/<name>/`. To expose it in the nav, add an entry under the `news` parent in [config/_default/languages.yaml](config/_default/languages.yaml).

### Pinning

Add `pinned: true` to float a post above chronological posts in every feed it appears in. Multiple pinned posts sort by date among themselves. **Remember to unpin when a pinned post is no longer timely** — pinned posts stay at the top indefinitely.

### Authors

Authors are defined in [data/authors.yaml](data/authors.yaml). Each entry has a lookup key (usually a short slug) and at minimum a `name`:

```yaml
sequoia:
  name: Sequoia Ploeg
  twitter: sequoiaploeg   # optional
  company: PG03           # optional
```

Reference it from a post with `author: sequoia`. Posts without `author:` render the byline with date only.

## Homepage "Latest News"

The homepage auto-renders up to 3 most recent posts via the `{{< latest_news count=3 >}}` shortcode in [content/en/_index.md](content/en/_index.md). No manual updates required — posting a new `.md` file is enough.
