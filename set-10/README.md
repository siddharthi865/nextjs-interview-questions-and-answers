# Set 10

| #   | Question                                                                                                                                                                   |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [How do you implement on-demand ISR revalidation?](#question-1-how-do-you-implement-on-demand-isr-revalidation)                                                            |
| 2   | [How do you handle large-scale image galleries in Next.js?](#question-2-how-do-you-handle-large-scale-image-galleries-in-nextjs)                                           |
| 3   | [How do you optimize performance for large lists in SSR pages?](#question-3-how-do-you-optimize-performance-for-large-lists-in-ssr-pages)                                  |
| 4   | [How do you integrate WebAssembly in Next.js?](#question-4-how-do-you-integrate-webassembly-in-nextjs)                                                                     |
| 5   | [How do you handle edge caching for SSR content?](#question-5-how-do-you-handle-edge-caching-for-ssr-content)                                                              |
| 6   | [How do you implement middleware at the edge?](#question-6-how-do-you-implement-middleware-at-the-edge)                                                                    |
| 7   | [How do you implement authentication at the edge?](#question-7-how-do-you-implement-authentication-at-the-edge)                                                            |
| 8   | [How do you implement feature flags in Next.js?](#question-8-how-do-you-implement-feature-flags-in-nextjs)                                                                 |
| 9   | [How do you handle multitenancy with Next.js?](#question-9-how-do-you-handle-multitenancy-with-nextjs)                                                                     |
| 10  | [How do you implement AB testing for SSR pages?](#question-10-how-do-you-implement-ab-testing-for-ssr-pages)                                                               |
| 11  | [How do you implement analytics tracking at the edge?](#question-11-how-do-you-implement-analytics-tracking-at-the-edge)                                                   |
| 12  | [How do you implement partial caching with stale-while-revalidate?](#question-12-how-do-you-implement-partial-caching-with-stale-while-revalidate)                         |
| 13  | [How do you integrate Next.js with micro-frontends architecture?](#question-13-how-do-you-integrate-nextjs-with-micro-frontends-architecture)                              |
| 14  | [How do you handle server-side image generation?](#question-14-how-do-you-handle-server-side-image-generation)                                                             |
| 15  | [How do you implement hybrid SSR/SSG pages in one app?](#question-15-how-do-you-implement-hybrid-ssrssg-pages-in-one-app)                                                  |
| 16  | [How do you integrate Next.js with a message queue system?](#question-16-how-do-you-integrate-nextjs-with-a-message-queue-system)                                          |
| 17  | [How do you handle background jobs from API routes?](#question-17-how-do-you-handle-background-jobs-from-api-routes)                                                       |
| 18  | [How do you implement custom authentication strategies with server components?](#question-18-how-do-you-implement-custom-authentication-strategies-with-server-components) |
| 19  | [How do you debug server components in Next.js 13+?](#question-19-how-do-you-debug-server-components-in-nextjs-13)                                                         |
| 20  | [How do you scale a large Next.js application for high traffic?](#question-20-how-do-you-scale-a-large-nextjs-application-for-high-traffic)                                |

## Question 1. How do you implement on-demand ISR revalidation?

## **Q: How do you implement on-demand ISR revalidation?**

### **Short Answer (30 seconds):**

On-demand ISR (Incremental Static Regeneration) allows you to **invalidate cached pages or data immediately** when content changes, instead of waiting for the `revalidate` interval. In **Next.js 16 App Router**, this is implemented using **`revalidatePath()`** or **`revalidateTag()`**, typically inside **Server Actions** or **Route Handlers** after a CMS update, webhook, or admin action.

---

# **Detailed Explanation**

## 1. What is On-Demand ISR?

Normally, ISR works like this:

```tsx
export const revalidate = 3600;
```

The page stays cached for one hour.

Problem:

Suppose an editor publishes a new blog after 5 minutes.

Without on-demand revalidation:

- Users continue seeing stale content.
- They wait up to one hour.

On-demand ISR solves this by **clearing the cache immediately** whenever content changes.

Instead of waiting:

```
CMS Update
      ↓
Webhook
      ↓
Next.js
      ↓
Invalidate Cache
      ↓
Next Request
      ↓
Fresh HTML
```

This gives you:

- Static performance
- Instant updates
- No rebuilds

---

## 2. Two Types of Revalidation

### A. Path-based

Revalidate a specific route.

```tsx
revalidatePath("/blog");
```

Useful for:

- Product page
- Blog page
- Dashboard page

---

### B. Tag-based

Revalidate all cached fetches sharing a tag.

```tsx
revalidateTag("posts");
```

Useful when multiple pages depend on the same data.

Example:

```
Homepage
Blog
Sidebar
Latest Posts Widget
```

All consume:

```
posts
```

One call:

```tsx
revalidateTag("posts");
```

refreshes everything.

---

# 3. Production Example using Route Handler

Imagine a CMS sends a webhook whenever a blog post changes.

```
CMS
 ↓
POST /api/revalidate
 ↓
Next.js clears cache
 ↓
Visitors receive updated page
```

### app/api/revalidate/route.ts

```tsx
import { revalidatePath, revalidateTag } from "next/cache";
import { NextResponse } from "next/server";

export async function POST(req: Request) {
  const { secret } = await req.json();

  if (secret !== process.env.REVALIDATE_SECRET) {
    return NextResponse.json({ message: "Unauthorized" }, { status: 401 });
  }

  revalidatePath("/blog");
  revalidateTag("posts");

  return NextResponse.json({
    revalidated: true,
  });
}
```

Whenever your CMS updates content:

```
CMS
↓
Webhook
↓
POST /api/revalidate
↓
Cache cleared
↓
Next request regenerates page
```

---

# 4. Using Tagged Fetches

Tag your data fetches.

```tsx
async function getPosts() {
  const res = await fetch("https://api.example.com/posts", {
    next: {
      tags: ["posts"],
    },
  });

  return res.json();
}
```

Later:

```tsx
revalidateTag("posts");
```

Every fetch using:

```
posts
```

is refreshed.

This is much cleaner than invalidating many individual pages.

---

# 5. Using Server Actions

An admin updates content.

```tsx
"use server";

import { revalidatePath } from "next/cache";

export async function updatePost(data: FormData) {
  // Save to database

  revalidatePath("/blog");
}
```

User flow:

```
Admin edits
      ↓
Database updated
      ↓
revalidatePath()
      ↓
Cache cleared
      ↓
Visitors receive latest content
```

No webhook is needed because the mutation happens within the app.

---

# 6. Path vs Tag

| Feature         | `revalidatePath()` | `revalidateTag()`           |
| --------------- | ------------------ | --------------------------- |
| Revalidates     | Specific route     | Shared cached data          |
| Best for        | Individual pages   | Shared API responses        |
| Scope           | Narrow             | Broad                       |
| Recommended for | Page updates       | CMS/API-driven applications |

---

# 7. Performance Considerations

- **Prefer `revalidateTag()` for shared data** to avoid manually invalidating multiple pages.
- Revalidation **does not rebuild the entire application**—only affected cached content is regenerated on the next request.
- Use **webhooks** from your CMS (e.g., Contentful, Sanity, Strapi) so updates trigger cache invalidation automatically.
- Protect revalidation endpoints with a **secret token** or authentication to prevent unauthorized cache invalidation.
- Keep cache tags meaningful (e.g., `"posts"`, `"products"`, `"categories"`) to make selective invalidation easier.

---

# 8. Common Pitfalls & Solutions

### ❌ Forgetting to tag fetch requests

```tsx
fetch(url);
```

Then:

```tsx
revalidateTag("posts");
```

Nothing happens because the fetch wasn't tagged.

Always use:

```tsx
fetch(url, {
  next: {
    tags: ["posts"],
  },
});
```

---

### ❌ Using a public revalidation endpoint

Never expose:

```tsx
POST / api / revalidate;
```

without authentication.

Protect it using:

- Secret tokens
- Signed webhooks
- Authentication/authorization

---

### ❌ Revalidating too broadly

Avoid:

```tsx
revalidatePath("/");
```

when only a product page changed.

Instead:

```tsx
revalidatePath("/products/123");
```

or:

```tsx
revalidateTag("products");
```

---

### ❌ Expecting immediate regeneration

`revalidatePath()` and `revalidateTag()` **invalidate the cache**. The affected page or data is typically regenerated on the **next request**, not instantly in the background.

---

# **Code (Production-Ready Example)**

### Fetch with cache tag

```tsx
// app/blog/page.tsx
async function getPosts() {
  const res = await fetch("https://api.example.com/posts", {
    next: {
      tags: ["posts"],
    },
  });

  return res.json();
}

export default async function BlogPage() {
  const posts = await getPosts();

  return (
    <ul>
      {posts.map((post: { id: number; title: string }) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}
```

### Route Handler for webhook revalidation

```tsx
// app/api/revalidate/route.ts
import { revalidateTag } from "next/cache";
import { NextResponse } from "next/server";

export async function POST(req: Request) {
  const { secret } = await req.json();

  if (secret !== process.env.REVALIDATE_SECRET) {
    return NextResponse.json({ error: "Unauthorized" }, { status: 401 });
  }

  revalidateTag("posts");

  return NextResponse.json({ revalidated: true });
}
```

---

## **When to use**

Use on-demand ISR when:

- A CMS publishes or updates content.
- Product inventory or pricing changes.
- E-commerce catalogs need immediate updates.
- News or documentation sites require fresh content without full redeployments.
- Admin dashboards modify publicly cached content.

---

## **Alternatives**

| Approach                                                                   | Best For                                                                                   |
| -------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| **Time-based ISR (`export const revalidate`)**                             | Content that can tolerate periodic staleness.                                              |
| **`revalidatePath()`**                                                     | Refreshing specific routes after a mutation.                                               |
| **`revalidateTag()`**                                                      | Refreshing shared cached data across multiple pages (recommended for larger applications). |
| **Dynamic rendering (`cache: "no-store"` or `dynamic = "force-dynamic"`)** | Highly dynamic, user-specific, or real-time content where caching is undesirable.          |

### **Next.js 16 Documentation**

- **Caching and Revalidating:** [https://nextjs.org/docs/app/building-your-application/caching](https://nextjs.org/docs/app/building-your-application/caching)
- **`revalidatePath` API:** [https://nextjs.org/docs/app/api-reference/functions/revalidatePath](https://nextjs.org/docs/app/api-reference/functions/revalidatePath)
- **`revalidateTag` API:** [https://nextjs.org/docs/app/api-reference/functions/revalidateTag](https://nextjs.org/docs/app/api-reference/functions/revalidateTag)
- **Route Handlers:** [https://nextjs.org/docs/app/building-your-application/routing/route-handlers](https://nextjs.org/docs/app/building-your-application/routing/route-handlers)
- **Server Actions:** [https://nextjs.org/docs/app/building-your-application/data-fetching/server-actions](https://nextjs.org/docs/app/building-your-application/data-fetching/server-actions)

> **Interview tip:** Mention that in modern Next.js (App Router), **`revalidateTag()` is often preferred for CMS-driven applications** because it decouples cache invalidation from specific routes. Multiple pages can share the same tagged data, making cache management more scalable than invalidating each path individually.

## Question 2. How do you handle large-scale image galleries in Next.js?

## Question 3. How do you optimize performance for large lists in SSR pages?

## Question 4. How do you integrate WebAssembly in Next.js?

## Question 5. How do you handle edge caching for SSR content?

## Question 6. How do you implement middleware at the edge?

## Question 7. How do you implement authentication at the edge?

## Question 8. How do you implement feature flags in Next.js?

## Question 9. How do you handle multitenancy with Next.js?

## Question 10. How do you implement AB testing for SSR pages?

## Question 11. How do you implement analytics tracking at the edge?

## Question 12. How do you implement partial caching with stale-while-revalidate?

## Question 13. How do you integrate Next.js with micro-frontends architecture?

## Question 14. How do you handle server-side image generation?

## Question 15. How do you implement hybrid SSR/SSG pages in one app?

## Question 16. How do you integrate Next.js with a message queue system?

## Question 17. How do you handle background jobs from API routes?

## Question 18. How do you implement custom authentication strategies with server components?

## Question 19. How do you debug server components in Next.js 13+?

## Question 20. How do you scale a large Next.js application for high traffic?
