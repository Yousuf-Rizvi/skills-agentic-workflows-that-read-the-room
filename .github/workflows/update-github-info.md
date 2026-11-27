---
name: update-github-info
description: Daily refresh of the GitHub Info website page from the GitHub Blog, Changelog, and Awesome Copilot workflows, proposed as a pull request for Mona to review.
on:
  schedule: daily
  workflow_dispatch:

permissions:
  contents: read

engine:
  id: copilot
  copilot-sdk: true

tools:
  edit:
  web-fetch:

network:
  allowed:
    - github.blog
    - github.com
    - awesome-copilot.github.com

safe-outputs:
  create-pull-request:
    title-prefix: "[github-info] "
    labels: [automation]
    draft: true
    allowed-files:
      - "site/content/github-info.md"
    if-no-changes: "warn"
  noop:
---

# Update GitHub Info

You keep the GitHub Info website page current with the latest GitHub news.

## Steps

1. Read `notes/mona-notes.md` and follow Mona's guidance when writing.
2. Use web-fetch to read https://github.blog/latest/.
3. Use web-fetch to read https://github.blog/changelog/.
4. Use web-fetch to read https://awesome-copilot.github.com/workflows/.
5. Read `site/content/github-info.md` (create it if missing) and compare it with what you fetched.
6. Update `site/content/github-info.md` with new, relevant items:
   - Keep summaries short and practical, focused on helping developers learn GitHub faster.
   - Cite the source (GitHub Blog, GitHub Changelog, or Awesome Copilot workflows) with a link for each item.
   - Do not duplicate items already on the page.
   - Only edit `site/content/github-info.md`.
7. Open a pull request with `create-pull-request` so Mona can review the changes before they go live. Summarize the added items and their sources in the PR body.

If none of the sources contain anything new compared to the page, do not open a pull request; call `noop` with a short explanation. If a source cannot be fetched and no meaningful update is possible, explain that via `noop` instead of guessing.
