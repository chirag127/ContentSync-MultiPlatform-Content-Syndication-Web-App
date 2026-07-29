# ContentSync — Multi-Platform Content Syndication

[![Live](https://img.shields.io/badge/live-oriz.in-blue?style=flat-square)](https://contentsync-multiplatform-content-syndication-web-app.oriz.in)
[![Stars](https://img.shields.io/github/stars/chirag127/ContentSync-MultiPlatform-Content-Syndication-Web-App?style=flat-square)](https://github.com/chirag127/ContentSync-MultiPlatform-Content-Syndication-Web-App)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](https://github.com/chirag127/ContentSync-MultiPlatform-Content-Syndication-Web-App/blob/main/LICENSE)

Write Markdown once, publish everywhere. A TypeScript CLI that syndicates a folder of Markdown posts to 11 blogging platforms and generates a static website from the same source.

**Live:** https://contentsync-multiplatform-content-syndication-web-app.oriz.in

## What it does

- One source of truth: `content/posts/*.md` with YAML front-matter.
- Publishes each post to every configured platform via pluggable adapters.
- Idempotent: tracks per-post, per-platform state so re-runs update instead of duplicate.
- Builds a static HTML site from the same Markdown.
- `--dry-run` and `--mock` modes for safe local testing.

## Supported platforms

Dev.to, Hashnode, Medium, WordPress, Blogger, Tumblr, Wix, Write.as, Telegraph, Micro.blog, Substack.

Each lives in `src/adapters/`; add a platform by dropping a new adapter and registering it in `src/adapters/index.ts`.

## Setup

```bash
git clone https://github.com/chirag127/ContentSync-MultiPlatform-Content-Syndication-Web-App.git
cd ContentSync-MultiPlatform-Content-Syndication-Web-App
npm install
cp .env.example .env   # fill in the platform tokens you use
```

Only the tokens for platforms you actually target are required — adapters missing credentials are skipped.

## Usage

```bash
# Publish all posts to every configured platform
npm run publish

# Simulate without hitting any API
npm run publish -- --dry-run

# Run against the built-in mock server
npm test

# Generate the static site into ./public
npm run build-site
```

Options: `--dry-run`, `--mock`, `--concurrency <n>` (default 2).

## Content format

Each file in `content/posts/` is Markdown with front-matter:

```markdown
---
title: My Post Title
slug: my-post-title
tags: [tech, india]
---

Post body in Markdown.
```

State lives in `.postmap.json` (slug -> platform -> {id, url, lastUpdated}) so republishing updates existing posts rather than creating duplicates.

## Project layout

```
content/posts/     Markdown source posts
src/publish.ts     CLI entrypoint — publish orchestration
src/build-site.ts  static-site generator
src/adapters/      one file per platform
src/utils/         markdown parsing, state, logging
scripts/           content generation helpers
```

## License

MIT — see [LICENSE](LICENSE).
