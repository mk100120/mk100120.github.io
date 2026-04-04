---
title: "TypeScript Best Practices in 2026"
description: "A collection of TypeScript patterns and practices I follow for production codebases — strict types, discriminated unions, and more."
pubDate: 2026-03-15
tags: ["typescript", "best-practices"]
---

## Strict Mode is Non-Negotiable

Always enable strict mode in `tsconfig.json`. It catches a class of bugs that are otherwise invisible:

```json
{
  "compilerOptions": {
    "strict": true,
    "noUncheckedIndexedAccess": true
  }
}
```

## Prefer Discriminated Unions Over Enums

Discriminated unions provide better type narrowing and are more idiomatic:

```typescript
type Result<T> =
  | { status: 'success'; data: T }
  | { status: 'error'; message: string };

function handle(result: Result<User>) {
  if (result.status === 'success') {
    // TypeScript knows result.data is User here
    console.log(result.data.name);
  }
}
```

## Use `satisfies` for Type Validation

The `satisfies` operator validates types without widening:

```typescript
const config = {
  port: 3000,
  host: 'localhost',
} satisfies ServerConfig;
// config.port is number (not string | number)
```

## Branded Types for Domain Safety

Prevent accidental type mixing with branded types:

```typescript
type UserId = string & { readonly __brand: 'UserId' };
type OrderId = string & { readonly __brand: 'OrderId' };

function getUser(id: UserId): User { /* ... */ }

const orderId = 'abc' as OrderId;
// getUser(orderId); // Compile error!
```

## Key Takeaways

| Practice | Why |
|----------|-----|
| Strict mode | Catches null/undefined bugs |
| Discriminated unions | Better type narrowing |
| `satisfies` | Type-safe without widening |
| Branded types | Domain boundary safety |
