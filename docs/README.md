# Clay public docs

Public, markdown-only mirror of selected [Clay University](https://university.clay.com/docs) documentation pages.

Each file in `docs/` has frontmatter linking back to its source page:

```yaml
---
title: <page title>
source_url: <university.clay.com URL>
---
```

Updates to these docs are managed by an agent that opens PRs against this repo in response to tickets, Slack mentions, and similar intake signals.

## `incorrect_docs/`

`docs/incorrect_docs/` contains test copies of 21 pages with 1-2 deliberately
injected factual errors each (wrong numbers, limits, dates, or reversed logic
claims). They exist to test whether Fin surfaces inaccurate answers when its
knowledge source includes bad docs. These replace the corresponding topics for
this repo's published site — the accurate versions of those specific topics
are not mirrored elsewhere in `docs/`. Other pages in `docs/` may link to a
topic under test; those links point into `incorrect_docs/` so the published
site has no dead links.
