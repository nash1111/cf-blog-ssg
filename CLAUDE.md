# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Git ワークフロー（必須）

### ブランチ運用

mainブランチに直接コミットしない。必ず作業ブランチを作成する。

```bash
# 機能追加
git checkout -b feat/<feature-name>

# バグ修正
git checkout -b fix/<bug-name>

# ドキュメント
git checkout -b docs/<doc-name>

# リファクタリング
git checkout -b refactor/<target>
```

### Pull Request

`gh` コマンドでPRを作成する。直接pushではなくPR経由でマージする。

```bash
# 変更をpush
git push -u origin <branch-name>

# PRの作成
gh pr create --title "タイトル" --body "説明"

# PRの確認
gh pr view

# PRのマージ
gh pr merge
```

### コミットメッセージ規約

```
<type>: <subject>

# type:
# feat: 新機能
# fix: バグ修正
# docs: ドキュメント
# refactor: リファクタリング
# test: テスト
# chore: その他
```

## Project Overview

This is an Astro-based static blog deployed to Cloudflare Pages/Workers. It uses MDX for content, generates RSS feeds, and includes a sitemap.

## Commands

```bash
bun run dev        # Start dev server at localhost:4321
bun run build      # Build production site to ./dist/
bun run preview    # Build and run with wrangler dev (Cloudflare local)
bun run deploy     # Build and deploy to Cloudflare
bun run cf-typegen # Generate Cloudflare worker types
```

## Architecture

### Content System
- Blog posts are MDX files in `src/content/blog/`
- Content schema defined in `src/content.config.ts` with required fields: `title`, `description`, `pubDate`, and optional `updatedDate`, `heroImage`
- Images referenced in frontmatter use relative paths from `src/assets/` (e.g., `heroImage: '../../assets/8.jpg'`)
- Use `getCollection('blog')` to query posts

### Routing
- `src/pages/index.astro` - Home page
- `src/pages/blog/index.astro` - Blog listing
- `src/pages/blog/[...slug].astro` - Individual blog posts (dynamic route)
- `src/pages/rss.xml.js` - RSS feed generation
- `src/pages/about.astro` - About page

### Site Configuration
- Global constants (site title, description) in `src/consts.ts`
- Astro config uses `imageService: "passthrough"` for Cloudflare compatibility

### Cloudflare Deployment
- Adapter: `@astrojs/cloudflare` with `platformProxy` enabled
- Worker config in `wrangler.jsonc`
- Output goes to `./dist/` with worker at `./dist/_worker.js/index.js`

## Adding New Blog Posts

Create a new `.mdx` file in `src/content/blog/` with this frontmatter:

```mdx
---
title: Post Title
description: Brief description
pubDate: YYYY-MM-DD
heroImage: '../../assets/image.jpg'  # optional
---

Content here...
```
