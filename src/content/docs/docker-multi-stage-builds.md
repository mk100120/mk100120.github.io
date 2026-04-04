---
title: "Docker Multi-Stage Builds Explained"
description: "Reduce Docker image sizes by 90% with multi-stage builds. A practical guide with Java and Node.js examples."
pubDate: 2026-02-20
tags: ["docker", "devops"]
---

## The Problem with Single-Stage Builds

A typical Java application Docker image can easily exceed **800MB** because it includes the entire JDK, build tools, and intermediate artifacts.

## Multi-Stage to the Rescue

Multi-stage builds let you use multiple `FROM` statements. Each stage can copy artifacts from previous stages:

```dockerfile
# Stage 1: Build
FROM eclipse-temurin:21-jdk AS builder
WORKDIR /app
COPY . .
RUN ./gradlew bootJar

# Stage 2: Runtime (only JRE needed)
FROM eclipse-temurin:21-jre
WORKDIR /app
COPY --from=builder /app/build/libs/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

**Result**: Image drops from ~800MB to ~250MB.

## Node.js Example

```dockerfile
FROM node:22-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:22-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
CMD ["node", "dist/index.js"]
```

## Best Practices

1. **Use specific base image tags** — avoid `latest`
2. **Copy dependency files first** — leverage Docker layer caching
3. **Use `.dockerignore`** — exclude `node_modules`, `.git`, etc.
4. **Run as non-root** — add `USER node` or `USER 1000`

> Multi-stage builds are the single most impactful optimization for Docker image sizes. There's no reason not to use them.
