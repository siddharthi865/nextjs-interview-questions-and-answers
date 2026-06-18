# Set 15

| #   | Question                                                                                                                                                                              |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [How do you implement edge functions for dynamic personalization?](#question-1-how-do-you-implement-edge-functions-for-dynamic-personalization)                                       |
| 2   | [How do you implement feature flags in a Next.js production app?](#question-2-how-do-you-implement-feature-flags-in-a-nextjs-production-app)                                          |
| 3   | [How do you implement multi-tenant architecture with server components?](#question-3-how-do-you-implement-multi-tenant-architecture-with-server-components)                           |
| 4   | [How do you optimize bundle size for large Next.js apps?](#question-4-how-do-you-optimize-bundle-size-for-large-nextjs-apps)                                                          |
| 5   | [How do you implement code splitting for large libraries?](#question-5-how-do-you-implement-code-splitting-for-large-libraries)                                                       |
| 6   | [How do you integrate Next.js with Redis caching?](#question-6-how-do-you-integrate-nextjs-with-redis-caching)                                                                        |
| 7   | [How do you implement a streaming blog with SSR + ISR?](#question-7-how-do-you-implement-a-streaming-blog-with-ssr--isr)                                                              |
| 8   | [How do you implement high-performance image galleries with next/image?](#question-8-how-do-you-implement-high-performance-image-galleries-with-nextimage)                            |
| 9   | [How do you debug memory leaks in SSR pages?](#question-9-how-do-you-debug-memory-leaks-in-ssr-pages)                                                                                 |
| 10  | [How do you implement server-side WebSockets in Next.js?](#question-10-how-do-you-implement-server-side-websockets-in-nextjs)                                                         |
| 11  | [How do you integrate third-party analytics at the edge?](#question-11-how-do-you-integrate-third-party-analytics-at-the-edge)                                                        |
| 12  | [How do you implement incremental builds for very large SSG sites?](#question-12-how-do-you-implement-incremental-builds-for-very-large-ssg-sites)                                    |
| 13  | [How do you implement SEO-friendly dynamic routes with nested content?](#question-13-how-do-you-implement-seo-friendly-dynamic-routes-with-nested-content)                            |
| 14  | [How do you handle multi-language pages with SSR?](#question-14-how-do-you-handle-multi-language-pages-with-ssr)                                                                      |
| 15  | [How do you implement AB testing on the edge?](#question-15-how-do-you-implement-ab-testing-on-the-edge)                                                                              |
| 16  | [How do you implement partial caching using stale-while-revalidate?](#question-16-how-do-you-implement-partial-caching-using-stale-while-revalidate)                                  |
| 17  | [How do you integrate a message queue system for background tasks?](#question-17-how-do-you-integrate-a-message-queue-system-for-background-tasks)                                    |
| 18  | [How do you implement a real-time dashboard with SSR + WebSockets?](#question-18-how-do-you-implement-a-real-time-dashboard-with-ssr--websockets)                                     |
| 19  | [How do you handle server-side logging and monitoring in Next.js?](#question-19-how-do-you-handle-server-side-logging-and-monitoring-in-nextjs)                                       |
| 20  | [What are the best practices for scaling Next.js applications for millions of users?](#question-20-what-are-the-best-practices-for-scaling-nextjs-applications-for-millions-of-users) |

## Question 1. How do you implement edge functions for dynamic personalization?

## **Q: How do you implement Edge Functions for dynamic personalization in Next.js?**

### **Short Answer (30 seconds):**

In **Next.js 16**, Edge Functions allow you to execute code close to the user using the **Edge Runtime**, enabling **low-latency dynamic personalization** based on cookies, headers, geolocation, or authentication without routing every request to the origin server. The most common approaches are **Middleware** for request routing and **Edge Route Handlers** for personalized responses.

> **Docs:**
>
> - App Router → Route Handlers
> - Edge Runtime
> - Middleware
> - [https://nextjs.org/docs/app/building-your-application/routing](https://nextjs.org/docs/app/building-your-application/routing)
> - [https://nextjs.org/docs/app/api-reference/edge](https://nextjs.org/docs/app/api-reference/edge)

---

# Detailed Explanation

## 1. What are Edge Functions?

Edge Functions run on distributed edge servers instead of a centralized Node.js server.

Instead of:

```
User
   ↓
Origin Server (India)
   ↓
Response
```

It becomes:

```
User
   ↓
Nearest Edge Location
   ↓
Personalized Response
```

This reduces latency dramatically for global applications.

Typical personalization includes:

- Authentication
- A/B testing
- Geo personalization
- Language detection
- Currency selection
- Feature flags
- Regional pricing
- Country-specific content
- Redirects

---

# 2. Where do Edge Functions run?

There are two primary options in Next.js:

### A. Middleware (most common)

Runs **before** a request reaches your application.

Perfect for:

- Redirects
- Authentication
- Locale detection
- Country routing
- Experiment assignment

Example:

```
Request
      ↓
Middleware (Edge)
      ↓
Modify request
      ↓
Page
```

---

### B. Edge Route Handlers

API endpoints executed on the Edge Runtime.

Example:

```ts
export const runtime = "edge";
```

Useful when:

- Personalized APIs
- Feature flag APIs
- User-specific recommendations

---

# 3. Example: Country-Based Personalization

Suppose users from:

- India → INR pricing
- US → USD pricing
- UK → GBP pricing

Using Middleware:

```ts
// middleware.ts

import { NextResponse } from "next/server";
import type { NextRequest } from "next/server";

export function middleware(request: NextRequest) {
  const country = request.headers.get("x-vercel-ip-country") ?? "US";

  const response = NextResponse.next();

  response.cookies.set("country", country);

  return response;
}
```

Then inside a Server Component:

```tsx
import { cookies } from "next/headers";

export default async function PricingPage() {
  const cookieStore = await cookies();
  const country = cookieStore.get("country")?.value;

  return <h1>Country: {country}</h1>;
}
```

The UI becomes personalized immediately.

---

# 4. Example: A/B Testing at the Edge

Middleware can randomly assign users to experiments.

```ts
import { NextRequest, NextResponse } from "next/server";

export function middleware(req: NextRequest) {
  const variant = Math.random() > 0.5 ? "A" : "B";

  const response = NextResponse.next();

  response.cookies.set("experiment", variant);

  return response;
}
```

Server Component:

```tsx
import { cookies } from "next/headers";

export default async function HomePage() {
  const cookieStore = await cookies();
  const variant = cookieStore.get("experiment")?.value;

  return variant === "A" ? <HeroV1 /> : <HeroV2 />;
}
```

This avoids client-side flicker because the decision is made before rendering.

---

# 5. Example: Personalized Edge API

```ts
// app/api/recommendations/route.ts

export const runtime = "edge";

import { NextRequest } from "next/server";

export async function GET(request: NextRequest) {
  const user = request.cookies.get("userId")?.value;

  return Response.json({
    recommendations: [`Products for ${user}`],
  });
}
```

The response is generated from the nearest edge location.

---

# 6. Authentication Personalization

Edge Middleware is commonly used to protect routes.

```ts
import { NextRequest, NextResponse } from "next/server";

export function middleware(request: NextRequest) {
  const token = request.cookies.get("token");

  if (!token) {
    return NextResponse.redirect(new URL("/login", request.url));
  }

  return NextResponse.next();
}
```

Users are redirected before the page loads.

---

# 7. Locale Personalization

Automatically detect language.

```ts
const language = request.headers.get("accept-language");

if (language?.startsWith("fr")) {
  return NextResponse.redirect(new URL("/fr", request.url));
}
```

No client-side detection is needed.

---

# 8. Feature Flags

Edge is ideal for gradual rollouts.

```ts
const enabledUsers = ["1001", "1002"];

if (enabledUsers.includes(userId)) {
  response.cookies.set("new-dashboard", "true");
}
```

Server Components can render the new UI conditionally.

---

# 9. Performance Considerations

### ✅ Personalize before rendering

Avoid:

```
SSR
↓
Hydration
↓
Client fetch
↓
Personalization
```

Prefer:

```
Edge
↓
Personalization
↓
Server Render
↓
Hydration
```

No layout shift or flicker.

---

### Keep Edge Functions lightweight

Good:

- Read cookies
- Read headers
- Geo lookup
- Redirect
- Feature flags

Avoid:

- Heavy database joins
- CPU-intensive processing
- Large dependencies
- Long-running tasks

---

### Use Server Components after Middleware

Middleware determines personalization.

Server Components render personalized HTML.

This combination minimizes client-side JavaScript while delivering tailored content.

---

# 10. Common Pitfalls & Solutions

### ❌ Using unsupported Node.js APIs

The Edge Runtime does **not** support all Node.js APIs (for example, modules like `fs` or many native Node libraries).

**Solution:** Use Web APIs (`fetch`, `Request`, `Response`, `crypto.subtle`) and libraries that explicitly support the Edge Runtime.

---

### ❌ Heavy database queries in Middleware

Middleware should stay fast.

**Solution:** Use Middleware for routing, authentication, locale detection, and lightweight decisions. Perform complex data fetching in Route Handlers or Server Components.

---

### ❌ Random A/B assignment on every request

Using `Math.random()` without persistence can cause users to switch variants between requests.

**Solution:** Assign the variant once and persist it in a cookie.

---

### ❌ Large bundles

Every imported package increases Edge startup time.

**Solution:** Keep dependencies minimal and import only what is required.

---

# Production Architecture

```
User
      │
      ▼
Edge Middleware
      │
      ├── Read cookies
      ├── Read headers
      ├── Detect country
      ├── Detect locale
      ├── Authenticate
      ├── Assign A/B test
      └── Set personalization cookies
               │
               ▼
Server Component
      │
      ├── Read cookies()
      ├── Fetch personalized data
      ├── Render HTML
      └── Stream UI
               │
               ▼
Browser
```

This architecture delivers personalized HTML on the first response while keeping latency low and reducing client-side work.

---

## **Code (Production-ready)**

### `middleware.ts`

```ts
import { NextRequest, NextResponse } from "next/server";

export function middleware(request: NextRequest) {
  const response = NextResponse.next();

  const country = request.headers.get("x-vercel-ip-country") ?? "US";
  const experiment =
    request.cookies.get("experiment")?.value ??
    (Math.random() > 0.5 ? "A" : "B");

  response.cookies.set("country", country);
  response.cookies.set("experiment", experiment);

  return response;
}

export const config = {
  matcher: ["/((?!api|_next/static|_next/image|favicon.ico).*)"],
};
```

### `app/page.tsx`

```tsx
import { cookies } from "next/headers";

export default async function HomePage() {
  const cookieStore = await cookies();

  const country = cookieStore.get("country")?.value;
  const experiment = cookieStore.get("experiment")?.value;

  return (
    <main>
      <h1>Country: {country}</h1>
      {experiment === "A" ? <HeroV1 /> : <HeroV2 />}
    </main>
  );
}
```

---

## **When to use**

- Global applications with low-latency personalization
- Authentication and authorization at the edge
- Country or region-specific pricing
- Internationalization (i18n)
- A/B testing and experimentation
- Feature flag rollouts
- Personalized marketing or landing pages
- Bot detection and request routing

---

## **Alternatives**

| Approach                             | Best For                           | Pros                                         | Cons                                                                               |
| ------------------------------------ | ---------------------------------- | -------------------------------------------- | ---------------------------------------------------------------------------------- |
| **Edge Middleware**                  | Redirects, auth, locale, A/B tests | Runs before rendering, very low latency      | Limited runtime APIs; keep logic lightweight                                       |
| **Edge Route Handlers**              | Personalized APIs                  | Fast, scalable, close to users               | Not suitable for long-running or compute-heavy tasks                               |
| **Server Components (Node runtime)** | Complex personalized pages         | Full server capabilities, direct data access | Higher latency for globally distributed users                                      |
| **Client Components**                | User interactions after load       | Rich interactivity                           | Personalization happens after hydration, which can cause loading states or flicker |

## Question 2. How do you implement feature flags in a Next.js production app?

## Question 3. How do you implement multi-tenant architecture with server components?

## Question 4. How do you optimize bundle size for large Next.js apps?

## Question 5. How do you implement code splitting for large libraries?

## Question 6. How do you integrate Next.js with Redis caching?

## Question 7. How do you implement a streaming blog with SSR + ISR?

## Question 8. How do you implement high-performance image galleries with next/image?

## Question 9. How do you debug memory leaks in SSR pages?

## Question 10. How do you implement server-side WebSockets in Next.js?

## Question 11. How do you integrate third-party analytics at the edge?

## Question 12. How do you implement incremental builds for very large SSG sites?

## Question 13. How do you implement SEO-friendly dynamic routes with nested content?

## Question 14. How do you handle multi-language pages with SSR?

## Question 15. How do you implement AB testing on the edge?

## Question 16. How do you implement partial caching using stale-while-revalidate?

## Question 17. How do you integrate a message queue system for background tasks?

## Question 18. How do you implement a real-time dashboard with SSR + WebSockets?

## Question 19. How do you handle server-side logging and monitoring in Next.js?

## Question 20. What are the best practices for scaling Next.js applications for millions of users?
