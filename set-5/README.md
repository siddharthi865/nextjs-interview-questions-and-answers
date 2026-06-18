# Set 5

| #   | Question                                                                                                                                                              |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [How can you deploy Next.js with Edge Runtime?](#question-1-how-can-you-deploy-nextjs-with-edge-runtime)                                                              |
| 2   | [How does Next.js optimize bundle size?](#question-2-how-does-nextjs-optimize-bundle-size)                                                                            |
| 3   | [What is the difference between middleware and edge functions?](#question-3-what-is-the-difference-between-middleware-and-edge-functions)                             |
| 4   | [How do you implement authentication in server components?](#question-4-how-do-you-implement-authentication-in-server-components)                                     |
| 5   | [How can you integrate GraphQL with Next.js?](#question-5-how-can-you-integrate-graphql-with-nextjs)                                                                  |
| 6   | [How do you optimize SEO in Next.js?](#question-6-how-do-you-optimize-seo-in-nextjs)                                                                                  |
| 7   | [How do you implement structured data (JSON-LD) in Next.js?](#question-7-how-do-you-implement-structured-data-json-ld-in-nextjs)                                      |
| 8   | [How can you do incremental static regeneration with on-demand revalidation?](#question-8-how-can-you-do-incremental-static-regeneration-with-on-demand-revalidation) |
| 9   | [How can you implement A/B testing in Next.js?](#question-9-how-can-you-implement-ab-testing-in-nextjs)                                                               |
| 10  | [How does Next.js handle streaming SSR with Suspense?](#question-10-how-does-nextjs-handle-streaming-ssr-with-suspense)                                               |
| 11  | [How do you do caching with stale-while-revalidate in Next.js?](#question-11-how-do-you-do-caching-with-stale-while-revalidate-in-nextjs)                             |
| 12  | [How does Next.js handle CDN integration?](#question-12-how-does-nextjs-handle-cdn-integration)                                                                       |
| 13  | [How do you implement micro-frontends with Next.js?](#question-13-how-do-you-implement-micro-frontends-with-nextjs)                                                   |
| 14  | [How do you implement analytics tracking in server components?](#question-14-how-do-you-implement-analytics-tracking-in-server-components)                            |
| 15  | [How do you implement a multi-tenant architecture in Next.js?](#question-15-how-do-you-implement-a-multi-tenant-architecture-in-nextjs)                               |
| 16  | [How can you optimize images with AVIF or WebP formats?](#question-16-how-can-you-optimize-images-with-avif-or-webp-formats)                                          |
| 17  | [How do you use custom webpack configuration in Next.js?](#question-17-how-do-you-use-custom-webpack-configuration-in-nextjs)                                         |
| 18  | [How do you implement role-based access control in Next.js?](#question-18-how-do-you-implement-role-based-access-control-in-nextjs)                                   |
| 19  | [How do you handle high concurrency SSR pages efficiently?](#question-19-how-do-you-handle-high-concurrency-ssr-pages-efficiently)                                    |
| 20  | [What are the best practices for scaling Next.js applications?](#question-20-what-are-the-best-practices-for-scaling-nextjs-applications)                             |

## Question 1. How can you deploy Next.js with Edge Runtime?

**Q: How can you deploy Next.js with Edge Runtime?**

**Short Answer (30 seconds):**
Next.js Edge Runtime allows you to run Server Components, Route Handlers, and Middleware closer to users at the edge for ultra-low latency. You enable it by setting `runtime = "edge"` in your route or middleware, and deploy on platforms that support edge execution like Vercel Edge Network or Cloudflare.

---

**Detailed Explanation:**

### 1. Core Concept with Docs Reference

Next.js Edge Runtime is a lightweight JavaScript runtime based on the Web APIs (not Node.js). It’s designed for fast, globally distributed execution.

- Docs: [https://nextjs.org/docs/app/building-your-application/rendering/edge-and-nodejs-runtimes](https://nextjs.org/docs/app/building-your-application/rendering/edge-and-nodejs-runtimes)
- Supported in:
  - Middleware
  - Route Handlers (`app/api/.../route.ts`)
  - Server Components (selectively, via runtime config)

Key difference:

- **Node.js Runtime** → full backend capabilities (fs, native modules)
- **Edge Runtime** → limited APIs, but extremely fast cold start and global distribution

---

### 2. Implementation Approach

To deploy Next.js with Edge Runtime:

#### A. Enable Edge in Route Handler

You explicitly opt into Edge runtime per file:

```ts
export const runtime = "edge";

export async function GET() {
  return Response.json({
    message: "Hello from Edge Runtime",
  });
}
```

---

#### B. Middleware at the Edge

Middleware always runs on Edge Runtime by default:

```ts
import { NextResponse } from "next/server";
import type { NextRequest } from "next/server";

export function middleware(request: NextRequest) {
  const url = request.nextUrl;

  // Example: simple auth gating
  const token = request.cookies.get("token");

  if (!token) {
    return NextResponse.redirect(new URL("/login", request.url));
  }

  return NextResponse.next();
}
```

---

#### C. Edge-Ready Server Component (optional)

You can force Edge runtime in a page/layout:

```ts
export const runtime = "edge";

export default async function Page() {
  const res = await fetch("https://api.example.com/data", {
    cache: "no-store",
  });

  const data = await res.json();

  return <pre>{JSON.stringify(data, null, 2)}</pre>;
}
```

---

### 3. Deployment Strategy

#### Vercel (Recommended)

Next.js Edge Runtime is first-class on Vercel:

- Deploy normally:

```bash
vercel
```

Vercel automatically:

- Deploys middleware to Edge Network
- Deploys edge routes to regional edge nodes
- Handles routing, caching, and streaming

---

#### Cloudflare (Alternative)

Use:

- `@cloudflare/next-on-pages`

Build:

```bash
npm run build
npx @cloudflare/next-on-pages
```

Deploy to Cloudflare Pages.

---

#### AWS / Other CDNs

You typically need:

- Adapter like OpenNext (`@opennextjs/cloudflare`, `@opennextjs/aws`)
- Edge functions mapped to CloudFront / Lambda@Edge

---

### 4. Performance Considerations

Edge Runtime improves:

- 🚀 Lower TTFB (closer to user)
- 🚀 Faster auth checks (middleware)
- 🚀 Better global distribution
- 🚀 Reduced cold starts vs serverless Node

But comes with constraints:

- No Node.js APIs (`fs`, `net`, `child_process`)
- Limited CPU execution time
- Smaller memory footprint
- Must rely on Web APIs (`fetch`, `Request`, `Response`, `URL`)

Best suited for:

- Auth middleware
- A/B testing
- Personalization
- Geo-based routing
- Lightweight API responses

---

### 5. Common Pitfalls & Solutions

#### ❌ Using Node APIs in Edge

```ts
import fs from "fs"; // ❌ not supported
```

✅ Solution: use external storage (S3, KV, DB APIs)

---

#### ❌ Heavy computation in Edge

Edge is not for CPU-heavy tasks.

✅ Move to:

- Node runtime server actions
- Background workers

---

#### ❌ Large bundles

Edge has size limits.

✅ Optimize:

- Tree-shake dependencies
- Avoid heavy libraries (like full lodash, moment)

---

#### ❌ Unexpected runtime mismatch

Mixing Node + Edge incorrectly:

```ts
export const runtime = "edge"; // but using Node-only library ❌
```

---

**Code:**

```ts
// app/api/user/route.ts
export const runtime = "edge";

export async function GET(req: Request) {
  const ip = req.headers.get("x-forwarded-for");

  return new Response(
    JSON.stringify({
      message: "Hello from Edge",
      ip,
    }),
    {
      headers: {
        "content-type": "application/json",
        "cache-control": "no-store",
      },
    },
  );
}
```

---

**When to use:**

- Authentication middleware at scale
- Personalized content per region
- Global APIs with low latency needs
- A/B testing and feature flags at edge
- Lightweight JSON APIs

---

**Alternatives:**

- **Node.js Runtime (default)** → full backend features, best for DB-heavy logic
- **Server Actions (Node)** → mutations, form handling, business logic
- **Edge Runtime** → ultra-fast, lightweight, globally distributed logic
- **Hybrid approach (recommended)** → Edge for routing/auth + Node for heavy compute

## Question 2. How does Next.js optimize bundle size?

## Question 3. What is the difference between middleware and edge functions?

## Question 4. How do you implement authentication in server components?

## Question 5. How can you integrate GraphQL with Next.js?

## Question 6. How do you optimize SEO in Next.js?

## Question 7. How do you implement structured data (JSON-LD) in Next.js?

## Question 8. How can you do incremental static regeneration with on-demand revalidation?

## Question 9. How can you implement A/B testing in Next.js?

## Question 10. How does Next.js handle streaming SSR with Suspense?

## Question 11. How do you do caching with stale-while-revalidate in Next.js?

## Question 12. How does Next.js handle CDN integration?

## Question 13. How do you implement micro-frontends with Next.js?

## Question 14. How do you implement analytics tracking in server components?

## Question 15. How do you implement a multi-tenant architecture in Next.js?

## Question 16. How can you optimize images with AVIF or WebP formats?

## Question 17. How do you use custom webpack configuration in Next.js?

## Question 18. How do you implement role-based access control in Next.js?

## Question 19. How do you handle high concurrency SSR pages efficiently?

## Question 20. What are the best practices for scaling Next.js applications?
