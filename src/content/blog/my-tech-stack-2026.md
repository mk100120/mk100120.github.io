---
title: "2026 年我的技术栈选择"
description: "回顾过去一年的技术选型，聊聊为什么我最终选择了 Astro + Java 21 + PostgreSQL 这套组合。"
pubDate: 2026-04-01
tags: ["thoughts", "tech-stack"]
---

## 前端：Astro

从 Next.js 切换到 Astro 是今年最正确的决定之一。对于内容驱动的站点，Astro 的零 JS 默认策略让性能开箱即用。

Content Collections 的类型安全让我不再担心 frontmatter 格式出错，Zod schema 在编译时就帮你兜底。

## 后端：Java 21 + Virtual Threads

Virtual Threads 彻底改变了 Java 的并发模型。以前需要精心调优的线程池，现在一个 `Executors.newVirtualThreadPerTaskExecutor()` 就搞定了。

Spring Boot 3.2 一行配置即可启用，迁移成本几乎为零。

## 数据库：PostgreSQL

> "No one ever got fired for choosing PostgreSQL."

JSONB、全文搜索、Window Functions……PostgreSQL 几乎能覆盖所有场景，不需要再引入 MongoDB 或 Elasticsearch。

## 总结

技术选型的核心原则：**选无聊的技术**。经过时间验证的工具，才能让你把精力放在真正有价值的事情上。
