---
title: "Building a Personal Site with Astro"
description: "How I built this glassmorphism-styled personal site using Astro 5.x, Content Collections, and deployed it to GitHub Pages."
pubDate: 2026-04-04
tags: ["astro", "frontend", "tutorial"]
---

## Why Astro?

Astro is a static site generator that ships **zero JavaScript** by default. It's perfect for content-heavy sites like blogs and documentation.

### Key Benefits

- **Content Collections** — Type-safe Markdown with Zod schemas
- **Island Architecture** — Interactive components only where needed
- **Fast builds** — Incremental builds with Vite under the hood
- **MDX support** — Use components inside Markdown

## Setting Up Content Collections

In Astro 5.x, content collections are defined in `src/content.config.ts`:

```typescript
import { defineCollection } from 'astro:content';
import { glob } from 'astro/loaders';
import { z } from 'astro/zod';

const docs = defineCollection({
  loader: glob({ base: './src/content/docs', pattern: '**/*.md' }),
  schema: z.object({
    title: z.string(),
    pubDate: z.coerce.date(),
    tags: z.array(z.string()).default([]),
  }),
});
```

## Deploying to GitHub Pages

Push your code, and GitHub Actions handles the rest. The `withastro/action` makes it trivial:

```yaml
- uses: withastro/action@v6
```

> The entire build-and-deploy pipeline runs in under 30 seconds for a typical content site.

## What's Next

- Add search functionality
- RSS feed generation
- View transition animations
