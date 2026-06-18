# Set 14

| #   | Question                                                                                                                                                                     |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [How do you implement a loading skeleton for SSR pages?](#question-1-how-do-you-implement-a-loading-skeleton-for-ssr-pages)                                                  |
| 2   | [How do you integrate third-party authentication providers like Google/Facebook?](#question-2-how-do-you-integrate-third-party-authentication-providers-like-googlefacebook) |
| 3   | [How do you implement refresh tokens for authentication?](#question-3-how-do-you-implement-refresh-tokens-for-authentication)                                                |
| 4   | [How do you implement conditional layouts per page?](#question-4-how-do-you-implement-conditional-layouts-per-page)                                                          |
| 5   | [How do you optimize API requests for SSR and ISR?](#question-5-how-do-you-optimize-api-requests-for-ssr-and-isr)                                                            |
| 6   | [How do you handle edge cases in ISR when data updates frequently?](#question-6-how-do-you-handle-edge-cases-in-isr-when-data-updates-frequently)                            |
| 7   | [How do you implement dynamic meta tags based on route content?](#question-7-how-do-you-implement-dynamic-meta-tags-based-on-route-content)                                  |
| 8   | [How do you integrate Stripe payments in a Next.js app?](#question-8-how-do-you-integrate-stripe-payments-in-a-nextjs-app)                                                   |
| 9   | [How do you implement a multi-step form with SSR support?](#question-9-how-do-you-implement-a-multi-step-form-with-ssr-support)                                              |
| 10  | [How do you implement server-side validation for forms?](#question-10-how-do-you-implement-server-side-validation-for-forms)                                                 |
| 11  | [How do you implement a large-scale Next.js app with micro-frontends?](#question-11-how-do-you-implement-a-large-scale-nextjs-app-with-micro-frontends)                      |
| 12  | [How do you implement hybrid SSG + SSR in one app?](#question-12-how-do-you-implement-hybrid-ssg--ssr-in-one-app)                                                            |
| 13  | [How do you handle streaming SSR for large pages?](#question-13-how-do-you-handle-streaming-ssr-for-large-pages)                                                             |
| 14  | [How do you implement React Server Components in Next.js 13+?](#question-14-how-do-you-implement-react-server-components-in-nextjs-13)                                       |
| 15  | [How do you implement nested layouts using the app router?](#question-15-how-do-you-implement-nested-layouts-using-the-app-router)                                           |
| 16  | [How do you implement partial hydration for performance optimization?](#question-16-how-do-you-implement-partial-hydration-for-performance-optimization)                     |
| 17  | [How do you implement edge caching with Next.js and Vercel?](#question-17-how-do-you-implement-edge-caching-with-nextjs-and-vercel)                                          |
| 18  | [How do you handle authentication at the edge using middleware?](#question-18-how-do-you-handle-authentication-at-the-edge-using-middleware)                                 |
| 19  | [How do you implement on-demand ISR revalidation for high-traffic pages?](#question-19-how-do-you-implement-on-demand-isr-revalidation-for-high-traffic-pages)               |
| 20  | [How do you implement GraphQL subscriptions for real-time data?](#question-20-how-do-you-implement-graphql-subscriptions-for-real-time-data)                                 |

## Question 1. How do you implement a loading skeleton for SSR pages?

## **Q: How do you implement a loading skeleton for SSR pages?**

### **Short Answer (30 seconds):**

In **Next.js 16 App Router**, the recommended way to implement a loading skeleton for SSR pages is by using the **`loading.tsx`** convention. It automatically wraps the page in a React **Suspense** boundary and streams the loading UI immediately while the server fetches data. This provides much better perceived performance compared to showing a blank page.

---

# Detailed Explanation

## 1. Core Concept (Next.js 16)

Traditional SSR waits for all server-side data before sending HTML, causing users to stare at a blank page.

Next.js App Router solves this with **Streaming + Suspense**.

When you create:

```
app/
 ├── dashboard/
 │   ├── page.tsx
 │   ├── loading.tsx
```

Next.js automatically:

1. Starts rendering immediately.
2. Sends the HTML shell.
3. Displays `loading.tsx`.
4. Streams the real server-rendered content when data is ready.

No manual Suspense wrapper is required for route-level loading.

**Docs:**

- App Router → Loading UI & Streaming
- App Router → File Conventions → `loading.tsx`

---

# 2. Basic Folder Structure

```
app/
 ├── products/
 │   ├── page.tsx
 │   ├── loading.tsx
 │   └── error.tsx
```

---

## page.tsx

```tsx
// app/products/page.tsx

async function getProducts() {
  const res = await fetch("https://api.example.com/products", {
    cache: "no-store",
  });

  return res.json();
}

export default async function ProductsPage() {
  const products = await getProducts();

  return (
    <div>
      <h1>Products</h1>

      {products.map((product: any) => (
        <div key={product.id}>{product.name}</div>
      ))}
    </div>
  );
}
```

---

## loading.tsx

```tsx
// app/products/loading.tsx

export default function Loading() {
  return (
    <div className="space-y-4 animate-pulse">
      <div className="h-8 w-64 rounded bg-gray-300" />

      {[...Array(5)].map((_, i) => (
        <div key={i} className="h-20 rounded bg-gray-200" />
      ))}
    </div>
  );
}
```

While the server fetches data, users immediately see this skeleton.

---

# 3. Streaming Behavior

```
User Request
      │
      ▼
Server starts rendering
      │
      ▼
loading.tsx shown instantly
      │
      ▼
Server fetches data
      │
      ▼
HTML streamed progressively
      │
      ▼
Skeleton disappears automatically
```

This improves **perceived performance** even if the actual data-fetch time is unchanged.

---

# 4. Component-Level Skeletons with Suspense

For independent sections that load at different speeds, wrap them in **React Suspense** so they stream separately.

```tsx
import { Suspense } from "react";

export default function Dashboard() {
  return (
    <>
      <Header />

      <Suspense fallback={<StatsSkeleton />}>
        <Stats />
      </Suspense>

      <Suspense fallback={<OrdersSkeleton />}>
        <Orders />
      </Suspense>
    </>
  );
}
```

Benefits:

- Header appears immediately.
- Stats load independently.
- Orders stream independently.
- Users don't wait for the slowest request.

---

# 5. Example Skeleton Component

```tsx
export function ProductCardSkeleton() {
  return (
    <div className="animate-pulse rounded-lg border p-4">
      <div className="mb-4 h-40 rounded bg-gray-300" />

      <div className="mb-2 h-6 rounded bg-gray-300" />

      <div className="h-4 w-3/4 rounded bg-gray-200" />
    </div>
  );
}
```

Render multiple placeholders:

```tsx
export default function Loading() {
  return (
    <div className="grid grid-cols-3 gap-6">
      {Array.from({ length: 6 }).map((_, index) => (
        <ProductCardSkeleton key={index} />
      ))}
    </div>
  );
}
```

---

# 6. Performance Considerations

### ✅ Match the final layout

Skeletons should closely resemble the final UI to reduce layout shifts and improve visual stability.

### ✅ Keep skeletons lightweight

Avoid unnecessary JavaScript or heavy animations. Simple CSS animations such as `animate-pulse` are usually sufficient.

### ✅ Stream independent sections

Use multiple Suspense boundaries so slower components don't block faster ones.

### ✅ Fetch data in Server Components

Perform data fetching on the server to minimize client-side JavaScript and improve initial load performance.

### ✅ Combine with error handling

Add an `error.tsx` file so users see a graceful fallback if streamed content fails.

---

# 7. Common Pitfalls & Solutions

### ❌ Showing a spinner instead of a skeleton

Spinners provide little context. Skeletons better communicate the structure of the upcoming content and generally improve perceived performance.

---

### ❌ One large Suspense boundary

If the entire page is wrapped in a single boundary, one slow request delays all content.

**Better:** Split the page into multiple Suspense boundaries.

---

### ❌ Skeleton layout differs from final UI

Large differences can cause layout shifts when content appears.

**Solution:** Design the skeleton to closely mirror the final component dimensions.

---

### ❌ Using client-side loading for server-rendered pages

For App Router SSR, prefer `loading.tsx` and streaming rather than fetching on the client solely to display a loading state.

---

# Code

### `app/products/loading.tsx`

```tsx
export default function Loading() {
  return (
    <div className="space-y-6">
      <div className="h-10 w-60 animate-pulse rounded bg-gray-300" />

      <div className="grid grid-cols-3 gap-6">
        {Array.from({ length: 6 }).map((_, index) => (
          <div key={index} className="rounded-lg border p-4">
            <div className="mb-4 h-40 animate-pulse rounded bg-gray-300" />

            <div className="mb-2 h-6 animate-pulse rounded bg-gray-300" />

            <div className="h-4 w-3/4 animate-pulse rounded bg-gray-200" />
          </div>
        ))}
      </div>
    </div>
  );
}
```

---

# When to use

- Server-rendered pages with slow database or API requests.
- Product listings, dashboards, analytics pages, and user profiles.
- Any route where streaming can improve perceived performance.

---

# Alternatives

| Approach                        | Best For                                                                            |
| ------------------------------- | ----------------------------------------------------------------------------------- |
| **`loading.tsx` (Recommended)** | Route-level loading in the App Router with automatic streaming.                     |
| **React Suspense**              | Component-level streaming where different sections load independently.              |
| **Client-side loading state**   | User-triggered interactions or client-only data fetching after the initial render.  |
| **Spinner**                     | Very short operations or actions where displaying the layout provides little value. |

**Interview Tip:** Mention that in **Next.js 16**, `loading.tsx` is the idiomatic solution for route-level SSR loading. It leverages **React Server Components**, **Suspense**, and **streaming** to send a meaningful UI immediately while the server continues rendering, resulting in a faster and more polished user experience.

## Question 2. How do you integrate third-party authentication providers like Google/Facebook?

## Question 3. How do you implement refresh tokens for authentication?

## Question 4. How do you implement conditional layouts per page?

## Question 5. How do you optimize API requests for SSR and ISR?

## Question 6. How do you handle edge cases in ISR when data updates frequently?

## Question 7. How do you implement dynamic meta tags based on route content?

## Question 8. How do you integrate Stripe payments in a Next.js app?

## Question 9. How do you implement a multi-step form with SSR support?

## Question 10. How do you implement server-side validation for forms?

## Question 11. How do you implement a large-scale Next.js app with micro-frontends?

## Question 12. How do you implement hybrid SSG + SSR in one app?

## Question 13. How do you handle streaming SSR for large pages?

## Question 14. How do you implement React Server Components in Next.js 13+?

## Question 15. How do you implement nested layouts using the app router?

## Question 16. How do you implement partial hydration for performance optimization?

## Question 17. How do you implement edge caching with Next.js and Vercel?

## Question 18. How do you handle authentication at the edge using middleware?

## Question 19. How do you implement on-demand ISR revalidation for high-traffic pages?

## Question 20. How do you implement GraphQL subscriptions for real-time data?
