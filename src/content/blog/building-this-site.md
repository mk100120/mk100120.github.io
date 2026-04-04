---
title: "从零到部署：我如何搭建这个站点"
description: "记录这个个人站点从技术选型到上线的完整过程，包括踩过的坑和最终方案。"
pubDate: 2026-03-20
tags: ["tutorial", "astro"]
---

## 起因

一直想有一个属于自己的地方，记录学习笔记和技术思考。试过 Notion、语雀，但总觉得数据不在自己手上不踏实。

最终决定：**Markdown + Git + 静态站点**，数据永远在 GitHub 上。

## 技术选型

| 需求 | 方案 |
|------|------|
| 框架 | Astro 5.x |
| 样式 | 纯 CSS（玻璃拟态设计系统） |
| 内容 | Markdown + Content Collections |
| 部署 | GitHub Pages + Actions |

## 踩坑记录

### Content Collections 配置文件位置

Astro 5.x 把配置文件从 `src/content/config.ts` 移到了 `src/content.config.ts`，一字之差，debug 了半小时。

### `render()` API 变更

v5 中 `render` 需要从 `astro:content` 导入，不再是 `entry.render()`：

```typescript
import { render } from 'astro:content';
const { Content } = await render(entry);
```

## 最终效果

Push Markdown 文件到 GitHub，30 秒后站点自动更新。简单、可靠、零成本。
