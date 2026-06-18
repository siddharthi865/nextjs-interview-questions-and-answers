# Set 4

| #   | Question                                                                                                                                                   |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [How can you optimize web performance in Next.js?](#question-1-how-can-you-optimize-web-performance-in-nextjs)                                             |
| 2   | [How do you handle analytics in Next.js?](#question-2-how-do-you-handle-analytics-in-nextjs)                                                               |
| 3   | [How do you integrate Redux or Zustand in Next.js?](#question-3-how-do-you-integrate-redux-or-zustand-in-nextjs)                                           |
| 4   | [How do you implement server-side API calls with Axios or Fetch?](#question-4-how-do-you-implement-server-side-api-calls-with-axios-or-fetch)              |
| 5   | [How can you protect server-side rendered pages?](#question-5-how-can-you-protect-server-side-rendered-pages)                                              |
| 6   | [What is the role of next.config.js?](#question-6-what-is-the-role-of-nextconfigjs)                                                                        |
| 7   | [How can you configure redirects and rewrites in next.config.js?](#question-7-how-can-you-configure-redirects-and-rewrites-in-nextconfigjs)                |
| 8   | [How do you handle image domains in next.config.js?](#question-8-how-do-you-handle-image-domains-in-nextconfigjs)                                          |
| 9   | [How do you add custom headers in Next.js?](#question-9-how-do-you-add-custom-headers-in-nextjs)                                                           |
| 10  | [How do you enable internationalization (i18n) in Next.js?](#question-10-how-do-you-enable-internationalization-i18n-in-nextjs)                            |
| 11  | [Explain the Next.js app router (app/ directory) vs pages router (pages/)](#question-11-explain-the-nextjs-app-router-app-directory-vs-pages-router-pages) |
| 12  | [How does server components work in Next.js 13+?](#question-12-how-does-server-components-work-in-nextjs-13)                                               |
| 13  | [What are the advantages of React server components in Next.js?](#question-13-what-are-the-advantages-of-react-server-components-in-nextjs)                |
| 14  | [How does Next.js 13 handle streaming?](#question-14-how-does-nextjs-13-handle-streaming)                                                                  |
| 15  | [How do you implement layouts in the new app router?](#question-15-how-do-you-implement-layouts-in-the-new-app-router)                                     |
| 16  | [How do nested layouts work in Next.js 13+?](#question-16-how-do-nested-layouts-work-in-nextjs-13)                                                         |
| 17  | [How do you handle caching in server components?](#question-17-how-do-you-handle-caching-in-server-components)                                             |
| 18  | [What is the difference between server and client components?](#question-18-what-is-the-difference-between-server-and-client-components)                   |
| 19  | [How do you share state between server and client components?](#question-19-how-do-you-share-state-between-server-and-client-components)                   |
| 20  | [How does Next.js handle edge functions?](#question-20-how-does-nextjs-handle-edge-functions)                                                              |

## Question 1. How can you optimize web performance in Next.js?

**Q: How can you optimize web performance in Next.js?**

**Short Answer (30 seconds):**
Next.js performance optimization revolves around reducing JavaScript shipped to the client, leveraging Server Components by default, using intelligent caching (fetch + ISR), optimizing assets (images/fonts), and enabling streaming + edge rendering. The framework is designed to make “fast by default,” but real production performance comes from architectural decisions.

---

**Detailed Explanation:**

### 1. Core concept: Reduce Client-Side JavaScript (App Router + RSC)

Next.js 16 App Router heavily emphasizes **React Server Components (RSC)**.

- Server Components run on the server → zero client JS
- Only interactive parts become Client Components
- Docs: [https://nextjs.org/docs/app/building-your-application/rendering/server-components](https://nextjs.org/docs/app/building-your-application/rendering/server-components)

This reduces:

- hydration cost
- bundle size
- Time to Interactive (TTI)

---

### 2. Data fetching + caching strategy (critical for performance)

Next.js extends `fetch` with caching, revalidation, and ISR-like behavior.

- `force-cache` (default in RSC)
- `no-store` (dynamic, real-time)
- `revalidate` (ISR-like behavior)

**Example (cached + revalidated fetch):**

```tsx
// app/products/page.tsx (Server Component)

export default async function ProductsPage() {
  const res = await fetch("https://api.example.com/products", {
    next: { revalidate: 60 }, // ISR every 60 seconds
  });

  const products = await res.json();

  return (
    <div>
      {products.map((p: any) => (
        <div key={p.id}>{p.name}</div>
      ))}
    </div>
  );
}
```

**Why it matters:**

- avoids repeated API calls
- enables edge caching
- improves TTFB significantly

Docs: [https://nextjs.org/docs/app/building-your-application/data-fetching/fetching](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching)

---

### 3. Image & asset optimization (often biggest win)

Use `next/image` for:

- automatic resizing
- lazy loading
- modern formats (WebP/AVIF)
- responsive delivery

```tsx
import Image from "next/image";

export default function Hero() {
  return (
    <Image src="/hero.jpg" alt="Hero" width={1200} height={600} priority />
  );
}
```

**Performance impact:**

- reduces CLS (layout shift)
- improves LCP (Largest Contentful Paint)
- avoids shipping oversized images

Docs: [https://nextjs.org/docs/app/building-your-application/optimizing/images](https://nextjs.org/docs/app/building-your-application/optimizing/images)

---

### 4. Code splitting + dynamic imports

Next.js automatically splits routes, but you should also split heavy components.

```tsx
import dynamic from "next/dynamic";

const HeavyChart = dynamic(() => import("./HeavyChart"), {
  ssr: false,
  loading: () => <p>Loading chart...</p>,
});

export default function Dashboard() {
  return <HeavyChart />;
}
```

**Use case:**

- charts
- editors (Monaco)
- maps
- analytics dashboards

---

### 5. Fonts optimization (reduce layout shift + requests)

Use `next/font` to self-host fonts automatically.

```tsx
import { Inter } from "next/font/google";

const inter = Inter({
  subsets: ["latin"],
  display: "swap",
});

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html className={inter.className}>
      <body>{children}</body>
    </html>
  );
}
```

Benefits:

- no external font blocking
- reduced CLS
- better performance scoring

Docs: [https://nextjs.org/docs/app/building-your-application/optimizing/fonts](https://nextjs.org/docs/app/building-your-application/optimizing/fonts)

---

### 6. Streaming + Suspense (faster perceived performance)

Next.js supports **React Streaming SSR**.

```tsx
import { Suspense } from "react";
import Reviews from "./Reviews";

export default function ProductPage() {
  return (
    <div>
      <h1>Product</h1>

      <Suspense fallback={<p>Loading reviews...</p>}>
        <Reviews />
      </Suspense>
    </div>
  );
}
```

**Why it matters:**

- HTML is streamed progressively
- critical content loads first
- improves TTFB perception

---

### 7. Edge runtime & Middleware optimization

Use Edge Runtime for low-latency logic (auth, redirects, personalization).

```tsx
export const runtime = "edge";

export function middleware(req: Request) {
  const url = new URL(req.url);

  if (!req.headers.get("cookie")) {
    return Response.redirect(new URL("/login", url));
  }

  return Response.next();
}
```

Use cases:

- A/B testing
- geo personalization
- auth gating

Docs: [https://nextjs.org/docs/app/building-your-application/routing/middleware](https://nextjs.org/docs/app/building-your-application/routing/middleware)

---

### 8. Avoid unnecessary Client Components

A common performance mistake is overusing `"use client"`.

Bad:

```tsx
"use client"; // entire tree becomes client-side
```

Better:

- isolate interactivity
- keep layout/pages server-side
- pass props down

---

**Performance considerations (real-world):**

- Measure with Lighthouse + Web Vitals (LCP, INP, CLS)
- Watch hydration cost (bundle analyzer)
- Prefer server rendering over CSR
- Cache aggressively at edge when possible
- Avoid large client bundles in dashboards

---

**Common pitfalls & solutions:**

1. ❌ Overusing Client Components
   → Keep default Server Components

2. ❌ No caching strategy in fetch
   → Always define `revalidate` or `no-store` explicitly

3. ❌ Large images without optimization
   → Always use `next/image`

4. ❌ Blocking waterfalls in data fetching
   → Parallelize fetch calls:

```tsx
const [a, b] = await Promise.all([fetch("/api/a"), fetch("/api/b")]);
```

5. ❌ Hydrating entire pages unnecessarily
   → Use partial client boundaries

---

**Code (Production-ready optimization pattern):**

```tsx
// app/dashboard/page.tsx

import { Suspense } from "react";
import dynamic from "next/dynamic";

const Chart = dynamic(() => import("./Chart"), {
  ssr: false,
});

async function getStats() {
  const res = await fetch("https://api.example.com/stats", {
    next: { revalidate: 30 },
  });
  return res.json();
}

export default async function DashboardPage() {
  const stats = await getStats();

  return (
    <div>
      <h1>Dashboard</h1>

      <Suspense fallback={<p>Loading chart...</p>}>
        <Chart data={stats} />
      </Suspense>
    </div>
  );
}
```

---

**When to use:**

- High-traffic production apps (SaaS, e-commerce, dashboards)
- SEO-critical pages (marketing, blogs)
- Global applications requiring edge delivery
- Performance-sensitive user experiences

---

**Alternatives:**

- CSR (React SPA) → simpler but worse SEO/TTFB
- Remix → strong caching model but different ecosystem
- Traditional SSR (Express + React) → more control but less optimization built-in
- Static-only (SSG) → fastest but not dynamic

## Question 2. How do you handle analytics in Next.js?

## Question 3. How do you integrate Redux or Zustand in Next.js?

## Question 4. How do you implement server-side API calls with Axios or Fetch?

## Question 5. How can you protect server-side rendered pages?

## Question 6. What is the role of next.config.js?

## Question 7. How can you configure redirects and rewrites in next.config.js?

## Question 8. How do you handle image domains in next.config.js?

## Question 9. How do you add custom headers in Next.js?

## Question 10. How do you enable internationalization (i18n) in Next.js?

## Question 11. Explain the Next.js app router (app/ directory) vs pages router (pages/)

## Question 12. How does server components work in Next.js 13+?

## Question 13. What are the advantages of React server components in Next.js?

## Question 14. How does Next.js 13 handle streaming?

## Question 15. How do you implement layouts in the new app router?

## Question 16. How do nested layouts work in Next.js 13+?

## Question 17. How do you handle caching in server components?

## Question 18. What is the difference between server and client components?

## Question 19. How do you share state between server and client components?

## Question 20. How does Next.js handle edge functions?
