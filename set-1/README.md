# Set 1

| #   | Question                                                                                                                                                   |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [What is Next.js and why is it used?](#question-1-what-is-nextjs-and-why-is-it-used)                                                                       |
| 2   | [How is Next.js different from React?](#question-2-how-is-nextjs-different-from-react)                                                                     |
| 3   | [What is server-side rendering (SSR)?](#question-3-what-is-server-side-rendering-ssr)                                                                      |
| 4   | [What is static site generation (SSG)?](#question-4-what-is-static-site-generation-ssg)                                                                    |
| 5   | [What is client-side rendering (CSR) in Next.js?](#question-5-what-is-client-side-rendering-csr-in-nextjs)                                                 |
| 6   | [What is incremental static regeneration (ISR)?](#question-6-what-is-incremental-static-regeneration-isr)                                                  |
| 7   | [Explain the folder structure of a Next.js project](#question-7-explain-the-folder-structure-of-a-nextjs-project)                                          |
| 8   | [What is the pages directory used for?](#question-8-what-is-the-pages-directory-used-for)                                                                  |
| 9   | [What is the public folder in Next.js?](#question-9-what-is-the-public-folder-in-nextjs)                                                                   |
| 10  | [How does Next.js handle routing?](#question-10-how-does-nextjs-handle-routing)                                                                            |
| 11  | [What is a dynamic route in Next.js?](#question-11-what-is-a-dynamic-route-in-nextjs)                                                                      |
| 12  | [How do you create a dynamic route?](#question-12-how-do-you-create-a-dynamic-route)                                                                       |
| 13  | [What is a catch-all route?](#question-13-what-is-a-catch-all-route)                                                                                       |
| 14  | [How can you create API routes in Next.js?](#question-14-how-can-you-create-api-routes-in-nextjs)                                                          |
| 15  | [What is the difference between getStaticProps and getServerSideProps?](#question-15-what-is-the-difference-between-getstaticprops-and-getserversideprops) |
| 16  | [What is the difference between getStaticPaths and getServerSideProps?](#question-16-what-is-the-difference-between-getstaticpaths-and-getserversideprops) |
| 17  | [How can you fetch data in Next.js?](#question-17-how-can-you-fetch-data-in-nextjs)                                                                        |
| 18  | [What is the `_app.js` file used for?](#question-18-what-is-the-_appjs-file-used-for)                                                                      |
| 19  | [What is the `_document.js` file used for?](#question-19-what-is-the-_documentjs-file-used-for)                                                            |
| 20  | [How can you add global CSS in Next.js?](#question-20-how-can-you-add-global-css-in-nextjs)                                                                |

## Question 1. What is Next.js and why is it used?

Next.js is a full-stack React framework built on top of React that provides production-ready features like Server Components, file-based routing, SSR, SSG, streaming, API handling, and optimized performance out of the box. It’s used to build scalable, SEO-friendly, high-performance web applications without manually configuring complex tooling.

### 1. What is Next.js?

[Next.js Official Docs](https://nextjs.org/docs)

Next.js is a React framework developed by [Vercel](https://vercel.com) that solves many problems developers face in plain React applications:

- Routing
- SEO
- Performance optimization
- Data fetching
- Code splitting
- Backend APIs
- Rendering strategies
- Deployment optimization

In React alone, you typically assemble many libraries manually:

- React Router
- Webpack/Vite config
- SSR setup
- API layer
- Image optimization
- Performance tooling

Next.js provides all of this in a single framework with conventions and production defaults.

In **Next.js 16**, the recommended architecture is:

- **App Router**
- **React Server Components (RSC)**
- **Streaming**
- **Server Actions**
- **Partial prerendering**
- **Edge runtime support**

Docs:

- [App Router Docs](https://nextjs.org/docs/app)
- [Server Components Docs](https://nextjs.org/docs/app/building-your-application/rendering/server-components)

### 2. Why is Next.js Used?

#### A. SEO Optimization

Traditional React apps render mostly on the client side.

Problem:

- Search engines may not fully index content quickly.
- Initial page load can be slow.

Next.js solves this using:

- SSR (Server-Side Rendering)
- SSG (Static Site Generation)
- Streaming rendering

Example:

- E-commerce product pages
- Marketing websites
- Blogs
- News portals

These need HTML generated on the server for SEO.

#### B. Better Performance

Next.js includes built-in optimizations:

##### Automatic Code Splitting

Only the required JavaScript is loaded per route.

##### Image Optimization

Using the built-in `next/image` component.

##### Streaming

Send UI progressively instead of waiting for full page render.

##### React Server Components

Reduce client-side JavaScript bundle size.

Docs:

- [Image Optimization](https://nextjs.org/docs/app/building-your-application/optimizing/images)
- [Streaming](https://nextjs.org/docs/app/building-your-application/routing/loading-ui-and-streaming)

#### C. Multiple Rendering Strategies

One major reason companies adopt Next.js is flexibility.

You can choose per route:

| Rendering Type | Use Case                |
| -------------- | ----------------------- |
| SSR            | Dynamic dashboards      |
| SSG            | Blogs/docs              |
| ISR            | Product catalogs        |
| CSR            | Highly interactive UI   |
| Edge Rendering | Low latency global apps |

This hybrid rendering model is a huge advantage.

#### D. Full-Stack Capabilities

Next.js is no longer just frontend.

With:

- Route Handlers
- Server Actions
- Middleware
- Edge Functions

You can build full-stack applications in one codebase.

Example:

- Authentication
- Payments
- Form handling
- CRUD APIs

Docs:

- [Route Handlers](https://nextjs.org/docs/app/building-your-application/routing/route-handlers)
- [Server Actions](https://nextjs.org/docs/app/building-your-application/data-fetching/server-actions-and-mutations)

### 3. Core Architecture in Next.js 16

#### App Router

The App Router uses the `app/` directory.

Example:

```bash
app/
 ├── page.tsx
 ├── layout.tsx
 ├── products/
 │    └── page.tsx
```

Benefits:

- Nested layouts
- Streaming
- Server Components
- Better data fetching
- Shared UI persistence

#### Server Components by Default

In Next.js 16:

- Components are Server Components unless marked `"use client"`.

Benefits:

- Reduced JS bundle
- Better security
- Faster page loads
- Direct database access on server

Example:

```tsx
// app/products/page.tsx

async function getProducts() {
  const res = await fetch("https://api.example.com/products");
  return res.json();
}

export default async function ProductsPage() {
  const products = await getProducts();

  return (
    <div>
      {products.map((product: any) => (
        <p key={product.id}>{product.name}</p>
      ))}
    </div>
  );
}
```

No client-side fetching needed.

### 4. Production-Level Features

#### File-Based Routing

Routes are created from folder structure.

Example:

```bash
app/about/page.tsx
```

Becomes:

```bash
/about
```

#### Built-In API Handling

```tsx
// app/api/users/route.ts

import { NextResponse } from "next/server";

export async function GET() {
  return NextResponse.json({
    users: [],
  });
}
```

#### Middleware

Used for:

- Authentication
- Redirects
- Geo-based routing
- A/B testing

```tsx
// middleware.ts

import { NextResponse } from "next/server";

export function middleware() {
  return NextResponse.next();
}
```

### 5. Performance Considerations

#### Prefer Server Components

Best practice in Next.js 16:

- Keep most UI on server
- Use Client Components only for interactivity

Good:

- Product pages
- CMS pages
- Dashboards

Use Client Components only for:

- State
- Effects
- Event handlers
- Browser APIs

#### Minimize Client JavaScript

Bad:

```tsx
"use client";
```

at the root layout level.

Why?

- Entire subtree becomes client-rendered.
- Bundle size increases.

#### Use Streaming + Suspense

```tsx
import { Suspense } from "react";

<Suspense fallback={<p>Loading...</p>}>
  <Products />
</Suspense>;
```

This improves Time To First Byte (TTFB) and perceived performance.

### 6. Common Pitfalls & Solutions

| Pitfall                                    | Solution                               |
| ------------------------------------------ | -------------------------------------- |
| Overusing Client Components                | Default to Server Components           |
| Fetching data in `useEffect` unnecessarily | Fetch on server                        |
| Large JS bundles                           | Use RSC + dynamic imports              |
| Poor caching strategy                      | Use `revalidate` and caching correctly |
| Blocking rendering                         | Use streaming and Suspense             |

### Code Example (Production-Ready)

```tsx
// app/products/page.tsx

import Image from "next/image";

type Product = {
  id: number;
  title: string;
  image: string;
};

async function getProducts(): Promise<Product[]> {
  const res = await fetch("https://fakestoreapi.com/products", {
    next: {
      revalidate: 3600,
    },
  });

  if (!res.ok) {
    throw new Error("Failed to fetch products");
  }

  return res.json();
}

export default async function ProductsPage() {
  const products = await getProducts();

  return (
    <main className="grid grid-cols-3 gap-6 p-8">
      {products.map((product) => (
        <div key={product.id} className="border rounded-lg p-4">
          <Image
            src={product.image}
            alt={product.title}
            width={200}
            height={200}
          />

          <h2 className="mt-4 font-semibold">{product.title}</h2>
        </div>
      ))}
    </main>
  );
}
```

### When to Use Next.js

Use Next.js when you need:

- SEO-friendly applications
- Enterprise-scale React apps
- Fast initial page loads
- Hybrid rendering
- Full-stack capabilities
- Streaming and RSC architecture
- Optimized performance by default

Common production use cases:

- E-commerce
- SaaS platforms
- Dashboards
- Marketing sites
- CMS-driven apps
- AI applications
- Multi-tenant platforms

### Alternatives

| Technology   | Comparison                      |
| ------------ | ------------------------------- |
| React + Vite | More manual setup               |
| Remix        | Strong nested routing/data APIs |
| Gatsby       | Better for static-heavy sites   |
| Nuxt         | Vue equivalent of Next.js       |
| Angular      | Full framework but heavier      |

> “Next.js is not just a React framework anymore — it’s a full-stack React platform optimized for performance, scalability, and server-first rendering using React Server Components and streaming architecture.”

## Question 2. How is Next.js different from React?

React is a JavaScript library for building UI components, while Next.js is a full-stack framework built on top of React that provides routing, SSR, Server Components, API handling, optimization, and production-ready architecture out of the box. React handles the “view layer,” whereas Next.js provides the complete application framework.

### 1. Core Difference: Library vs Framework

#### React

[React Docs](https://react.dev)

React is primarily:

- A UI library
- Focused on component rendering
- Responsible for building interactive user interfaces

React itself does **not** provide:

- Routing
- SSR
- API layer
- File-based routing
- SEO optimizations
- Full-stack architecture
- Build optimization conventions

With plain React, developers usually add:

- React Router
- Vite/Webpack
- State libraries
- Data-fetching libraries
- SSR infrastructure manually

#### Next.js

[Next.js Docs](https://nextjs.org/docs)

Next.js is a React framework that adds:

- App Router
- Server Components
- SSR/SSG/ISR
- Streaming
- API routes
- Middleware
- Image optimization
- Server Actions
- Edge runtime
- Production deployment architecture

So:

```text
React = UI Library
Next.js = Full-stack React Framework
```

### 2. Architecture Comparison

| Feature            | React                      | Next.js      |
| ------------------ | -------------------------- | ------------ |
| UI Components      | ✅                         | ✅           |
| Routing            | ❌ External library needed | ✅ Built-in  |
| SSR                | ❌ Manual setup            | ✅ Built-in  |
| SSG                | ❌ Manual setup            | ✅ Built-in  |
| API Backend        | ❌                         | ✅           |
| Server Components  | ❌ Not framework-managed   | ✅           |
| SEO Optimization   | ❌ Limited                 | ✅ Excellent |
| Streaming          | ❌ Manual                  | ✅ Built-in  |
| Image Optimization | ❌                         | ✅           |
| Middleware         | ❌                         | ✅           |
| Edge Runtime       | ❌                         | ✅           |

### 3. Routing Difference

#### React

In React, routing is manually configured.

Example using React Router:

```tsx
import { BrowserRouter, Routes, Route } from "react-router-dom";

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
      </Routes>
    </BrowserRouter>
  );
}
```

This requires:

- Additional dependency
- Route configuration
- Client-side rendering setup

#### Next.js

Next.js uses file-system routing.

```bash
app/
 ├── page.tsx
 ├── about/
 │    └── page.tsx
```

Automatically becomes:

```bash
/ -> Home
/about -> About
```

Docs:

- [App Router Docs](https://nextjs.org/docs/app/building-your-application/routing)

### 4. Rendering Difference

This is one of the biggest interview discussion points.

#### React = Mostly Client-Side Rendering (CSR)

Traditional React apps:

1. Send minimal HTML
2. Browser downloads JS
3. React hydrates UI

Problem:

- Slower initial load
- SEO limitations
- Larger JS bundles

#### Next.js = Multiple Rendering Strategies

Next.js supports:

| Strategy       | Description              |
| -------------- | ------------------------ |
| CSR            | Client rendering         |
| SSR            | Server-side rendering    |
| SSG            | Static generation        |
| ISR            | Incremental regeneration |
| Streaming      | Progressive rendering    |
| Edge Rendering | Low latency rendering    |

This flexibility is a major advantage.

### 5. Server Components (Huge Difference in Next.js 16)

#### React Alone

React itself introduced Server Components conceptually, but React alone does not provide a production framework around them.

#### Next.js 16

Next.js fully implements:

- React Server Components (RSC)
- Server Actions
- Streaming architecture

By default:

```tsx
// Server Component
export default function Page() {
  return <h1>Hello</h1>;
}
```

Client Components require:

```tsx
"use client";
```

Benefits:

- Less client JS
- Better performance
- Direct server-side data access
- Better security

Docs:

- [Server Components Docs](https://nextjs.org/docs/app/building-your-application/rendering/server-components)

### 6. Backend Capabilities

#### React

React requires a separate backend:

- Node.js
- Express
- NestJS
- Spring Boot
- Django

#### Next.js

Next.js includes backend capabilities directly.

Example Route Handler:

```tsx
// app/api/users/route.ts

import { NextResponse } from "next/server";

export async function GET() {
  return NextResponse.json({
    users: [],
  });
}
```

This enables:

- Full-stack monorepo architecture
- Shared types
- Simplified deployment

Docs:

- [Route Handlers Docs](https://nextjs.org/docs/app/building-your-application/routing/route-handlers)

---

### 7. Performance Optimization

#### React

Performance optimization is mostly manual:

- Lazy loading
- Code splitting
- Asset optimization
- Image optimization

#### Next.js

Next.js includes automatic optimizations:

#### Automatic Code Splitting

Per-route bundle splitting.

#### Image Optimization

```tsx
import Image from "next/image";
```

#### Font Optimization

```tsx
import { Inter } from "next/font/google";
```

#### Streaming + Suspense

#### Prefetching Links

```tsx
import Link from "next/link";
```

Docs:

- [Optimizing Docs](https://nextjs.org/docs/app/building-your-application/optimizing)

---

### 8. Developer Experience

#### React

You configure:

- Build tools
- Babel
- Routing
- SSR
- Folder structure
- Deployment pipeline

#### Next.js

Next.js provides:

- Opinionated architecture
- Conventions
- Production defaults
- Integrated tooling

This increases team consistency and scalability.

### 9. Production-Level Code Comparison

#### React Example

```tsx
import { useEffect, useState } from "react";

export default function Products() {
  const [products, setProducts] = useState([]);

  useEffect(() => {
    fetch("/api/products")
      .then((res) => res.json())
      .then(setProducts);
  }, []);

  return (
    <div>
      {products.map((p: any) => (
        <p key={p.id}>{p.title}</p>
      ))}
    </div>
  );
}
```

Problems:

- Client-side fetching
- Loading states
- SEO limitations
- More JS shipped

## Next.js 16 Example

```tsx
// app/products/page.tsx

type Product = {
  id: number;
  title: string;
};

async function getProducts(): Promise<Product[]> {
  const res = await fetch("https://api.example.com/products", {
    next: {
      revalidate: 3600,
    },
  });

  if (!res.ok) {
    throw new Error("Failed to fetch products");
  }

  return res.json();
}

export default async function ProductsPage() {
  const products = await getProducts();

  return (
    <div>
      {products.map((product) => (
        <p key={product.id}>{product.title}</p>
      ))}
    </div>
  );
}
```

Advantages:

- Server-rendered
- SEO-friendly
- Cached
- Smaller client bundle
- Faster initial load

### 10. Common Interview Insight

> “React solves component rendering, while Next.js solves application architecture, rendering strategy, performance optimization, and full-stack delivery.”

### When to Use React vs Next.js

## Use React Only When

- Building internal tools
- Pure SPA applications
- Lightweight frontend widgets
- Microfrontends
- Electron apps

## Use Next.js When

- SEO matters
- Performance matters
- You need SSR/SSG
- Building production SaaS
- E-commerce platforms
- Content-heavy apps
- Full-stack React apps

### Alternatives

| Technology   | Comparison                           |
| ------------ | ------------------------------------ |
| React + Vite | Lightweight SPA setup                |
| Remix        | Nested routing + web standards focus |
| Gatsby       | Static site generation               |
| Nuxt         | Vue equivalent                       |
| Angular      | Full enterprise framework            |

> “React is the foundation for building UI components, while Next.js is an opinionated full-stack framework built on React that adds routing, server rendering, caching, streaming, API handling, and production optimizations. In modern production systems, Next.js is often preferred because it solves scalability, SEO, and performance challenges that plain React leaves to developers.”

## Question 3. What is server-side rendering (SSR)?

**Q: What is Server-Side Rendering (SSR)?**

**Short Answer (30 seconds):**
Server-Side Rendering (SSR) is a rendering strategy where HTML is generated on the server for every request and sent fully rendered to the browser. In Next.js, SSR improves SEO, faster first contentful paint, and better performance for dynamic content by rendering pages on the server before hydration happens on the client.

---

# Detailed Explanation

## 1. What is SSR?

In SSR, the server:

1. Receives a request
2. Fetches required data
3. Renders the React component into HTML
4. Sends fully rendered HTML to the browser
5. React hydrates the page on the client for interactivity

Flow:

```text
Browser Request
      ↓
Next.js Server
      ↓
Fetch Data
      ↓
Render HTML
      ↓
Send HTML to Browser
      ↓
Hydration
      ↓
Interactive Page
```

---

# 2. Why SSR is Important

SSR solves major problems found in client-side rendered apps.

---

## A. Better SEO

Search engines can immediately read fully rendered HTML.

Good for:

- E-commerce
- Blogs
- News sites
- Marketing pages

Without SSR:

- Search crawlers may only see an empty div initially.

---

## B. Faster Initial Page Load

Users see content earlier because HTML already contains the rendered UI.

This improves:

- FCP (First Contentful Paint)
- LCP (Largest Contentful Paint)

---

## C. Better Social Sharing

Meta tags are rendered server-side.

Example:

- Open Graph tags
- Twitter cards

---

# 3. SSR in React vs Next.js

## React Alone

React itself does not provide SSR.

You must configure:

- Node server
- React DOM Server
- Hydration
- Routing
- Streaming
- Caching

This is complex.

---

## Next.js

Next.js provides SSR out of the box.

Docs:

- [Server Rendering Docs](https://nextjs.org/docs/app/building-your-application/rendering/server-components)
- [Data Fetching Docs](https://nextjs.org/docs/app/building-your-application/data-fetching)

In Next.js 16 App Router:

- Server Components are rendered on the server by default.
- Data fetching naturally happens server-side.

---

# 4. SSR in Next.js 16 (App Router)

In modern Next.js, SSR is often automatic.

Example:

```tsx
// app/products/page.tsx

type Product = {
  id: number;
  title: string;
};

async function getProducts(): Promise<Product[]> {
  const res = await fetch("https://api.example.com/products", {
    cache: "no-store",
  });

  if (!res.ok) {
    throw new Error("Failed to fetch products");
  }

  return res.json();
}

export default async function ProductsPage() {
  const products = await getProducts();

  return (
    <div>
      {products.map((product) => (
        <p key={product.id}>{product.title}</p>
      ))}
    </div>
  );
}
```

---

# 5. Why `cache: 'no-store'` Enables SSR

```tsx
cache: "no-store";
```

This tells Next.js:

- Do not cache the response
- Render on every request

That means:

- Fresh data each request
- Dynamic rendering
- True SSR behavior

---

# 6. Hydration in SSR

After HTML is delivered:
React hydrates the page.

Hydration means:

- React attaches event listeners
- Makes UI interactive

Without hydration:

- Buttons wouldn’t work
- State wouldn’t update

---

# 7. SSR vs CSR vs SSG

This comparison is commonly asked in interviews.

| Strategy | Rendering Location  | Best For                |
| -------- | ------------------- | ----------------------- |
| CSR      | Browser             | Highly interactive apps |
| SSR      | Server per request  | Dynamic SEO pages       |
| SSG      | Build time          | Static content          |
| ISR      | Hybrid regeneration | Large content sites     |

---

# 8. SSR Request Lifecycle in Next.js

```text
User Request
   ↓
Next.js Server
   ↓
Fetch Database/API
   ↓
Render React Components
   ↓
Generate HTML
   ↓
Send HTML
   ↓
Hydration on Client
```

---

# 9. Streaming SSR (Modern Next.js)

Traditional SSR blocks rendering until everything finishes.

Next.js 16 supports streaming:

- Send partial HTML immediately
- Stream remaining UI progressively

Using Suspense:

```tsx
import { Suspense } from "react";

export default function Page() {
  return (
    <Suspense fallback={<p>Loading...</p>}>
      <Products />
    </Suspense>
  );
}
```

Benefits:

- Faster perceived performance
- Better user experience
- Reduced blocking

Docs:

- [Streaming Docs](https://nextjs.org/docs/app/building-your-application/routing/loading-ui-and-streaming)

---

# 10. Production-Level SSR Example

```tsx
// app/dashboard/page.tsx

type Analytics = {
  users: number;
  revenue: number;
};

async function getAnalytics(): Promise<Analytics> {
  const res = await fetch("https://api.example.com/analytics", {
    cache: "no-store",
  });

  if (!res.ok) {
    throw new Error("Analytics fetch failed");
  }

  return res.json();
}

export default async function DashboardPage() {
  const analytics = await getAnalytics();

  return (
    <main className="p-8">
      <h1 className="text-2xl font-bold">Dashboard</h1>

      <div className="mt-6 space-y-4">
        <p>Users: {analytics.users}</p>
        <p>Revenue: ${analytics.revenue}</p>
      </div>
    </main>
  );
}
```

Why this is SSR:

- Fresh data per request
- Server-rendered HTML
- No client-side loading spinner required

---

# 11. Performance Considerations

## Use SSR Only When Needed

SSR increases server workload because rendering happens per request.

Good use cases:
✅ Personalized dashboards
✅ Frequently changing data
✅ SEO-critical dynamic pages
✅ Authenticated pages

Avoid SSR for:
❌ Fully static pages
❌ Rarely changing content

Use SSG or ISR instead.

---

# 12. Common Pitfalls & Solutions

| Pitfall                                          | Solution                             |
| ------------------------------------------------ | ------------------------------------ |
| Overusing SSR                                    | Use static rendering where possible  |
| Slow APIs blocking rendering                     | Use streaming + Suspense             |
| Large server computation                         | Cache aggressively                   |
| Hydration mismatch                               | Keep server/client output consistent |
| Fetching data in Client Components unnecessarily | Fetch in Server Components           |

---

# 13. Common Interview Follow-Up

## “What is hydration mismatch?”

When server-rendered HTML differs from client-rendered HTML.

Example causes:

- `Math.random()`
- `Date.now()`
- Browser-only APIs during SSR

Bad:

```tsx
<p>{Date.now()}</p>
```

Fix:

- Move to Client Component
- Use `useEffect`

---

# 14. SSR vs Server Components

This is a senior-level distinction.

| Concept           | Description               |
| ----------------- | ------------------------- |
| SSR               | Rendering strategy        |
| Server Components | Component execution model |

Important:

- Server Components may still be statically rendered.
- SSR specifically means rendering per request.

---

# When to Use SSR

Use SSR when:

- Data changes frequently
- SEO matters
- Personalized content exists
- Auth/session-based rendering is needed
- Real-time dashboards exist

Examples:

- Trading dashboards
- Analytics apps
- E-commerce inventory pages
- Travel booking systems

---

# Alternatives

| Strategy       | Best Use                |
| -------------- | ----------------------- |
| SSG            | Blogs/docs              |
| ISR            | Product catalogs        |
| CSR            | Highly interactive apps |
| Edge Rendering | Low latency global apps |

---

# Final Interview Summary

> “SSR is a rendering strategy where HTML is generated on the server for every request and sent pre-rendered to the client. In Next.js 16, SSR is implemented naturally through Server Components and dynamic rendering, improving SEO, initial load performance, and user experience while still allowing client-side hydration for interactivity.”

## Question 4. What is static site generation (SSG)?

**Q: What is Static Site Generation (SSG)?**

**Short Answer (30 seconds):**
Static Site Generation (SSG) is a rendering strategy where HTML pages are generated at build time and served as static files through a CDN. In Next.js, SSG provides extremely fast performance, excellent SEO, low server cost, and high scalability because pages don’t need to be rendered on every request.

---

# Detailed Explanation

## 1. What is SSG?

With SSG:

1. Pages are pre-rendered during the build process
2. HTML files are generated ahead of time
3. Static assets are deployed to a CDN
4. Users receive prebuilt HTML instantly

Flow:

```text
Build Time
   ↓
Next.js Generates HTML
   ↓
Deploy Static Files to CDN
   ↓
User Requests Page
   ↓
CDN Serves HTML Instantly
```

Unlike SSR:

- No server rendering per request
- No database call during request time
- No runtime rendering overhead

---

# 2. Why SSG is Important

SSG is one of the fastest rendering strategies available.

Benefits:

✅ Extremely fast page loads
✅ Excellent SEO
✅ Low infrastructure cost
✅ High scalability
✅ CDN-friendly
✅ Better reliability

---

# 3. SSG in Next.js 16

Docs:

- [Static Rendering Docs](https://nextjs.org/docs/app/building-your-application/rendering/static-and-dynamic-rendering)

In the App Router, pages are statically generated by default when:

- No dynamic APIs are used
- No `cache: 'no-store'`
- No request-specific data exists

Example:

```tsx
// app/blog/page.tsx

async function getPosts() {
  const res = await fetch("https://api.example.com/posts");

  return res.json();
}

export default async function BlogPage() {
  const posts = await getPosts();

  return (
    <div>
      {posts.map((post: any) => (
        <h2 key={post.id}>{post.title}</h2>
      ))}
    </div>
  );
}
```

This page is statically generated if the data is cacheable.

---

# 4. How Next.js Detects SSG

Next.js automatically chooses static rendering when:

- Data is cacheable
- No cookies/headers usage
- No dynamic rendering flags
- No authenticated request dependency

This automatic optimization is a huge feature of Next.js 16.

---

# 5. SSG vs SSR

This comparison is critical in interviews.

| Feature        | SSG            | SSR               |
| -------------- | -------------- | ----------------- |
| Rendering Time | Build time     | Request time      |
| Speed          | Extremely fast | Slower            |
| Server Load    | Minimal        | Higher            |
| SEO            | Excellent      | Excellent         |
| Scalability    | Very high      | Limited by server |
| Dynamic Data   | Poor fit       | Excellent         |

---

# 6. Real-World Example

## Good SSG Candidates

### Blogs

Content changes infrequently.

### Documentation Sites

Mostly static content.

### Marketing Websites

SEO-focused and rarely updated.

### Portfolio Sites

Static content.

### Landing Pages

High traffic, minimal dynamic data.

---

# 7. Build-Time Rendering

During:

```bash
next build
```

Next.js:

1. Executes server components
2. Fetches data
3. Generates HTML
4. Saves output as static assets

Example generated files:

```bash
.next/server/app/blog/page.html
```

These are deployed to the CDN.

---

# 8. Dynamic Routes with SSG

Next.js can statically generate dynamic pages too.

Example:

```bash
app/blog/[slug]/page.tsx
```

Using:

```tsx
// app/blog/[slug]/page.tsx

export async function generateStaticParams() {
  const posts = await fetch("https://api.example.com/posts").then((res) =>
    res.json(),
  );

  return posts.map((post: any) => ({
    slug: post.slug,
  }));
}
```

This pre-generates pages like:

```bash
/blog/react
/blog/nextjs
/blog/typescript
```

Docs:

- [generateStaticParams Docs](https://nextjs.org/docs/app/api-reference/functions/generate-static-params)

---

# 9. Production-Level SSG Example

```tsx
// app/docs/page.tsx

type Doc = {
  id: number;
  title: string;
};

async function getDocs(): Promise<Doc[]> {
  const res = await fetch("https://api.example.com/docs", {
    next: {
      revalidate: 3600,
    },
  });

  if (!res.ok) {
    throw new Error("Failed to fetch docs");
  }

  return res.json();
}

export default async function DocsPage() {
  const docs = await getDocs();

  return (
    <main className="p-8">
      <h1 className="text-3xl font-bold">Documentation</h1>

      <ul className="mt-6 space-y-3">
        {docs.map((doc) => (
          <li key={doc.id}>{doc.title}</li>
        ))}
      </ul>
    </main>
  );
}
```

This is:

- Static by default
- CDN-cacheable
- Extremely fast

---

# 10. SSG + ISR (Incremental Static Regeneration)

Pure SSG has one limitation:

- Content becomes stale after deployment.

Next.js solves this using ISR.

Example:

```tsx
fetch(url, {
  next: {
    revalidate: 3600,
  },
});
```

Meaning:

- Regenerate page every hour
- Keep static performance
- Update content automatically

This hybrid approach is heavily used in production.

Docs:

- [ISR Docs](https://nextjs.org/docs/app/building-your-application/data-fetching/incremental-static-regeneration)

---

# 11. Performance Advantages

SSG is often the fastest architecture.

Benefits:

- CDN edge delivery
- Near-zero TTFB
- Minimal backend load
- High scalability
- Better Core Web Vitals

For high-traffic websites, SSG can dramatically reduce infrastructure costs.

---

# 12. Common Pitfalls

| Pitfall                                  | Solution               |
| ---------------------------------------- | ---------------------- |
| Using SSG for highly dynamic data        | Use SSR instead        |
| Huge build times with thousands of pages | Use ISR                |
| Stale content                            | Configure revalidation |
| Sensitive authenticated data             | Avoid static rendering |
| Excessive static generation              | Use on-demand ISR      |

---

# 13. SSG vs CSR vs SSR

| Strategy | Best For                         |
| -------- | -------------------------------- |
| SSG      | Blogs/docs/marketing             |
| SSR      | Personalized dynamic pages       |
| CSR      | Interactive SPA features         |
| ISR      | Large scalable content platforms |

---

# 14. Advanced Interview Insight

A senior-level explanation includes:

> “SSG shifts rendering work from request time to build time.”

That demonstrates understanding of scalability and architecture.

Another strong insight:

> “SSG works best when data changes less frequently than user requests.”

---

# 15. SSG in Modern Next.js Architecture

In Next.js 16:

- Static rendering is the default optimization
- Server Components make SSG more efficient
- Streaming can still work with static shells
- Partial prerendering improves hybrid performance

This server-first architecture is a major evolution from older React SPAs.

---

# When to Use SSG

Use SSG when:
✅ Content changes rarely
✅ SEO matters
✅ Performance is critical
✅ High traffic is expected
✅ Content can be cached globally

Examples:

- Blogs
- Documentation
- Marketing pages
- SaaS landing pages
- Product showcase pages

---

# Alternatives

| Strategy       | Use When                               |
| -------------- | -------------------------------------- |
| SSR            | Data changes per request               |
| ISR            | Mostly static but occasionally updated |
| CSR            | Browser-only interactivity             |
| Edge Rendering | Geo-sensitive dynamic content          |

---

# Final Interview Summary

> “SSG is a rendering strategy where pages are generated at build time and served as static HTML through a CDN. In Next.js 16, static rendering is the default optimization for cacheable pages, providing excellent SEO, near-instant performance, and massive scalability while minimizing server workload.”

## Question 5. What is client-side rendering (CSR) in Next.js?

**Q: What is Client-Side Rendering (CSR) in Next.js?**

**Short Answer (30 seconds):**
Client-Side Rendering (CSR) is a rendering strategy where the browser downloads JavaScript first, and React renders the UI on the client after hydration. In Next.js, CSR is mainly used for highly interactive, user-specific, or browser-dependent features, typically inside Client Components marked with `"use client"`.

---

# Detailed Explanation

## 1. What is Client-Side Rendering (CSR)?

In CSR:

1. The browser receives minimal HTML
2. JavaScript bundles are downloaded
3. React executes in the browser
4. Data is fetched client-side
5. UI is rendered dynamically

Flow:

```text
Browser Request
      ↓
Server Sends Minimal HTML + JS
      ↓
Browser Downloads JS
      ↓
React Executes in Browser
      ↓
Fetch API/Data
      ↓
Render UI
```

Unlike SSR:

- HTML is not fully rendered on the server
- Rendering happens primarily in the browser

---

# 2. CSR in Traditional React

Traditional React SPAs are mostly CSR-based.

Example:

```tsx
import { useEffect, useState } from "react";

export default function Products() {
  const [products, setProducts] = useState([]);

  useEffect(() => {
    fetch("/api/products")
      .then((res) => res.json())
      .then(setProducts);
  }, []);

  return (
    <div>
      {products.map((p: any) => (
        <p key={p.id}>{p.title}</p>
      ))}
    </div>
  );
}
```

Problems:

- Empty initial HTML
- SEO limitations
- Slower first render
- Loading states required

---

# 3. CSR in Next.js 16

In Next.js App Router:

- Server Components are default
- CSR happens inside Client Components

Client Components require:

```tsx
"use client";
```

Docs:

- [Client Components Docs](https://nextjs.org/docs/app/building-your-application/rendering/client-components)

---

# 4. Example of CSR in Next.js

```tsx
"use client";

import { useEffect, useState } from "react";

type User = {
  id: number;
  name: string;
};

export default function UsersList() {
  const [users, setUsers] = useState<User[]>([]);

  useEffect(() => {
    async function fetchUsers() {
      const res = await fetch("/api/users");
      const data = await res.json();

      setUsers(data);
    }

    fetchUsers();
  }, []);

  return (
    <div>
      {users.map((user) => (
        <p key={user.id}>{user.name}</p>
      ))}
    </div>
  );
}
```

Why this is CSR:

- Runs in browser
- Uses `useEffect`
- Fetches after page load
- Requires client JavaScript

---

# 5. Why CSR Exists in Next.js

Even though Next.js is server-first, CSR is still necessary for:

✅ Interactive UI
✅ Browser APIs
✅ Real-time updates
✅ User interactions
✅ Local state management
✅ Animations
✅ WebSocket connections

Examples:

- Chat apps
- Drag-and-drop
- Charts
- Maps
- Theme toggles
- Form interactions

---

# 6. Client Components vs Server Components

This is extremely important in Next.js interviews.

| Feature             | Server Component | Client Component |
| ------------------- | ---------------- | ---------------- |
| Runs On             | Server           | Browser          |
| JS Sent To Client   | Minimal          | Yes              |
| Access Browser APIs | ❌               | ✅               |
| useState/useEffect  | ❌               | ✅               |
| SEO Friendly        | Excellent        | Depends          |
| Bundle Size         | Smaller          | Larger           |

---

# 7. Hydration in CSR

Hydration means:

- React attaches interactivity to server-rendered HTML.

But in pure CSR:

- The browser often renders UI completely after JS loads.

This can delay:

- FCP
- LCP
- TTI

Which is why Next.js prefers Server Components first.

---

# 8. Production Architecture in Next.js 16

Best practice:

✅ Use Server Components by default
✅ Add Client Components only where needed

Bad architecture:

```tsx
"use client";

// entire application becomes client-rendered
```

This increases:

- Bundle size
- Hydration cost
- Runtime JS
- Performance overhead

---

# 9. Performance Trade-Offs of CSR

## Advantages

### Rich Interactivity

Best for dynamic UI.

### Reduced Server Load

Rendering shifts to browser.

### Better SPA Experience

Smooth transitions after hydration.

---

## Disadvantages

### Larger JS Bundles

Browser must execute more code.

### Poor SEO

Initial HTML may lack content.

### Slower Initial Render

JS must download before rendering.

### Hydration Cost

Heavy client hydration impacts performance.

---

# 10. CSR vs SSR vs SSG

| Strategy | Rendering Location  | Best For                |
| -------- | ------------------- | ----------------------- |
| CSR      | Browser             | Interactive apps        |
| SSR      | Server per request  | Dynamic SEO pages       |
| SSG      | Build time          | Static content          |
| ISR      | Hybrid regeneration | Large content platforms |

---

# 11. Production-Level CSR Example

```tsx
"use client";

import { useState } from "react";

export default function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div className="p-6">
      <p className="text-xl">Count: {count}</p>

      <button
        onClick={() => setCount(count + 1)}
        className="mt-4 rounded bg-black px-4 py-2 text-white"
      >
        Increment
      </button>
    </div>
  );
}
```

This must be a Client Component because:

- Uses state
- Uses click handlers
- Requires browser interactivity

---

# 12. Common Pitfalls

| Pitfall                                 | Solution                             |
| --------------------------------------- | ------------------------------------ |
| Overusing `"use client"`                | Keep components server-first         |
| Fetching everything in `useEffect`      | Fetch on server when possible        |
| Large hydration cost                    | Split into smaller Client Components |
| Browser-only logic in Server Components | Move to Client Components            |
| SEO-critical pages using CSR            | Use SSR or SSG                       |

---

# 13. Advanced Interview Insight

A senior-level understanding is:

> “CSR is no longer the default architecture in Next.js 16.”

Modern Next.js prefers:

- Server-first rendering
- Streaming
- React Server Components

CSR is now:

- Targeted
- Selective
- Interaction-focused

This is a huge architectural shift from older React SPA development.

---

# 14. Partial Client Rendering Pattern

Best practice:

```tsx
// Server Component

import Counter from "./Counter";

export default function Page() {
  return (
    <div>
      <h1>Dashboard</h1>

      <Counter />
    </div>
  );
}
```

Only the interactive part becomes client-rendered.

This minimizes JS sent to the browser.

---

# 15. Real-World Use Cases

CSR is ideal for:

✅ Dashboards
✅ Chat systems
✅ Live notifications
✅ Real-time analytics
✅ Interactive charts
✅ Form builders
✅ Drag-and-drop UIs

---

# Alternatives

| Approach          | Best Use                     |
| ----------------- | ---------------------------- |
| Server Components | Default rendering            |
| SSR               | Dynamic SEO pages            |
| SSG               | Static content               |
| ISR               | Scalable content updates     |
| CSR               | Interactivity-heavy features |

---

# Final Interview Summary

> “Client-Side Rendering in Next.js means rendering UI in the browser after JavaScript loads. In Next.js 16, CSR is implemented through Client Components using `'use client'`. While CSR is excellent for highly interactive experiences, modern Next.js prefers a server-first architecture using Server Components and streaming to minimize client JavaScript and improve performance.”

## Question 6. What is incremental static regeneration (ISR)?

**Q: What is Incremental Static Regeneration (ISR)?**

**Short Answer (30 seconds):**
Incremental Static Regeneration (ISR) is a Next.js rendering strategy that allows statically generated pages to be updated after deployment without rebuilding the entire application. ISR combines the performance of Static Site Generation (SSG) with the freshness of SSR by regenerating pages in the background at configurable intervals.

---

# Detailed Explanation

## 1. Why ISR Exists

Pure SSG has a limitation:

```text
Build Once → Content Becomes Stale
```

Example:

- E-commerce products change
- Blog posts update
- CMS content changes
- Prices/inventory change

Without ISR:

- You must rebuild and redeploy the app
- Large sites become expensive to rebuild

ISR solves this problem.

---

# 2. What ISR Does

ISR allows:

- Static pages to stay cached
- Pages to regenerate automatically
- Updates to happen incrementally

Flow:

```text
Initial Request
      ↓
Serve Static HTML
      ↓
Cache Page
      ↓
Revalidation Time Expires
      ↓
Background Regeneration
      ↓
New Static Version Replaces Old One
```

Key idea:

> Users continue receiving cached pages while regeneration happens in the background.

---

# 3. ISR in Next.js 16

Docs:

- [ISR Docs](https://nextjs.org/docs/app/building-your-application/data-fetching/incremental-static-regeneration)

In the App Router, ISR is configured using:

```tsx
next: {
  revalidate: 60;
}
```

Example:

```tsx
// app/products/page.tsx

async function getProducts() {
  const res = await fetch("https://api.example.com/products", {
    next: {
      revalidate: 60,
    },
  });

  return res.json();
}

export default async function ProductsPage() {
  const products = await getProducts();

  return (
    <div>
      {products.map((product: any) => (
        <p key={product.id}>{product.title}</p>
      ))}
    </div>
  );
}
```

---

# 4. What `revalidate: 60` Means

```tsx
revalidate: 60;
```

Meaning:

- Cache static page for 60 seconds
- After 60 seconds:
  - first request gets stale page
  - regeneration happens in background
  - cache updates automatically

This gives:
✅ Static performance
✅ Fresh content
✅ Minimal server load

---

# 5. ISR Request Lifecycle

```text
1st Request
   ↓
Generate Static HTML
   ↓
Store in Cache

Subsequent Requests
   ↓
Serve Cached HTML

Revalidation Window Expires
   ↓
Background Regeneration
   ↓
Replace Old Cache
```

---

# 6. ISR vs SSG vs SSR

| Strategy | Rendering Time             | Freshness    | Performance |
| -------- | -------------------------- | ------------ | ----------- |
| SSG      | Build time                 | Static       | Excellent   |
| ISR      | Build + background updates | Semi-dynamic | Excellent   |
| SSR      | Every request              | Always fresh | Slower      |

---

# 7. Why ISR is Powerful

ISR is one of Next.js’s biggest innovations because it solves scalability problems.

Without ISR:

- Large sites require full rebuilds
- Rebuilds become slow and expensive

With ISR:

- Only changed pages regenerate
- No full deployment required

This is critical for:

- E-commerce
- CMS platforms
- Marketplace apps
- Large documentation sites

---

# 8. Real-World Example

## E-commerce Product Pages

Imagine:

```text
100,000 products
```

Using SSR:

- Every request hits the server
- Expensive infrastructure

Using SSG:

- Full rebuild needed for updates

Using ISR:

- Products stay static
- Updated incrementally
- Best scalability/performance balance

---

# 9. Dynamic Routes + ISR

Example:

```bash
app/products/[id]/page.tsx
```

```tsx
// app/products/[id]/page.tsx

type Props = {
  params: Promise<{
    id: string;
  }>;
};

async function getProduct(id: string) {
  const res = await fetch(`https://api.example.com/products/${id}`, {
    next: {
      revalidate: 300,
    },
  });

  return res.json();
}

export default async function ProductPage({ params }: Props) {
  const { id } = await params;

  const product = await getProduct(id);

  return (
    <div>
      <h1>{product.title}</h1>
    </div>
  );
}
```

This statically caches each product page individually.

---

# 10. On-Demand Revalidation

Modern Next.js also supports manual invalidation.

Instead of waiting for time intervals, you can trigger revalidation when content changes.

Example:

```tsx
import { revalidatePath } from "next/cache";

revalidatePath("/products");
```

Useful for:

- CMS publishing
- Admin dashboards
- Real-time inventory updates

Docs:

- [revalidatePath Docs](https://nextjs.org/docs/app/api-reference/functions/revalidatePath)

---

# 11. Production-Level ISR Example

```tsx
// app/blog/page.tsx

type Post = {
  id: number;
  title: string;
};

async function getPosts(): Promise<Post[]> {
  const res = await fetch("https://api.example.com/posts", {
    next: {
      revalidate: 3600,
    },
  });

  if (!res.ok) {
    throw new Error("Failed to fetch posts");
  }

  return res.json();
}

export default async function BlogPage() {
  const posts = await getPosts();

  return (
    <main className="p-8">
      <h1 className="text-3xl font-bold">Blog</h1>

      <div className="mt-6 space-y-4">
        {posts.map((post) => (
          <article key={post.id}>
            <h2>{post.title}</h2>
          </article>
        ))}
      </div>
    </main>
  );
}
```

Benefits:

- Static performance
- Automatic updates
- CDN-friendly
- Massive scalability

---

# 12. Performance Benefits

ISR dramatically improves:

✅ Scalability
✅ CDN caching
✅ TTFB
✅ Infrastructure cost
✅ SEO
✅ Reliability

This is why ISR is heavily used in enterprise Next.js apps.

---

# 13. Common Pitfalls

| Pitfall                          | Solution                         |
| -------------------------------- | -------------------------------- |
| Revalidation interval too short  | Increases regeneration frequency |
| Revalidation interval too long   | Content becomes stale            |
| Using ISR for user-specific data | Use SSR instead                  |
| Forgetting cache invalidation    | Use on-demand revalidation       |
| Large-scale regeneration spikes  | Stagger cache windows            |

---

# 14. ISR + Server Components

In Next.js 16:

- ISR works naturally with Server Components
- Cached RSC payloads improve performance further
- Streaming can combine with ISR for faster UX

This creates:

```text
Static Shell + Streaming + Incremental Updates
```

Which is extremely performant.

---

# 15. Advanced Interview Insight

Strong senior-level statement:

> “ISR moves regeneration from deployment time to runtime while still preserving static delivery.”

Another excellent insight:

> “ISR is essentially cache invalidation plus background regeneration.”

That demonstrates architectural understanding.

---

# 16. When to Use ISR

Use ISR when:
✅ Content changes periodically
✅ SEO matters
✅ Traffic is high
✅ Full rebuilds are expensive
✅ Static performance is desired

Examples:

- E-commerce catalogs
- Blogs
- CMS sites
- Marketplace listings
- Documentation portals

---

# Alternatives

| Strategy | Best Use                       |
| -------- | ------------------------------ |
| SSG      | Fully static content           |
| SSR      | Personalized real-time content |
| CSR      | Highly interactive UI          |
| ISR      | Semi-dynamic scalable content  |

---

# Final Interview Summary

> “Incremental Static Regeneration is a Next.js rendering strategy that combines the speed of static generation with the flexibility of dynamic updates. Pages are statically cached and automatically regenerated in the background after a configurable interval, enabling large-scale applications to serve fast CDN-backed pages without rebuilding the entire site.”

## Question 7. Explain the folder structure of a Next.js project

**Q: Explain the folder structure of a Next.js project**

**Short Answer (30 seconds):**
A Next.js project is organized around convention-based routing and server-first architecture. In Next.js 16 with the App Router, the `app/` directory is the core of the application and contains routes, layouts, loading states, error boundaries, and server components, while folders like `public/`, `components/`, `lib/`, and `styles/` organize reusable assets and application logic.

---

# Detailed Explanation

## 1. High-Level Next.js 16 Project Structure

Typical production-grade structure:

```bash
my-app/
├── app/
├── components/
├── lib/
├── public/
├── styles/
├── types/
├── middleware.ts
├── next.config.ts
├── package.json
├── tsconfig.json
└── .env
```

In modern Next.js:

- `app/` is the most important directory
- Routing is filesystem-based
- Server Components are default
- Layouts and nested routing are built-in

Docs:

- [Project Structure Docs](https://nextjs.org/docs/app/getting-started/project-structure)

---

# 2. The `app/` Directory (Core of Next.js 16)

Docs:

- [App Router Docs](https://nextjs.org/docs/app)

The `app/` directory defines:

- Routes
- Layouts
- Pages
- Loading states
- Error boundaries
- API handlers

Example:

```bash
app/
├── layout.tsx
├── page.tsx
├── loading.tsx
├── error.tsx
├── products/
│   ├── page.tsx
│   ├── loading.tsx
│   └── [id]/
│       └── page.tsx
└── api/
    └── users/
        └── route.ts
```

---

# 3. Important Files Inside `app/`

---

## A. `page.tsx`

Defines a route.

Example:

```bash
app/about/page.tsx
```

Accessible at:

```bash
/about
```

Example:

```tsx
export default function AboutPage() {
  return <h1>About</h1>;
}
```

---

## B. `layout.tsx`

Defines shared UI.

Used for:

- Navigation
- Sidebars
- Persistent layouts

Example:

```tsx
// app/layout.tsx

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

Layouts persist across route transitions.

---

## C. `loading.tsx`

Automatically creates Suspense loading UI.

Example:

```tsx
export default function Loading() {
  return <p>Loading...</p>;
}
```

Used with streaming rendering.

Docs:

- [Loading UI Docs](https://nextjs.org/docs/app/building-your-application/routing/loading-ui-and-streaming)

---

## D. `error.tsx`

Route-level error boundary.

Example:

```tsx
"use client";

export default function Error({ error }: { error: Error }) {
  return <p>{error.message}</p>;
}
```

---

## E. `not-found.tsx`

Custom 404 UI.

```tsx
export default function NotFound() {
  return <h1>Page Not Found</h1>;
}
```

---

# 4. Dynamic Routes

Example:

```bash
app/blog/[slug]/page.tsx
```

Route:

```bash
/blog/react
```

Access params:

```tsx
type Props = {
  params: Promise<{
    slug: string;
  }>;
};

export default async function BlogPage({ params }: Props) {
  const { slug } = await params;

  return <h1>{slug}</h1>;
}
```

Docs:

- [Dynamic Routes Docs](https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes)

---

# 5. Route Groups

Used to organize routes without affecting URL structure.

Example:

```bash
app/
├── (marketing)/
├── (dashboard)/
```

Helps separate:

- Public pages
- Authenticated dashboards
- Admin sections

Without changing URLs.

---

# 6. API Route Structure

In App Router:

- APIs use `route.ts`

Example:

```bash
app/api/users/route.ts
```

```tsx
import { NextResponse } from "next/server";

export async function GET() {
  return NextResponse.json({
    users: [],
  });
}
```

Docs:

- [Route Handlers Docs](https://nextjs.org/docs/app/building-your-application/routing/route-handlers)

---

# 7. `components/` Directory

Contains reusable UI components.

Example:

```bash
components/
├── ui/
├── forms/
├── dashboard/
└── shared/
```

Best practice:

- Keep components small
- Separate Server and Client Components
- Use feature-based organization for large apps

Example:

```tsx
// components/ui/Button.tsx

type Props = {
  children: React.ReactNode;
};

export function Button({ children }: Props) {
  return (
    <button className="rounded bg-black px-4 py-2 text-white">
      {children}
    </button>
  );
}
```

---

# 8. `lib/` Directory

Stores:

- Database utilities
- API clients
- Auth helpers
- Server utilities

Example:

```bash
lib/
├── db.ts
├── auth.ts
├── fetcher.ts
└── utils.ts
```

Example:

```tsx
// lib/db.ts

import { PrismaClient } from "@prisma/client";

export const db = new PrismaClient();
```

---

# 9. `public/` Directory

Stores static assets.

Example:

```bash
public/
├── images/
├── icons/
└── fonts/
```

Access:

```bash
/images/logo.png
```

---

# 10. `styles/` Directory

Optional organization for:

- Global CSS
- Tailwind config styles
- CSS modules

Example:

```bash
styles/
├── globals.css
└── variables.css
```

Imported in:

```tsx
// app/layout.tsx

import "./globals.css";
```

---

# 11. `middleware.ts`

Runs before requests complete.

Used for:

- Authentication
- Redirects
- Geo routing
- Feature flags

Example:

```tsx
import { NextResponse } from "next/server";

export function middleware() {
  return NextResponse.next();
}
```

Docs:

- [Middleware Docs](https://nextjs.org/docs/app/building-your-application/routing/middleware)

---

# 12. Configuration Files

---

## `next.config.ts`

Framework configuration.

Example:

```tsx
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  images: {
    remotePatterns: [],
  },
};

export default nextConfig;
```

---

## `tsconfig.json`

TypeScript configuration.

---

## `.env`

Environment variables.

Example:

```bash
DATABASE_URL=
NEXTAUTH_SECRET=
```

---

# 13. Production-Grade Folder Structure

Large-scale enterprise example:

```bash
src/
├── app/
├── components/
├── features/
│   ├── auth/
│   ├── products/
│   └── checkout/
├── lib/
├── hooks/
├── services/
├── store/
├── types/
├── constants/
└── utils/
```

Feature-based architecture improves:

- Scalability
- Team collaboration
- Maintainability

---

# 14. Recommended Best Practices

## Prefer Feature-Based Organization

Good:

```bash
features/products/
```

Instead of:

```bash
all components in one folder
```

---

## Separate Server and Client Components

Example:

```bash
components/
├── server/
└── client/
```

---

## Keep Business Logic Outside Components

Move logic into:

- `lib/`
- `services/`
- `actions/`

---

## Co-Locate Route-Specific Components

Example:

```bash
app/dashboard/components/
```

For private route-specific UI.

---

# 15. Common Pitfalls

| Pitfall                    | Solution                     |
| -------------------------- | ---------------------------- |
| Huge `components/` folder  | Use feature-based structure  |
| Too many Client Components | Default to Server Components |
| Business logic inside UI   | Move to services/lib         |
| Flat folder structure      | Use domains/features         |
| Shared state everywhere    | Keep state localized         |

---

# 16. Advanced Next.js 16 Concepts

Modern folder structure also supports:

| Feature             | Folder Convention |
| ------------------- | ----------------- |
| Parallel Routes     | `@slot`           |
| Intercepting Routes | `(.)route`        |
| Route Groups        | `(group)`         |
| Server Actions      | colocated actions |
| Streaming           | `loading.tsx`     |

Docs:

- [Advanced Routing Docs](https://nextjs.org/docs/app/building-your-application/routing)

---

# Production Example Structure

```bash
app/
├── layout.tsx
├── page.tsx
├── dashboard/
│   ├── layout.tsx
│   ├── page.tsx
│   ├── loading.tsx
│   └── settings/
│       └── page.tsx
├── products/
│   ├── page.tsx
│   └── [id]/
│       └── page.tsx
└── api/
    └── auth/
        └── route.ts
```

This demonstrates:

- Nested layouts
- Route hierarchy
- Dynamic routing
- API handling
- Streaming support

---

# When to Use This Structure

This architecture is ideal for:
✅ Enterprise apps
✅ SaaS platforms
✅ E-commerce systems
✅ Multi-team development
✅ Large-scale React applications

---

# Alternatives

| Structure      | Best For             |
| -------------- | -------------------- |
| Flat structure | Small apps           |
| Feature-based  | Enterprise apps      |
| Domain-driven  | Large teams          |
| Monorepo       | Multi-app ecosystems |

---

# Final Interview Summary

> “In Next.js 16, the folder structure is convention-driven and centered around the `app/` directory. The App Router enables filesystem-based routing, nested layouts, streaming, route-level loading and error handling, and Server Components by default. A scalable production architecture typically separates reusable UI, business logic, utilities, and feature domains for maintainability and performance.”

## Question 8. What is the pages directory used for?

**Q: What is the `pages` directory used for in Next.js?**

**Short Answer (30 seconds):**
The `pages/` directory is the legacy routing system in Next.js used before the App Router (`app/`) was introduced. It enables file-based routing, API routes, SSR, SSG, and client-side navigation. While Next.js 16 recommends using the App Router for new applications, the `pages/` directory is still fully supported for backward compatibility and incremental migration.

---

# Detailed Explanation

## 1. What is the `pages/` Directory?

Before Next.js introduced the App Router, routing was based entirely on the `pages/` directory.

Example:

```bash
pages/
├── index.tsx
├── about.tsx
├── products/
│   └── [id].tsx
└── api/
    └── users.ts
```

Generated routes:

| File                      | Route           |
| ------------------------- | --------------- |
| `pages/index.tsx`         | `/`             |
| `pages/about.tsx`         | `/about`        |
| `pages/products/[id].tsx` | `/products/:id` |

Docs:

- [Pages Router Docs](https://nextjs.org/docs/pages)

---

# 2. Main Purpose of the `pages/` Directory

The `pages/` directory provides:

✅ File-based routing
✅ SSR support
✅ SSG support
✅ API routes
✅ Dynamic routing
✅ Client-side navigation
✅ Middleware compatibility

Historically, it was the core architecture of Next.js applications.

---

# 3. Example of a Basic Page

```tsx
// pages/about.tsx

export default function AboutPage() {
  return <h1>About Us</h1>;
}
```

Automatically available at:

```bash
/about
```

No manual router configuration required.

---

# 4. Dynamic Routes in `pages/`

Example:

```bash
pages/blog/[slug].tsx
```

URL:

```bash
/blog/react
```

Access params using:

```tsx
import { useRouter } from "next/router";

export default function BlogPost() {
  const router = useRouter();

  return <p>{router.query.slug}</p>;
}
```

Docs:

- [Dynamic Routes (Pages Router)](https://nextjs.org/docs/pages/building-your-application/routing/dynamic-routes)

---

# 5. SSR in `pages/`

SSR is implemented using:

```tsx
getServerSideProps;
```

Example:

```tsx
// pages/dashboard.tsx

type Props = {
  users: number;
};

export default function Dashboard({ users }: Props) {
  return <p>Users: {users}</p>;
}

export async function getServerSideProps() {
  const res = await fetch("https://api.example.com/users");

  const data = await res.json();

  return {
    props: {
      users: data.count,
    },
  };
}
```

This runs on every request.

Docs:

- [getServerSideProps Docs](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-server-side-props)

---

# 6. SSG in `pages/`

Implemented using:

```tsx
getStaticProps;
```

Example:

```tsx
// pages/blog.tsx

export default function Blog({ posts }: any) {
  return (
    <div>
      {posts.map((post: any) => (
        <p key={post.id}>{post.title}</p>
      ))}
    </div>
  );
}

export async function getStaticProps() {
  const res = await fetch("https://api.example.com/posts");

  const posts = await res.json();

  return {
    props: {
      posts,
    },
    revalidate: 60,
  };
}
```

This supports ISR as well.

Docs:

- [getStaticProps Docs](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-static-props)

---

# 7. API Routes in `pages/api`

The `pages/api` folder creates backend endpoints.

Example:

```bash
pages/api/users.ts
```

```tsx
import type { NextApiRequest, NextApiResponse } from "next";

export default function handler(req: NextApiRequest, res: NextApiResponse) {
  res.status(200).json({
    users: [],
  });
}
```

Route:

```bash
/api/users
```

Docs:

- [API Routes Docs](https://nextjs.org/docs/pages/building-your-application/routing/api-routes)

---

# 8. `_app.tsx`

Custom application wrapper.

Example:

```tsx
// pages/_app.tsx

import type { AppProps } from "next/app";

export default function App({ Component, pageProps }: AppProps) {
  return <Component {...pageProps} />;
}
```

Used for:

- Providers
- Global layouts
- Authentication wrappers

---

# 9. `_document.tsx`

Custom HTML document structure.

Example:

```tsx
// pages/_document.tsx

import { Html, Head, Main, NextScript } from "next/document";

export default function Document() {
  return (
    <Html>
      <Head />
      <body>
        <Main />
        <NextScript />
      </body>
    </Html>
  );
}
```

Used for:

- HTML customization
- Font loading
- Meta tags

---

# 10. `pages/` vs `app/` Directory

This comparison is very important in interviews.

| Feature                   | `pages/` Router      | `app/` Router           |
| ------------------------- | -------------------- | ----------------------- |
| Introduced                | Older architecture   | Modern architecture     |
| Routing                   | File-based           | File-based              |
| Layouts                   | Manual               | Nested layouts          |
| Server Components         | ❌                   | ✅                      |
| Streaming                 | Limited              | Built-in                |
| Loading UI                | Manual               | `loading.tsx`           |
| Error Boundaries          | Manual               | `error.tsx`             |
| Data Fetching             | `getServerSideProps` | Native async components |
| Recommended in Next.js 16 | Legacy               | Preferred               |

---

# 11. Why App Router Replaced `pages/`

The Pages Router had limitations:

- No nested layouts
- No React Server Components
- More client-side JavaScript
- Less efficient rendering
- More boilerplate

The App Router introduced:
✅ Server-first rendering
✅ Streaming
✅ Better layouts
✅ Colocated data fetching
✅ Improved performance

Docs:

- [App Router Migration Docs](https://nextjs.org/docs/app/guides/migrating/app-router-migration)

---

# 12. Can `pages/` and `app/` Coexist?

Yes.

Next.js supports incremental migration.

Example:

```bash
app/
pages/
```

Both can run together.

Priority:

- App Router routes take precedence if overlapping.

This is heavily used in enterprise migrations.

---

# 13. Production-Level `pages/` Structure

```bash
pages/
├── _app.tsx
├── _document.tsx
├── index.tsx
├── dashboard.tsx
├── products/
│   ├── index.tsx
│   └── [id].tsx
└── api/
    ├── auth.ts
    └── users.ts
```

---

# 14. Common Pitfalls

| Pitfall                                       | Solution                            |
| --------------------------------------------- | ----------------------------------- |
| Excessive `getServerSideProps` usage          | Prefer static generation            |
| Global layouts in `_app.tsx` becoming complex | Migrate to App Router               |
| Large client bundles                          | Use Server Components in App Router |
| API route monoliths                           | Modularize backend logic            |
| Hydration-heavy apps                          | Move to server-first architecture   |

---

# 15. Senior-Level Interview Insight

Strong answer:

> “The `pages/` directory represents the original Pages Router architecture of Next.js, while the `app/` directory represents the modern React Server Components-based architecture.”

Another excellent insight:

> “The App Router reduces the need for special data-fetching functions like `getServerSideProps` because async Server Components handle data fetching naturally.”

---

# 16. When Should You Still Use `pages/`?

Still valid for:
✅ Legacy applications
✅ Incremental migration
✅ Existing enterprise codebases
✅ Teams not yet migrated to App Router

But for new apps:
✅ Prefer App Router (`app/`)

---

# Alternatives

| Approach     | Best Use                    |
| ------------ | --------------------------- |
| Pages Router | Legacy compatibility        |
| App Router   | Modern Next.js architecture |
| React Router | Pure React SPA              |
| Remix        | Nested routing architecture |

---

# Final Interview Summary

> “The `pages/` directory is the legacy routing system in Next.js that enables file-based routing, SSR, SSG, API routes, and dynamic routing. It powers the Pages Router architecture using functions like `getServerSideProps` and `getStaticProps`. While still fully supported in Next.js 16, modern applications typically prefer the App Router because it introduces React Server Components, streaming, nested layouts, and server-first rendering.”

## Question 9. What is the public folder in Next.js?

**Q: What is the `public` folder in Next.js?**

**Short Answer (30 seconds):**
The `public/` folder in Next.js is used to store static assets such as images, fonts, icons, videos, PDFs, and other files that should be served directly by the browser without processing by the build system. Files inside `public/` are accessible from the root URL path of the application.

---

# Detailed Explanation

## 1. What is the `public/` Folder?

The `public/` directory is a special folder in Next.js for static assets.

Example structure:

```bash
public/
├── images/
│   └── logo.png
├── icons/
│   └── favicon.ico
├── fonts/
├── videos/
└── documents/
```

Files inside `public/` are served directly by Next.js.

Example:

```bash
public/images/logo.png
```

Accessible at:

```bash
/images/logo.png
```

Docs:

- [Static Assets Docs](https://nextjs.org/docs/app/building-your-application/optimizing/static-assets)

---

# 2. How the `public/` Folder Works

Next.js maps:

```bash
public/*
```

Directly to:

```bash
/*
```

Meaning:

- The `public` name is not included in URLs.

Example:

| File                     | URL                |
| ------------------------ | ------------------ |
| `public/logo.png`        | `/logo.png`        |
| `public/images/hero.jpg` | `/images/hero.jpg` |

---

# 3. Common Assets Stored in `public/`

Typically used for:

✅ Images
✅ Favicon
✅ Fonts
✅ Robots.txt
✅ Sitemap.xml
✅ PDFs
✅ Videos
✅ Static JSON files

Example:

```bash
public/
├── favicon.ico
├── robots.txt
├── sitemap.xml
└── brochure.pdf
```

---

# 4. Using Images from `public/`

## With HTML `<img>`

```tsx
<img src="/images/logo.png" alt="Logo" />
```

---

## With `next/image` (Recommended)

Docs:

- [Image Optimization Docs](https://nextjs.org/docs/app/building-your-application/optimizing/images)

```tsx
import Image from "next/image";

export default function Header() {
  return <Image src="/images/logo.png" alt="Logo" width={200} height={80} />;
}
```

Benefits:
✅ Automatic optimization
✅ Lazy loading
✅ Responsive images
✅ Better Core Web Vitals

---

# 5. Favicon Example

Place:

```bash
public/favicon.ico
```

Browser automatically loads:

```bash
/favicon.ico
```

Or in App Router:

```tsx
// app/layout.tsx

export const metadata = {
  icons: {
    icon: "/favicon.ico",
  },
};
```

---

# 6. Fonts in `public/`

Example:

```bash
public/fonts/Inter-Regular.woff2
```

Use in CSS:

```css
@font-face {
  font-family: "Inter";
  src: url("/fonts/Inter-Regular.woff2");
}
```

However, modern Next.js prefers:

```tsx
next / font;
```

Docs:

- [Font Optimization Docs](https://nextjs.org/docs/app/building-your-application/optimizing/fonts)

---

# 7. SEO Files in `public/`

## robots.txt

```bash
public/robots.txt
```

Accessible:

```bash
/robots.txt
```

---

## sitemap.xml

```bash
public/sitemap.xml
```

Accessible:

```bash
/sitemap.xml
```

Important for SEO and indexing.

---

# 8. Downloadable Files

Example:

```bash
public/resume.pdf
```

Download link:

```tsx
<a href="/resume.pdf" download>
  Download Resume
</a>
```

---

# 9. `public/` vs Importing Assets

This is commonly discussed in interviews.

---

## A. Using `public/`

```tsx
src = "/images/logo.png";
```

Good for:

- Static references
- SEO assets
- Publicly accessible files

---

## B. Importing Assets

```tsx
import logo from "@/assets/logo.png";
```

Good for:

- Tree shaking
- Build optimization
- Type safety
- Bundled assets

---

# 10. Important Behavior of `public/`

Files in `public/`:

- Are NOT bundled by webpack/turbopack
- Are NOT hashed automatically
- Are served as-is

This means:

- Browser caching matters
- Cache invalidation must be managed carefully

---

# 11. Performance Considerations

## Avoid Huge Unoptimized Files

Bad:

```bash
public/videos/4k-video.mov
```

Can slow page load dramatically.

---

## Prefer `next/image`

Instead of raw `<img>` whenever possible.

---

## Use CDN for Large Assets

For:

- Large videos
- Massive downloads
- Enterprise media libraries

---

# 12. Production-Level Example

```bash
public/
├── images/
│   ├── hero.webp
│   ├── logo.svg
│   └── avatars/
├── fonts/
├── icons/
├── videos/
├── robots.txt
├── sitemap.xml
└── manifest.json
```

---

# 13. Common Pitfalls

| Pitfall                           | Solution                                     |
| --------------------------------- | -------------------------------------------- |
| Storing huge media files          | Use external storage/CDN                     |
| Using `<img>` everywhere          | Prefer `next/image`                          |
| Expecting hashing/versioning      | Use imported assets when needed              |
| Sensitive files in `public/`      | Never store secrets/publicly accessible data |
| Deep nested assets becoming messy | Organize by domain/type                      |

---

# 14. Security Considerations

Everything in `public/` is publicly accessible.

Never store:
❌ API keys
❌ Secrets
❌ Private documents
❌ Internal configs

Good rule:

> If it’s in `public/`, assume anyone can access it.

---

# 15. App Router Integration

In Next.js 16:

- `public/` works identically with App Router
- Assets integrate naturally with:
  - Metadata API
  - `next/image`
  - Streaming layouts

Example:

```tsx
export const metadata = {
  openGraph: {
    images: ["/images/og-banner.png"],
  },
};
```

---

# 16. Senior-Level Interview Insight

Strong answer:

> “The `public/` folder bypasses the bundling pipeline and exposes static assets directly at the application root.”

Another good insight:

> “Unlike imported assets, files in `public/` are not fingerprinted or hashed, so cache management becomes a production consideration.”

---

# When to Use `public/`

Use `public/` for:
✅ Favicons
✅ Robots.txt
✅ Sitemap.xml
✅ Static downloadable files
✅ Public images/icons
✅ Manifest files
✅ SEO assets

---

# Alternatives

| Approach      | Best Use                 |
| ------------- | ------------------------ |
| `public/`     | Static public assets     |
| Asset imports | Bundled optimized assets |
| CDN           | Large media delivery     |
| Cloud storage | Scalable file hosting    |

---

# Final Interview Summary

> “The `public/` folder in Next.js is used for serving static assets directly from the application root. It’s ideal for images, icons, fonts, SEO files, and downloadable resources that don’t need processing by the bundler. In production, developers should combine `public/` with Next.js optimizations like `next/image` and external CDNs for scalability and performance.”

## Question 10. How does Next.js handle routing?

**Q: How does Next.js handle routing?**

**Short Answer (30 seconds):**
Next.js uses a filesystem-based routing system where folders and files automatically become application routes. In Next.js 16, the App Router (`app/` directory) is the modern routing architecture that supports nested layouts, dynamic routes, streaming, loading states, route groups, parallel routes, and React Server Components by default.

---

# Detailed Explanation

## 1. What is Routing in Next.js?

Routing determines:

- Which UI renders for a URL
- How navigation works
- How layouts are shared
- How dynamic pages behave

Unlike React Router, Next.js routing is:
✅ Convention-based
✅ Automatic
✅ File-system driven

Docs:

- [App Router Docs](https://nextjs.org/docs/app/building-your-application/routing)

---

# 2. File-System Based Routing

In Next.js:

- Folders define URL segments
- `page.tsx` defines the route UI

Example:

```bash
app/
├── page.tsx
├── about/
│   └── page.tsx
├── products/
│   └── page.tsx
```

Generated routes:

| File                    | Route       |
| ----------------------- | ----------- |
| `app/page.tsx`          | `/`         |
| `app/about/page.tsx`    | `/about`    |
| `app/products/page.tsx` | `/products` |

No manual route configuration required.

---

# 3. Basic Route Example

```tsx
// app/about/page.tsx

export default function AboutPage() {
  return <h1>About Page</h1>;
}
```

Automatically accessible at:

```bash
/about
```

---

# 4. Nested Routing

Folders create nested routes naturally.

Example:

```bash
app/
├── dashboard/
│   ├── page.tsx
│   └── settings/
│       └── page.tsx
```

Routes:

```bash
/dashboard
/dashboard/settings
```

This scales extremely well for enterprise applications.

---

# 5. Dynamic Routes

Dynamic segments use square brackets.

Example:

```bash
app/blog/[slug]/page.tsx
```

Matches:

```bash
/blog/react
/blog/nextjs
```

Access params:

```tsx
type Props = {
  params: Promise<{
    slug: string;
  }>;
};

export default async function BlogPage({ params }: Props) {
  const { slug } = await params;

  return <h1>{slug}</h1>;
}
```

Docs:

- [Dynamic Routes Docs](https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes)

---

# 6. Catch-All Routes

Used for flexible nested URLs.

Example:

```bash
app/docs/[...slug]/page.tsx
```

Matches:

```bash
/docs/react
/docs/react/hooks
/docs/react/hooks/useEffect
```

Params:

```tsx
{
  slug: ["react", "hooks"];
}
```

---

# 7. Optional Catch-All Routes

Example:

```bash
app/docs/[[...slug]]/page.tsx
```

Matches:

```bash
/docs
/docs/react
/docs/react/hooks
```

---

# 8. Layouts in Routing

One of the biggest App Router features.

Example:

```bash
app/
├── layout.tsx
├── dashboard/
│   ├── layout.tsx
│   └── page.tsx
```

Root layout wraps the entire app.

Nested layout wraps only dashboard routes.

Example:

```tsx
// app/dashboard/layout.tsx

export default function DashboardLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <div>
      <aside>Sidebar</aside>
      <main>{children}</main>
    </div>
  );
}
```

Benefits:
✅ Persistent UI
✅ Shared navigation
✅ Reduced re-renders

Docs:

- [Layouts Docs](https://nextjs.org/docs/app/building-your-application/routing/layouts-and-templates)

---

# 9. Loading States

Each route can define:

```bash
loading.tsx
```

Example:

```tsx
export default function Loading() {
  return <p>Loading...</p>;
}
```

Works automatically with streaming.

---

# 10. Error Boundaries

Each route can define:

```bash
error.tsx
```

Example:

```tsx
"use client";

export default function Error({ error }: { error: Error }) {
  return <p>{error.message}</p>;
}
```

Provides route-level error isolation.

---

# 11. Route Groups

Used for organization without affecting URLs.

Example:

```bash
app/
├── (marketing)/
├── (dashboard)/
```

URL does NOT include group names.

Benefits:

- Separate layouts
- Team/domain organization
- Cleaner architecture

---

# 12. Parallel Routes

Advanced App Router feature.

Example:

```bash
app/
├── @analytics/
├── @team/
```

Allows rendering multiple route trees simultaneously.

Used in:

- Dashboards
- Complex admin panels
- Multi-pane UIs

Docs:

- [Parallel Routes Docs](https://nextjs.org/docs/app/building-your-application/routing/parallel-routes)

---

# 13. Intercepting Routes

Allows overlay navigation patterns.

Example:

```bash
(.)photo
```

Used for:

- Modals
- Overlay navigation
- Advanced UX patterns

---

# 14. Client Navigation

Next.js uses:

```tsx
import Link from "next/link";
```

Example:

```tsx
import Link from "next/link";

export default function Home() {
  return <Link href="/about">About</Link>;
}
```

Benefits:
✅ Prefetching
✅ SPA navigation
✅ Faster transitions

Docs:

- [Link Component Docs](https://nextjs.org/docs/app/api-reference/components/link)

---

# 15. Programmatic Navigation

Using:

```tsx
"use client";

import { useRouter } from "next/navigation";
```

Example:

```tsx
"use client";

import { useRouter } from "next/navigation";

export default function LoginButton() {
  const router = useRouter();

  return <button onClick={() => router.push("/dashboard")}>Login</button>;
}
```

---

# 16. API Routing

In App Router:

```bash
app/api/users/route.ts
```

Example:

```tsx
import { NextResponse } from "next/server";

export async function GET() {
  return NextResponse.json({
    users: [],
  });
}
```

This creates:

```bash
/api/users
```

---

# 17. Middleware-Based Routing

Using:

```bash
middleware.ts
```

Can:

- Redirect
- Rewrite
- Authenticate
- Geo-route

Example:

```tsx
import { NextResponse } from "next/server";

export function middleware() {
  return NextResponse.redirect(new URL("/login", request.url));
}
```

Docs:

- [Middleware Docs](https://nextjs.org/docs/app/building-your-application/routing/middleware)

---

# 18. App Router vs Pages Router

| Feature           | Pages Router      | App Router              |
| ----------------- | ----------------- | ----------------------- |
| Routing           | File-based        | File-based              |
| Layouts           | Manual            | Nested                  |
| Streaming         | Limited           | Built-in                |
| Server Components | ❌                | ✅                      |
| Loading UI        | Manual            | `loading.tsx`           |
| Error Handling    | Manual            | `error.tsx`             |
| Data Fetching     | Special functions | Native async components |

---

# 19. Production-Level Routing Structure

```bash
app/
├── layout.tsx
├── page.tsx
├── dashboard/
│   ├── layout.tsx
│   ├── page.tsx
│   ├── analytics/
│   │   └── page.tsx
│   └── settings/
│       └── page.tsx
├── products/
│   ├── page.tsx
│   └── [id]/
│       └── page.tsx
└── api/
    └── auth/
        └── route.ts
```

Demonstrates:

- Nested routes
- Dynamic routes
- Shared layouts
- APIs
- Scalability

---

# 20. Common Pitfalls

| Pitfall                                   | Solution                      |
| ----------------------------------------- | ----------------------------- |
| Overusing Client Components               | Keep routing server-first     |
| Huge layout trees                         | Split layouts logically       |
| Deep nested routes becoming messy         | Use route groups              |
| Client-side fetching everywhere           | Fetch in Server Components    |
| Using Pages Router patterns in App Router | Follow App Router conventions |

---

# 21. Advanced Interview Insight

Strong senior-level answer:

> “Next.js routing is not just URL mapping anymore — in the App Router, routing also defines rendering boundaries, layouts, streaming behavior, loading states, and data-fetching architecture.”

Another excellent insight:

> “The App Router tightly integrates routing with React Server Components and Suspense.”

That demonstrates deep architectural understanding.

---

# When to Use Next.js Routing

Ideal for:
✅ Enterprise apps
✅ SaaS dashboards
✅ E-commerce
✅ CMS systems
✅ Multi-layout applications
✅ Streaming UIs

---

# Alternatives

| Technology     | Comparison                  |
| -------------- | --------------------------- |
| React Router   | Manual SPA routing          |
| Remix          | Nested routing with loaders |
| Angular Router | Enterprise routing          |
| Nuxt Router    | Vue equivalent              |

---

# Final Interview Summary

> “Next.js handles routing through a filesystem-based architecture where folders and files automatically map to URLs. In Next.js 16, the App Router extends routing beyond navigation by integrating layouts, React Server Components, streaming, loading states, error boundaries, dynamic routes, and server-side rendering into a unified server-first architecture.”

## Question 11. What is a dynamic route in Next.js?

**Q: What is a dynamic route in Next.js?**

**Short Answer (30 seconds):**
A dynamic route in Next.js allows pages to handle variable URL segments using bracket syntax like `[id]` or `[slug]`. It enables building scalable applications where a single route template can render different content dynamically, such as product pages, blog posts, user profiles, or dashboards.

---

# Detailed Explanation

## 1. What is a Dynamic Route?

Dynamic routes are routes whose URL segments are not fixed.

Example:

```text
/products/1
/products/2
/products/3
```

Instead of creating:

```bash
products/1/page.tsx
products/2/page.tsx
products/3/page.tsx
```

We create one dynamic route:

```bash
app/products/[id]/page.tsx
```

The `[id]` segment becomes a route parameter.

Docs:

- [Dynamic Routes Docs](https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes)

---

# 2. Dynamic Route Syntax

In the App Router:

```bash
app/blog/[slug]/page.tsx
```

Examples matched:

```bash
/blog/react
/blog/nextjs
/blog/typescript
```

Here:

```text
slug = react
slug = nextjs
slug = typescript
```

---

# 3. Accessing Route Parameters

In Next.js 16 App Router:

```tsx
// app/blog/[slug]/page.tsx

type Props = {
  params: Promise<{
    slug: string;
  }>;
};

export default async function BlogPage({ params }: Props) {
  const { slug } = await params;

  return <h1>Blog: {slug}</h1>;
}
```

Important:

- `params` are provided automatically by Next.js
- Route params are available server-side
- Perfect for Server Components and SSR

---

# 4. Real-World Use Cases

Dynamic routes are heavily used for:

✅ Product pages
✅ Blog posts
✅ User profiles
✅ Dashboards
✅ CMS content
✅ Multi-tenant apps
✅ Category pages

Examples:

```text
/products/iphone-15
/users/john
/blog/react-server-components
/company/openai
```

---

# 5. Nested Dynamic Routes

Dynamic segments can be nested.

Example:

```bash
app/
└── shop/
    └── [category]/
        └── [productId]/
            └── page.tsx
```

URL:

```text
/shop/electronics/123
```

Params:

```tsx
{
  category: 'electronics',
  productId: '123'
}
```

---

# 6. Catch-All Routes

Catch-all routes match multiple segments.

Syntax:

```bash
[...slug]
```

Example:

```bash
app/docs/[...slug]/page.tsx
```

Matches:

```text
/docs/react
/docs/react/hooks
/docs/react/hooks/useEffect
```

Params:

```tsx
{
  slug: ["react", "hooks"];
}
```

---

# 7. Optional Catch-All Routes

Syntax:

```bash
[[...slug]]
```

Matches:

```text
/docs
/docs/react
/docs/react/hooks
```

This allows optional nested segments.

---

# 8. Dynamic Routes + Data Fetching

Dynamic routes are commonly combined with database/API fetching.

Example:

```tsx
// app/products/[id]/page.tsx

type Product = {
  id: number;
  title: string;
};

async function getProduct(id: string): Promise<Product> {
  const res = await fetch(`https://api.example.com/products/${id}`);

  if (!res.ok) {
    throw new Error("Product not found");
  }

  return res.json();
}

type Props = {
  params: Promise<{
    id: string;
  }>;
};

export default async function ProductPage({ params }: Props) {
  const { id } = await params;

  const product = await getProduct(id);

  return (
    <div>
      <h1>{product.title}</h1>
    </div>
  );
}
```

---

# 9. Static Generation with Dynamic Routes

Dynamic routes can still be statically generated.

Using:

```tsx
generateStaticParams();
```

Example:

```tsx
// app/blog/[slug]/page.tsx

export async function generateStaticParams() {
  const posts = await fetch("https://api.example.com/posts").then((res) =>
    res.json(),
  );

  return posts.map((post: any) => ({
    slug: post.slug,
  }));
}
```

This prebuilds pages at build time.

Docs:

- [generateStaticParams Docs](https://nextjs.org/docs/app/api-reference/functions/generate-static-params)

---

# 10. Dynamic Routes + ISR

Dynamic routes work extremely well with ISR.

Example:

```tsx
fetch(url, {
  next: {
    revalidate: 60,
  },
});
```

Benefits:
✅ Static performance
✅ Dynamic scalability
✅ Automatic updates

Perfect for:

- Product catalogs
- CMS pages
- Marketplace listings

---

# 11. Dynamic Routes in Pages Router (Legacy)

Older Pages Router syntax:

```bash
pages/blog/[slug].tsx
```

Access params using:

```tsx
import { useRouter } from "next/router";

const router = useRouter();

router.query.slug;
```

Modern App Router uses:

```tsx
params;
```

instead.

---

# 12. Dynamic Routing and SEO

Dynamic routes are SEO-friendly because:

- Each URL becomes indexable
- SSR/SSG works naturally
- Metadata can be generated dynamically

Example:

```tsx
export async function generateMetadata({ params }: Props) {
  return {
    title: `Product ${params.id}`,
  };
}
```

Docs:

- [Metadata API Docs](https://nextjs.org/docs/app/building-your-application/optimizing/metadata)

---

# 13. Production-Level Dynamic Route Example

```tsx
// app/blog/[slug]/page.tsx

type Post = {
  title: string;
  content: string;
};

async function getPost(slug: string): Promise<Post> {
  const res = await fetch(`https://cms.example.com/posts/${slug}`, {
    next: {
      revalidate: 3600,
    },
  });

  if (!res.ok) {
    throw new Error("Post not found");
  }

  return res.json();
}

type Props = {
  params: Promise<{
    slug: string;
  }>;
};

export default async function BlogPost({ params }: Props) {
  const { slug } = await params;

  const post = await getPost(slug);

  return (
    <article className="prose mx-auto">
      <h1>{post.title}</h1>
      <div>{post.content}</div>
    </article>
  );
}
```

Production-ready features:
✅ Server rendering
✅ ISR
✅ Dynamic params
✅ SEO-friendly
✅ Cacheable

---

# 14. Common Pitfalls

| Pitfall                                     | Solution                         |
| ------------------------------------------- | -------------------------------- |
| Using client-side fetching unnecessarily    | Fetch in Server Components       |
| Missing `generateStaticParams` for SSG      | Prebuild dynamic pages           |
| Deeply nested dynamic routes becoming messy | Use route groups                 |
| Not handling missing data                   | Use `notFound()`                 |
| Overusing catch-all routes                  | Keep route hierarchy predictable |

---

# 15. Handling 404s in Dynamic Routes

Example:

```tsx
import { notFound } from "next/navigation";

if (!product) {
  notFound();
}
```

Automatically renders:

```bash
not-found.tsx
```

---

# 16. Advanced Interview Insight

Strong senior-level answer:

> “Dynamic routes in Next.js are filesystem-based parameterized routes that integrate directly with Server Components, static generation, ISR, and metadata generation.”

Another excellent insight:

> “Dynamic routing in the App Router is tightly coupled with rendering strategy selection.”

That shows deeper framework understanding.

---

# Dynamic Routes vs Query Parameters

| Dynamic Route           | Query Param            |
| ----------------------- | ---------------------- |
| `/products/1`           | `/products?id=1`       |
| Better SEO              | Less SEO-friendly      |
| Cleaner URLs            | More flexible filters  |
| Preferred for resources | Preferred for UI state |

---

# When to Use Dynamic Routes

Use dynamic routes for:
✅ Resource pages
✅ SEO-friendly URLs
✅ CMS-driven content
✅ Product catalogs
✅ User profiles
✅ Tenant-specific routes

---

# Alternatives

| Approach            | Best Use                 |
| ------------------- | ------------------------ |
| Dynamic Routes      | Structured resource URLs |
| Query Params        | Filters/sorting          |
| Catch-All Routes    | Nested docs/navigation   |
| Middleware Rewrites | Advanced routing logic   |

---

# Final Interview Summary

> “Dynamic routes in Next.js allow variable URL segments using bracket-based filesystem routing like `[id]` or `[slug]`. They enable scalable, SEO-friendly applications where one route template dynamically renders multiple resources such as products, blog posts, or user profiles. In Next.js 16, dynamic routes integrate deeply with Server Components, ISR, metadata generation, and streaming architecture.”

## Question 12. How do you create a dynamic route?

**Q: How do you create a dynamic route in Next.js?**

**Short Answer (30 seconds):**
In Next.js, dynamic routes are created by wrapping a folder name in square brackets, such as `[id]` or `[slug]`. The value from the URL becomes a route parameter that can be accessed through the `params` object in App Router pages and layouts.

---

# Detailed Explanation

## 1. Creating a Basic Dynamic Route

In Next.js 16 App Router, dynamic routes are created using:

```bash
[paramName]
```

Example:

```bash
app/products/[id]/page.tsx
```

This route matches:

```text
/products/1
/products/42
/products/iphone-15
```

Docs:

- [Dynamic Routes Docs](https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes)

---

# 2. Route Structure Example

Project structure:

```bash
app/
├── products/
│   └── [id]/
│       └── page.tsx
```

Generated route:

```text
/products/:id
```

---

# 3. Accessing Dynamic Parameters

Example:

```tsx
// app/products/[id]/page.tsx

type Props = {
  params: Promise<{
    id: string;
  }>;
};

export default async function ProductPage({ params }: Props) {
  const { id } = await params;

  return <h1>Product ID: {id}</h1>;
}
```

If the URL is:

```text
/products/123
```

Then:

```tsx
id === "123";
```

---

# 4. Real-World Example (Product Page)

Dynamic routes are commonly used for:

- Products
- Blogs
- Users
- CMS content

Example:

```tsx
// app/blog/[slug]/page.tsx

type Post = {
  title: string;
  content: string;
};

async function getPost(slug: string): Promise<Post> {
  const res = await fetch(`https://api.example.com/posts/${slug}`);

  if (!res.ok) {
    throw new Error("Post not found");
  }

  return res.json();
}

type Props = {
  params: Promise<{
    slug: string;
  }>;
};

export default async function BlogPost({ params }: Props) {
  const { slug } = await params;

  const post = await getPost(slug);

  return (
    <article>
      <h1>{post.title}</h1>
      <p>{post.content}</p>
    </article>
  );
}
```

Example URLs:

```text
/blog/react
/blog/nextjs
/blog/server-components
```

---

# 5. Nested Dynamic Routes

You can combine multiple dynamic segments.

Example:

```bash
app/
└── shop/
    └── [category]/
        └── [productId]/
            └── page.tsx
```

URL:

```text
/shop/electronics/123
```

Access:

```tsx
{
  category: 'electronics',
  productId: '123'
}
```

---

# 6. Catch-All Dynamic Routes

Use:

```bash
[...slug]
```

Example:

```bash
app/docs/[...slug]/page.tsx
```

Matches:

```text
/docs/react
/docs/react/hooks
/docs/react/hooks/useEffect
```

Params:

```tsx
{
  slug: ["react", "hooks"];
}
```

Useful for:

- Documentation systems
- Nested CMS routes
- File explorers

---

# 7. Optional Catch-All Routes

Syntax:

```bash
[[...slug]]
```

Matches:

```text
/docs
/docs/react
/docs/react/hooks
```

This makes the parameter optional.

---

# 8. Static Generation for Dynamic Routes

Using:

```tsx
generateStaticParams();
```

Example:

```tsx
// app/blog/[slug]/page.tsx

export async function generateStaticParams() {
  const posts = await fetch("https://api.example.com/posts").then((res) =>
    res.json(),
  );

  return posts.map((post: any) => ({
    slug: post.slug,
  }));
}
```

Benefits:
✅ Build-time generation
✅ Better SEO
✅ Faster performance

Docs:

- [generateStaticParams Docs](https://nextjs.org/docs/app/api-reference/functions/generate-static-params)

---

# 9. Dynamic Metadata

Dynamic routes often generate metadata dynamically.

Example:

```tsx
import type { Metadata } from "next";

type Props = {
  params: Promise<{
    slug: string;
  }>;
};

export async function generateMetadata({ params }: Props): Promise<Metadata> {
  const { slug } = await params;

  return {
    title: `Blog - ${slug}`,
  };
}
```

This improves SEO significantly.

Docs:

- [Metadata API Docs](https://nextjs.org/docs/app/building-your-application/optimizing/metadata)

---

# 10. Dynamic Routes + ISR

Dynamic pages can use ISR.

Example:

```tsx
fetch(url, {
  next: {
    revalidate: 3600,
  },
});
```

Benefits:
✅ Static performance
✅ Fresh content
✅ CDN caching

Perfect for:

- Product catalogs
- Blog systems
- CMS pages

---

# 11. Client Navigation to Dynamic Routes

Using:

```tsx
import Link from "next/link";
```

Example:

```tsx
<Link href={`/products/${product.id}`}>View Product</Link>
```

Benefits:
✅ Prefetching
✅ SPA navigation
✅ Faster transitions

---

# 12. Handling Missing Dynamic Routes

Use:

```tsx
import { notFound } from "next/navigation";
```

Example:

```tsx
if (!product) {
  notFound();
}
```

Automatically renders:

```bash
not-found.tsx
```

---

# 13. Production-Level Example

```tsx
// app/products/[id]/page.tsx

import { notFound } from "next/navigation";

type Product = {
  id: number;
  title: string;
  description: string;
};

async function getProduct(id: string): Promise<Product | null> {
  const res = await fetch(`https://api.example.com/products/${id}`, {
    next: {
      revalidate: 3600,
    },
  });

  if (!res.ok) {
    return null;
  }

  return res.json();
}

type Props = {
  params: Promise<{
    id: string;
  }>;
};

export default async function ProductPage({ params }: Props) {
  const { id } = await params;

  const product = await getProduct(id);

  if (!product) {
    notFound();
  }

  return (
    <main className="p-8">
      <h1 className="text-3xl font-bold">{product.title}</h1>

      <p className="mt-4">{product.description}</p>
    </main>
  );
}
```

Production features:
✅ ISR
✅ Error handling
✅ SEO-friendly
✅ Dynamic rendering
✅ Server Components

---

# 14. Common Pitfalls

| Pitfall                                                  | Solution                     |
| -------------------------------------------------------- | ---------------------------- |
| Using query params instead of route params unnecessarily | Prefer clean URLs            |
| Client-side fetching for SEO pages                       | Fetch on server              |
| Missing `notFound()` handling                            | Handle invalid routes        |
| Overusing catch-all routes                               | Keep routing predictable     |
| Forgetting static generation                             | Use `generateStaticParams()` |

---

# 15. App Router vs Pages Router Dynamic Routes

| Feature       | Pages Router          | App Router       |
| ------------- | --------------------- | ---------------- |
| Param Access  | `useRouter()`         | `params`         |
| Data Fetching | `getStaticProps`      | Async components |
| Rendering     | Mostly client-centric | Server-first     |
| Recommended   | Legacy                | Modern           |

---

# 16. Advanced Interview Insight

Strong senior-level statement:

> “Dynamic routes in Next.js are filesystem-based parameterized routes that integrate directly with rendering strategies like SSR, SSG, and ISR.”

Another strong insight:

> “In the App Router, dynamic routes are server-first and colocate routing with data fetching and metadata generation.”

That demonstrates deep architectural understanding.

---

# When to Use Dynamic Routes

Use dynamic routes for:
✅ Product pages
✅ Blog systems
✅ User profiles
✅ CMS content
✅ Marketplace listings
✅ Multi-tenant applications

---

# Alternatives

| Approach            | Best Use         |
| ------------------- | ---------------- |
| Dynamic Routes      | Resource pages   |
| Query Params        | Filtering/search |
| Catch-All Routes    | Nested docs      |
| Middleware Rewrites | Advanced routing |

---

# Final Interview Summary

> “To create a dynamic route in Next.js, you wrap a folder name in square brackets like `[id]` or `[slug]`. The value from the URL becomes available through the `params` object. Dynamic routes are commonly used for products, blogs, and CMS content, and in Next.js 16 they integrate deeply with Server Components, ISR, metadata generation, and server-first rendering.”

## Question 13. What is a catch-all route?

**Q: What is a catch-all route in Next.js?**

**Short Answer (30 seconds):**
A catch-all route in Next.js is a dynamic route that matches multiple URL segments using the `[...param]` syntax. It allows a single route to handle deeply nested or variable-length paths, making it useful for documentation systems, CMS-driven pages, file explorers, and complex nested navigation structures.

---

# Detailed Explanation

## 1. What is a Catch-All Route?

Normally, a dynamic route captures only one segment.

Example:

```bash
app/blog/[slug]/page.tsx
```

Matches:

```text
/blog/react
```

But NOT:

```text
/blog/react/hooks/useEffect
```

To match multiple segments, use a catch-all route.

---

# 2. Catch-All Route Syntax

Syntax:

```bash
[...param]
```

Example:

```bash
app/docs/[...slug]/page.tsx
```

Docs:

- [Dynamic Routes Docs](https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes#catch-all-segments)

---

# 3. What URLs Does It Match?

This route:

```bash
app/docs/[...slug]/page.tsx
```

Matches:

```text
/docs/react
/docs/react/hooks
/docs/react/hooks/useEffect
/docs/nextjs/app-router/routing
```

---

# 4. How Parameters Work

Catch-all params are returned as arrays.

Example:

```text
/docs/react/hooks
```

Becomes:

```tsx
{
  slug: ["react", "hooks"];
}
```

---

# 5. Example in Next.js 16 App Router

```tsx
// app/docs/[...slug]/page.tsx

type Props = {
  params: Promise<{
    slug: string[];
  }>;
};

export default async function DocsPage({ params }: Props) {
  const { slug } = await params;

  return (
    <div>
      <h1>{slug.join(" / ")}</h1>
    </div>
  );
}
```

URL:

```text
/docs/react/hooks/useEffect
```

Renders:

```text
react / hooks / useEffect
```

---

# 6. Real-World Use Cases

Catch-all routes are ideal for:

✅ Documentation systems
✅ CMS-driven nested pages
✅ File explorers
✅ Category hierarchies
✅ Nested navigation
✅ Multi-level slugs

Examples:

```text
/docs/react/hooks/useEffect
/categories/electronics/laptops/gaming
/files/projects/frontend/nextjs
```

---

# 7. Optional Catch-All Routes

Syntax:

```bash
[[...slug]]
```

Difference:

- Optional catch-all also matches the parent route.

Example:

```bash
app/docs/[[...slug]]/page.tsx
```

Matches:

```text
/docs
/docs/react
/docs/react/hooks
```

Without optional syntax:

```text
/docs
```

would NOT match.

---

# 8. Catch-All vs Dynamic Routes

| Route Type         | Syntax        | Matches             |
| ------------------ | ------------- | ------------------- |
| Dynamic Route      | `[id]`        | One segment         |
| Catch-All Route    | `[...slug]`   | Multiple segments   |
| Optional Catch-All | `[[...slug]]` | Multiple + optional |

---

# 9. Production-Level Example

Example documentation system:

```bash
app/
└── docs/
    └── [...slug]/
        └── page.tsx
```

```tsx
// app/docs/[...slug]/page.tsx

import { notFound } from "next/navigation";

type Doc = {
  title: string;
  content: string;
};

async function getDoc(slug: string[]): Promise<Doc | null> {
  const path = slug.join("/");

  const res = await fetch(`https://cms.example.com/docs/${path}`, {
    next: {
      revalidate: 3600,
    },
  });

  if (!res.ok) {
    return null;
  }

  return res.json();
}

type Props = {
  params: Promise<{
    slug: string[];
  }>;
};

export default async function DocsPage({ params }: Props) {
  const { slug } = await params;

  const doc = await getDoc(slug);

  if (!doc) {
    notFound();
  }

  return (
    <article className="prose mx-auto">
      <h1>{doc.title}</h1>
      <div>{doc.content}</div>
    </article>
  );
}
```

Features:
✅ Dynamic nesting
✅ ISR
✅ SEO-friendly
✅ Server Components
✅ CMS scalability

---

# 10. Static Generation with Catch-All Routes

Using:

```tsx
generateStaticParams();
```

Example:

```tsx
export async function generateStaticParams() {
  return [
    {
      slug: ["react", "hooks"],
    },
    {
      slug: ["nextjs", "routing"],
    },
  ];
}
```

This prebuilds:

```text
/docs/react/hooks
/docs/nextjs/routing
```

---

# 11. Catch-All Routes + Metadata

Dynamic SEO generation:

```tsx
export async function generateMetadata({ params }: Props) {
  const { slug } = await params;

  return {
    title: slug.join(" / "),
  };
}
```

Excellent for:

- Docs platforms
- CMS sites
- SEO-heavy nested pages

---

# 12. Client Navigation

```tsx
import Link from "next/link";

<Link href="/docs/react/hooks">React Hooks</Link>;
```

Benefits:
✅ Prefetching
✅ SPA transitions
✅ Fast navigation

---

# 13. Performance Considerations

Catch-all routes can grow very large.

Best practices:
✅ Use ISR
✅ Cache aggressively
✅ Use static generation where possible
✅ Avoid huge recursive fetches

For large CMS systems:

- Combine ISR + CDN caching
- Generate critical paths statically

---

# 14. Common Pitfalls

| Pitfall                    | Solution                 |
| -------------------------- | ------------------------ |
| Overusing catch-all routes | Keep routing predictable |
| Poor slug validation       | Sanitize params          |
| Deep recursive fetching    | Cache results            |
| Huge static builds         | Use ISR                  |
| Missing 404 handling       | Use `notFound()`         |

---

# 15. Catch-All Routes in Pages Router

Legacy syntax is similar:

```bash
pages/docs/[...slug].tsx
```

But params accessed differently:

```tsx
import { useRouter } from "next/router";

const router = useRouter();

router.query.slug;
```

Modern App Router uses:

```tsx
params;
```

---

# 16. Advanced Interview Insight

Strong senior-level statement:

> “Catch-all routes allow Next.js to model hierarchical content structures using filesystem-based routing.”

Another excellent insight:

> “Catch-all routes are commonly paired with ISR and CMS-driven architectures for scalable content systems.”

That demonstrates architectural understanding.

---

# 17. When to Use Catch-All Routes

Use catch-all routes for:
✅ Documentation platforms
✅ Knowledge bases
✅ CMS hierarchies
✅ Nested categories
✅ File systems
✅ Deep navigation trees

---

# Alternatives

| Approach            | Best Use                |
| ------------------- | ----------------------- |
| Dynamic Route       | Single parameter        |
| Catch-All Route     | Nested paths            |
| Query Params        | Filters/search          |
| Middleware Rewrites | Advanced custom routing |

---

# Final Interview Summary

> “A catch-all route in Next.js uses the `[...param]` syntax to match multiple URL segments dynamically. Unlike standard dynamic routes that capture only one segment, catch-all routes capture entire nested paths as arrays. They are commonly used for documentation systems, CMS-driven sites, and hierarchical navigation structures, especially when combined with ISR and Server Components in Next.js 16.”

## Question 14. How can you create API routes in Next.js?

**Q: How can you create API routes in Next.js?**

**Short Answer (30 seconds):**
In Next.js, API routes allow you to build backend endpoints directly inside the application without a separate server. In Next.js 16 App Router, API routes are created using `route.ts` files inside the `app/api/` directory, where you export HTTP method handlers like `GET`, `POST`, `PUT`, and `DELETE`.

---

# Detailed Explanation

## 1. What are API Routes in Next.js?

API routes let Next.js function as both:

- Frontend framework
- Backend server

This enables:
✅ Full-stack applications
✅ Server-side logic
✅ Database access
✅ Authentication
✅ Webhooks
✅ Form handling

Docs:

- [Route Handlers Docs](https://nextjs.org/docs/app/building-your-application/routing/route-handlers)

---

# 2. API Routes in App Router (Next.js 16)

Modern Next.js uses:

```bash
app/api/
```

Example:

```bash
app/
└── api/
    └── users/
        └── route.ts
```

This creates:

```text
/api/users
```

---

# 3. Basic API Route Example

```tsx
// app/api/users/route.ts

import { NextResponse } from "next/server";

export async function GET() {
  return NextResponse.json({
    users: [],
  });
}
```

Request:

```bash
GET /api/users
```

Response:

```json
{
  "users": []
}
```

---

# 4. HTTP Methods

You export functions matching HTTP verbs.

Supported methods:

```tsx
GET;
POST;
PUT;
PATCH;
DELETE;
OPTIONS;
HEAD;
```

Example:

```tsx
// app/api/users/route.ts

import { NextResponse } from "next/server";

export async function GET() {
  return NextResponse.json({
    message: "GET request",
  });
}

export async function POST() {
  return NextResponse.json({
    message: "POST request",
  });
}
```

---

# 5. Accessing Request Data

Use the `Request` object.

Example:

```tsx
// app/api/users/route.ts

import { NextResponse } from "next/server";

export async function POST(request: Request) {
  const body = await request.json();

  return NextResponse.json({
    received: body,
  });
}
```

---

# 6. Dynamic API Routes

Use dynamic segments.

Example:

```bash
app/api/users/[id]/route.ts
```

Endpoint:

```text
/api/users/123
```

Example:

```tsx
// app/api/users/[id]/route.ts

import { NextResponse } from "next/server";

type Props = {
  params: Promise<{
    id: string;
  }>;
};

export async function GET(request: Request, { params }: Props) {
  const { id } = await params;

  return NextResponse.json({
    userId: id,
  });
}
```

---

# 7. Production-Level CRUD API Example

```tsx
// app/api/products/[id]/route.ts

import { NextResponse } from "next/server";

type Props = {
  params: Promise<{
    id: string;
  }>;
};

export async function GET(request: Request, { params }: Props) {
  const { id } = await params;

  // Database fetch example
  const product = {
    id,
    title: "iPhone 15",
  };

  return NextResponse.json(product);
}

export async function DELETE(request: Request, { params }: Props) {
  const { id } = await params;

  // Delete logic here

  return NextResponse.json({
    success: true,
    deletedId: id,
  });
}
```

---

# 8. Returning Responses

Recommended:

```tsx
NextResponse.json();
```

Example:

```tsx
return NextResponse.json({
  success: true,
});
```

You can also return:

```tsx
new Response();
```

for lower-level control.

---

# 9. Status Codes

Example:

```tsx
return NextResponse.json(
  {
    error: "Unauthorized",
  },
  {
    status: 401,
  },
);
```

---

# 10. Reading Query Parameters

Example:

```tsx
export async function GET(request: Request) {
  const { searchParams } = new URL(request.url);

  const page = searchParams.get("page");

  return NextResponse.json({
    page,
  });
}
```

Request:

```text
/api/products?page=2
```

---

# 11. Database Integration

API routes are commonly used with:

- Prisma
- PostgreSQL
- MongoDB
- Drizzle ORM

Example:

```tsx
import { db } from "@/lib/db";

export async function GET() {
  const users = await db.user.findMany();

  return NextResponse.json(users);
}
```

---

# 12. Authentication Example

```tsx
import { auth } from "@/lib/auth";
import { NextResponse } from "next/server";

export async function GET() {
  const session = await auth();

  if (!session) {
    return NextResponse.json(
      {
        error: "Unauthorized",
      },
      {
        status: 401,
      },
    );
  }

  return NextResponse.json({
    user: session.user,
  });
}
```

---

# 13. Edge Runtime Support

API routes can run on:
✅ Node.js runtime
✅ Edge runtime

Example:

```tsx
export const runtime = "edge";
```

Benefits:

- Lower latency
- Global distribution
- Faster cold starts

Docs:

- [Edge Runtime Docs](https://nextjs.org/docs/app/building-your-application/rendering/edge-and-nodejs-runtimes)

---

# 14. Middleware + API Routes

Example:

- Protect API endpoints
- Add rate limiting
- Handle auth globally

Using:

```bash
middleware.ts
```

---

# 15. API Routes in Pages Router (Legacy)

Legacy approach:

```bash
pages/api/users.ts
```

Example:

```tsx
import type { NextApiRequest, NextApiResponse } from "next";

export default function handler(req: NextApiRequest, res: NextApiResponse) {
  res.status(200).json({
    users: [],
  });
}
```

Modern App Router uses:

```tsx
route.ts;
```

instead.

---

# 16. Route Handlers vs Traditional Backend APIs

| Feature       | Next.js API Routes  | Express/NestJS |
| ------------- | ------------------- | -------------- |
| Deployment    | Integrated          | Separate       |
| Routing       | File-system         | Manual         |
| Scaling       | Serverless-friendly | Custom infra   |
| Full-stack DX | Excellent           | More setup     |
| Edge support  | Built-in            | Manual         |

---

# 17. Performance Considerations

Best practices:
✅ Use caching when possible
✅ Keep handlers stateless
✅ Prefer Edge runtime for lightweight APIs
✅ Avoid heavy synchronous work
✅ Validate input properly

---

# 18. Common Pitfalls

| Pitfall                        | Solution                   |
| ------------------------------ | -------------------------- |
| Business logic inside handlers | Move to services/lib       |
| No validation                  | Use Zod/Yup                |
| Large serverless bundles       | Keep dependencies minimal  |
| Blocking operations            | Use async operations       |
| No auth checks                 | Protect endpoints properly |

---

# 19. Production Architecture Example

```bash
app/
└── api/
    ├── auth/
    │   └── route.ts
    ├── users/
    │   ├── route.ts
    │   └── [id]/
    │       └── route.ts
    └── products/
        ├── route.ts
        └── [id]/
            └── route.ts
```

Scalable and enterprise-friendly.

---

# 20. Advanced Interview Insight

Strong senior-level answer:

> “Next.js API routes are essentially server-side request handlers integrated into the framework’s routing system.”

Another strong insight:

> “In the App Router, route handlers align naturally with React Server Components and server-first architecture.”

That demonstrates architectural depth.

---

# When to Use API Routes

Use API routes for:
✅ CRUD operations
✅ Authentication
✅ Webhooks
✅ Database access
✅ Server-side processing
✅ Internal APIs
✅ BFF (Backend for Frontend)

---

# Alternatives

| Technology         | Best Use                        |
| ------------------ | ------------------------------- |
| Next.js API Routes | Full-stack Next.js apps         |
| Express            | Custom backend servers          |
| NestJS             | Enterprise backend architecture |
| GraphQL            | Flexible APIs                   |
| tRPC               | Type-safe APIs                  |

---

# Final Interview Summary

> “In Next.js 16, API routes are created using `route.ts` files inside the `app/api` directory. Each route exports HTTP method handlers like `GET` or `POST`, allowing developers to build backend APIs directly within the Next.js application. These route handlers support dynamic routing, Edge runtime execution, database integration, authentication, and serverless deployment, making Next.js a powerful full-stack framework.”

## Question 15. What is the difference between getStaticProps and getServerSideProps?

**Q: What is the difference between `getStaticProps` and `getServerSideProps`?**

**Short Answer (30 seconds):**
`getStaticProps` fetches data at build time and generates static HTML for maximum performance and scalability, while `getServerSideProps` fetches data on every request and generates HTML dynamically on the server. `getStaticProps` is ideal for mostly static content, whereas `getServerSideProps` is used when data must always be fresh or user-specific.

---

# Detailed Explanation

## 1. Core Difference

The main difference is **when the data is fetched**.

| Function             | Data Fetch Timing |
| -------------------- | ----------------- |
| `getStaticProps`     | Build time        |
| `getServerSideProps` | Request time      |

---

# 2. `getStaticProps` (SSG)

`getStaticProps` is used for:
✅ Static Site Generation (SSG)
✅ Pre-rendering pages at build time

Docs:

- [getStaticProps Docs](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-static-props)

Example:

```tsx
// pages/blog.tsx

type Post = {
  id: number;
  title: string;
};

type Props = {
  posts: Post[];
};

export default function BlogPage({ posts }: Props) {
  return (
    <div>
      {posts.map((post) => (
        <p key={post.id}>{post.title}</p>
      ))}
    </div>
  );
}

export async function getStaticProps() {
  const res = await fetch("https://api.example.com/posts");

  const posts = await res.json();

  return {
    props: {
      posts,
    },
  };
}
```

---

# 3. How `getStaticProps` Works

Flow:

```text
Build Time
   ↓
Fetch Data
   ↓
Generate HTML
   ↓
Deploy Static Files
```

Result:
✅ Extremely fast
✅ CDN cacheable
✅ SEO-friendly
✅ Low server cost

---

# 4. `getServerSideProps` (SSR)

Used for:
✅ Server-Side Rendering (SSR)

Docs:

- [getServerSideProps Docs](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-server-side-props)

Example:

```tsx
// pages/dashboard.tsx

type Props = {
  users: number;
};

export default function Dashboard({ users }: Props) {
  return <h1>Users: {users}</h1>;
}

export async function getServerSideProps() {
  const res = await fetch("https://api.example.com/users");

  const data = await res.json();

  return {
    props: {
      users: data.count,
    },
  };
}
```

---

# 5. How `getServerSideProps` Works

Flow:

```text
Request
   ↓
Fetch Data
   ↓
Render HTML on Server
   ↓
Send Response
```

Result:
✅ Always fresh data
✅ Personalized rendering
✅ Request-aware rendering

But:
❌ Slower than static pages
❌ Higher server cost

---

# 6. Key Differences

| Feature        | `getStaticProps` | `getServerSideProps` |
| -------------- | ---------------- | -------------------- |
| Rendering Type | SSG              | SSR                  |
| Execution Time | Build time       | Request time         |
| Performance    | Very fast        | Slower               |
| Scalability    | Excellent        | Moderate             |
| CDN Cacheable  | Yes              | Limited              |
| Fresh Data     | Not always       | Always               |
| SEO            | Excellent        | Excellent            |
| Server Cost    | Low              | Higher               |

---

# 7. When to Use `getStaticProps`

Use for:
✅ Blogs
✅ Marketing pages
✅ Documentation
✅ Product catalogs
✅ Landing pages
✅ CMS content

Example:

```text
/blog
/docs
/pricing
```

---

# 8. When to Use `getServerSideProps`

Use for:
✅ Dashboards
✅ Authenticated pages
✅ Personalized content
✅ Real-time data
✅ Request-based rendering

Example:

```text
/dashboard
/profile
/orders
```

---

# 9. Incremental Static Regeneration (ISR)

`getStaticProps` supports ISR using:

```tsx
revalidate;
```

Example:

```tsx
export async function getStaticProps() {
  return {
    props: {},
    revalidate: 60,
  };
}
```

Meaning:

- Regenerate page every 60 seconds

Benefits:
✅ Static performance
✅ Fresh content

This reduces the need for SSR.

Docs:

- [ISR Docs](https://nextjs.org/docs/pages/building-your-application/data-fetching/incremental-static-regeneration)

---

# 10. Access to Request Object

`getServerSideProps` has access to:

```tsx
context.req
context.res
cookies
headers
query params
```

Example:

```tsx
export async function getServerSideProps(context) {
  const token = context.req.cookies.token;

  return {
    props: {},
  };
}
```

`getStaticProps` does NOT have request access because it runs at build time.

---

# 11. SEO Considerations

Both support:
✅ Fully rendered HTML
✅ Excellent SEO

Difference:

- SSG is faster
- SSR ensures freshest content

Google can crawl both effectively.

---

# 12. Performance Comparison

## `getStaticProps`

Advantages:
✅ CDN distribution
✅ Minimal server work
✅ Extremely scalable

Ideal for high-traffic sites.

---

## `getServerSideProps`

Advantages:
✅ Real-time rendering
✅ Dynamic personalization

Tradeoff:
❌ Higher latency

---

# 13. Production Architecture Example

Enterprise apps often combine both:

| Page Type        | Strategy |
| ---------------- | -------- |
| Landing pages    | SSG      |
| Blog             | ISR      |
| Dashboard        | SSR      |
| Checkout         | SSR      |
| Product listings | ISR      |
| Admin panel      | SSR      |

---

# 14. Important Modern Context (Next.js 16)

Very important interview point:

Both functions belong to the **Pages Router**.

In App Router:

- They are replaced by:
  - Async Server Components
  - `fetch()` caching
  - `revalidate`
  - Route segment configs

Docs:

- [App Router Data Fetching Docs](https://nextjs.org/docs/app/building-your-application/data-fetching)

---

# 15. Modern App Router Equivalent

## SSG-like behavior

```tsx
async function getPosts() {
  const res = await fetch("https://api.example.com/posts", {
    cache: "force-cache",
  });

  return res.json();
}
```

---

## SSR-like behavior

```tsx
async function getUser() {
  const res = await fetch("https://api.example.com/user", {
    cache: "no-store",
  });

  return res.json();
}
```

---

# 16. Common Pitfalls

| Pitfall                                  | Solution                      |
| ---------------------------------------- | ----------------------------- |
| Using SSR unnecessarily                  | Prefer SSG/ISR                |
| Rebuilding entire site too often         | Use ISR                       |
| Using SSG for personalized pages         | Use SSR                       |
| Slow SSR APIs                            | Add caching                   |
| Treating App Router same as Pages Router | Learn new fetch caching model |

---

# 17. Advanced Interview Insight

Strong senior-level statement:

> “The choice between `getStaticProps` and `getServerSideProps` is fundamentally a tradeoff between scalability and freshness.”

Another strong insight:

> “Modern Next.js App Router replaces these APIs with a more granular caching model built directly into `fetch()`.”

That demonstrates Next.js 16 knowledge.

---

# 18. Migration to App Router

| Pages Router         | App Router              |
| -------------------- | ----------------------- |
| `getStaticProps`     | Cached fetch            |
| `getServerSideProps` | `cache: 'no-store'`     |
| ISR                  | `revalidate`            |
| Special APIs         | Native async components |

---

# When to Use

| Use Case                | Recommended          |
| ----------------------- | -------------------- |
| Static content          | `getStaticProps`     |
| Fresh user data         | `getServerSideProps` |
| Large CMS sites         | ISR                  |
| Personalized dashboards | SSR                  |

---

# Alternatives

| Technique | Best Use                       |
| --------- | ------------------------------ |
| SSG       | Static pages                   |
| SSR       | Dynamic pages                  |
| ISR       | Hybrid freshness               |
| CSR       | Client-only data               |
| RSC       | Modern App Router architecture |

---

# Final Interview Summary

> “`getStaticProps` fetches data at build time and generates static pages for maximum performance and scalability, while `getServerSideProps` fetches data on every request to provide fresh, dynamic content. `getStaticProps` is ideal for static or infrequently changing pages, whereas `getServerSideProps` is used for personalized or real-time data. In Next.js 16 App Router, these older APIs are largely replaced by async Server Components and built-in fetch caching strategies.”

## Question 16. What is the difference between getStaticPaths and getServerSideProps?

**Q: What is the difference between `getStaticPaths` and `getServerSideProps`?**

**Short Answer (30 seconds):**
`getStaticPaths` is used with `getStaticProps` to pre-generate dynamic static pages at build time, while `getServerSideProps` renders pages dynamically on every request. `getStaticPaths` helps Next.js know which dynamic routes to statically generate, whereas `getServerSideProps` is for request-time rendering with always-fresh data.

---

# Detailed Explanation

## 1. Core Purpose Difference

These two functions solve completely different problems.

| Function             | Purpose                                   |
| -------------------- | ----------------------------------------- |
| `getStaticPaths`     | Defines which dynamic pages to pre-render |
| `getServerSideProps` | Fetches data on every request             |

---

# 2. `getStaticPaths`

Used ONLY with:

```tsx
getStaticProps;
```

And ONLY for:
✅ Dynamic routes

Docs:

- [getStaticPaths Docs](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-static-paths)

Example route:

```bash
pages/blog/[slug].tsx
```

Next.js needs to know:

- Which blog pages should be generated at build time.

That’s the role of `getStaticPaths`.

---

# 3. Example of `getStaticPaths`

```tsx
// pages/blog/[slug].tsx

export async function getStaticPaths() {
  const res = await fetch("https://api.example.com/posts");

  const posts = await res.json();

  const paths = posts.map((post: any) => ({
    params: {
      slug: post.slug,
    },
  }));

  return {
    paths,
    fallback: false,
  };
}
```

Generated pages:

```text
/blog/react
/blog/nextjs
/blog/typescript
```

---

# 4. `getStaticProps`

Usually paired with `getStaticPaths`.

Example:

```tsx
export async function getStaticProps({ params }: any) {
  const res = await fetch(`https://api.example.com/posts/${params.slug}`);

  const post = await res.json();

  return {
    props: {
      post,
    },
  };
}
```

---

# 5. How `getStaticPaths` Works

Flow:

```text
Build Time
   ↓
Fetch All Dynamic Paths
   ↓
Generate Static HTML Pages
   ↓
Deploy to CDN
```

Result:
✅ Extremely fast pages
✅ SEO-friendly
✅ Highly scalable

---

# 6. `getServerSideProps`

`getServerSideProps` is completely different.

Docs:

- [getServerSideProps Docs](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-server-side-props)

It runs:
✅ On every request

Example:

```tsx
// pages/dashboard.tsx

export async function getServerSideProps() {
  const res = await fetch("https://api.example.com/stats");

  const stats = await res.json();

  return {
    props: {
      stats,
    },
  };
}
```

---

# 7. How `getServerSideProps` Works

Flow:

```text
Request
   ↓
Fetch Data
   ↓
Render HTML on Server
   ↓
Send Response
```

Result:
✅ Always fresh data
✅ Personalized rendering
❌ More server work

---

# 8. Key Difference Table

| Feature               | `getStaticPaths`             | `getServerSideProps`    |
| --------------------- | ---------------------------- | ----------------------- |
| Purpose               | Define static dynamic routes | Fetch request-time data |
| Used With             | `getStaticProps`             | Standalone              |
| Rendering Type        | SSG                          | SSR                     |
| Runs At               | Build time                   | Request time            |
| Dynamic Routes        | Yes                          | Optional                |
| Generates Static HTML | Yes                          | No                      |
| Access to Request     | No                           | Yes                     |

---

# 9. Example Comparison

---

## `getStaticPaths`

Used for:

```text
/blog/react
/blog/nextjs
/blog/typescript
```

Generated once during build.

---

## `getServerSideProps`

Used for:

```text
/dashboard
/orders
/profile
```

Rendered dynamically every request.

---

# 10. Fallback Modes in `getStaticPaths`

Important interview topic.

Example:

```tsx
return {
  paths,
  fallback: true,
};
```

Options:

| Fallback     | Behavior                       |
| ------------ | ------------------------------ |
| `false`      | Unknown routes = 404           |
| `true`       | Generate on-demand             |
| `'blocking'` | SSR first request, cache later |

---

# 11. Production Example (`getStaticPaths`)

```tsx
// pages/products/[id].tsx

export async function getStaticPaths() {
  const products = await fetchProducts();

  return {
    paths: products.map((product) => ({
      params: {
        id: product.id.toString(),
      },
    })),
    fallback: "blocking",
  };
}

export async function getStaticProps({ params }: any) {
  const product = await fetchProduct(params.id);

  return {
    props: {
      product,
    },
    revalidate: 3600,
  };
}
```

Features:
✅ SSG
✅ ISR
✅ Dynamic pages
✅ On-demand generation

---

# 12. Use Cases

---

## `getStaticPaths`

Best for:
✅ Blogs
✅ Product pages
✅ CMS content
✅ Documentation
✅ SEO-heavy sites

---

## `getServerSideProps`

Best for:
✅ Dashboards
✅ Authenticated pages
✅ Personalized content
✅ Real-time data

---

# 13. Performance Comparison

| Feature     | `getStaticPaths` + SSG | `getServerSideProps` |
| ----------- | ---------------------- | -------------------- |
| Speed       | Extremely fast         | Slower               |
| CDN Cache   | Excellent              | Limited              |
| Server Load | Minimal                | Higher               |
| Freshness   | Depends on ISR         | Always fresh         |

---

# 14. Important Modern Context (Next.js 16)

Very important interview point:

Both APIs belong to:

```text
Pages Router
```

In App Router:

- `getStaticPaths` → `generateStaticParams`
- `getServerSideProps` → async Server Components + fetch caching

Docs:

- [generateStaticParams Docs](https://nextjs.org/docs/app/api-reference/functions/generate-static-params)

---

# 15. Modern App Router Equivalent

---

## `getStaticPaths`

Becomes:

```tsx
generateStaticParams();
```

Example:

```tsx
export async function generateStaticParams() {
  return [{ slug: "react" }, { slug: "nextjs" }];
}
```

---

## `getServerSideProps`

Becomes:

```tsx
fetch(url, {
  cache: "no-store",
});
```

---

# 16. Common Pitfalls

| Pitfall                                  | Solution                  |
| ---------------------------------------- | ------------------------- |
| Using SSR for static content             | Prefer SSG                |
| Generating too many pages at build       | Use fallback/ISR          |
| Missing fallback handling                | Use proper loading states |
| Treating App Router same as Pages Router | Learn new caching model   |
| Overbuilding huge CMS sites              | Use ISR                   |

---

# 17. Advanced Interview Insight

Strong senior-level statement:

> “`getStaticPaths` is fundamentally a build-time route generation API, while `getServerSideProps` is a request-time rendering API.”

Another strong insight:

> “In modern Next.js, these concepts evolved into route generation and fetch caching strategies in the App Router.”

That demonstrates architectural understanding.

---

# 18. Visual Comparison

---

## `getStaticPaths`

```text
Build Time
   ↓
Generate Dynamic Pages
   ↓
Serve Static HTML
```

---

## `getServerSideProps`

```text
Every Request
   ↓
Fetch Data
   ↓
Render HTML
```

---

# When to Use

| Scenario           | Best Choice            |
| ------------------ | ---------------------- |
| Blog pages         | `getStaticPaths`       |
| Product catalogs   | `getStaticPaths` + ISR |
| Dashboard          | `getServerSideProps`   |
| Personalized pages | `getServerSideProps`   |

---

# Alternatives

| Technique | Best Use                    |
| --------- | --------------------------- |
| SSG       | Static content              |
| SSR       | Dynamic content             |
| ISR       | Hybrid freshness            |
| RSC       | Modern App Router rendering |

---

# Final Interview Summary

> “`getStaticPaths` is used with `getStaticProps` to define which dynamic routes should be statically generated at build time, making it ideal for scalable SEO-friendly pages like blogs or product catalogs. `getServerSideProps`, on the other hand, runs on every request and renders pages dynamically with fresh data. In Next.js 16 App Router, these older APIs are replaced by `generateStaticParams` and built-in fetch caching strategies.”

## Question 17. How can you fetch data in Next.js?

**Q: How can you fetch data in Next.js?**

**Short Answer (30 seconds):**
In Next.js 16, data fetching is primarily done inside async Server Components using the native `fetch()` API. Next.js extends `fetch()` with built-in caching, revalidation, streaming, and server-side rendering capabilities. Data can also be fetched on the client side using React hooks, Server Actions, Route Handlers, or external libraries like SWR and TanStack Query depending on the use case.

---

# Detailed Explanation

## 1. Data Fetching in Next.js 16

Modern Next.js uses a **server-first data fetching model**.

Preferred approach:
✅ Fetch data in Server Components
✅ Use async/await directly
✅ Leverage built-in caching

Docs:

- [Data Fetching Docs](https://nextjs.org/docs/app/building-your-application/data-fetching)

---

# 2. Server Component Data Fetching (Recommended)

This is the modern default.

Example:

```tsx
// app/products/page.tsx

type Product = {
  id: number;
  title: string;
};

async function getProducts(): Promise<Product[]> {
  const res = await fetch("https://api.example.com/products");

  if (!res.ok) {
    throw new Error("Failed to fetch products");
  }

  return res.json();
}

export default async function ProductsPage() {
  const products = await getProducts();

  return (
    <div>
      {products.map((product) => (
        <p key={product.id}>{product.title}</p>
      ))}
    </div>
  );
}
```

Benefits:
✅ Server-side rendering
✅ Zero client JS for fetch
✅ Better SEO
✅ Faster performance
✅ Streaming support

---

# 3. Next.js Extended `fetch()`

Next.js enhances native `fetch()` with:
✅ Automatic caching
✅ Revalidation
✅ Request deduplication
✅ Streaming integration

---

# 4. Static Data Fetching (SSG-like)

Using cache:

```tsx
fetch(url, {
  cache: "force-cache",
});
```

Example:

```tsx
async function getPosts() {
  const res = await fetch("https://api.example.com/posts", {
    cache: "force-cache",
  });

  return res.json();
}
```

Equivalent to:

```text
Static Site Generation
```

---

# 5. Dynamic Data Fetching (SSR-like)

Disable caching:

```tsx
fetch(url, {
  cache: "no-store",
});
```

Example:

```tsx
async function getUser() {
  const res = await fetch("https://api.example.com/user", {
    cache: "no-store",
  });

  return res.json();
}
```

Equivalent to:

```text
Server-Side Rendering
```

---

# 6. ISR-Style Revalidation

Use:

```tsx
next.revalidate;
```

Example:

```tsx
async function getProducts() {
  const res = await fetch("https://api.example.com/products", {
    next: {
      revalidate: 60,
    },
  });

  return res.json();
}
```

Meaning:

- Cache for 60 seconds
- Then regenerate in background

Equivalent to:

```text
ISR
```

Docs:

- [Caching and Revalidation Docs](https://nextjs.org/docs/app/building-your-application/caching)

---

# 7. Client-Side Data Fetching

Used for:
✅ Interactive UI
✅ User-triggered updates
✅ Real-time state
✅ Browser-only APIs

Example:

```tsx
"use client";

import { useEffect, useState } from "react";

type User = {
  id: number;
  name: string;
};

export default function Users() {
  const [users, setUsers] = useState<User[]>([]);

  useEffect(() => {
    async function fetchUsers() {
      const res = await fetch("/api/users");
      const data = await res.json();

      setUsers(data);
    }

    fetchUsers();
  }, []);

  return (
    <div>
      {users.map((user) => (
        <p key={user.id}>{user.name}</p>
      ))}
    </div>
  );
}
```

Tradeoffs:
❌ Worse SEO
❌ Loading states needed
❌ More client JS

---

# 8. SWR for Client Fetching

Recommended by Vercel for client-side caching.

Docs:

- [SWR Docs](https://swr.vercel.app)

Example:

```tsx
"use client";

import useSWR from "swr";

const fetcher = (url: string) => fetch(url).then((res) => res.json());

export default function Profile() {
  const { data, isLoading } = useSWR("/api/profile", fetcher);

  if (isLoading) {
    return <p>Loading...</p>;
  }

  return <p>{data.name}</p>;
}
```

Benefits:
✅ Caching
✅ Revalidation
✅ Polling
✅ Optimistic updates

---

# 9. Fetching from Databases

Common in Server Components.

Example:

```tsx
import { db } from "@/lib/db";

export default async function UsersPage() {
  const users = await db.user.findMany();

  return (
    <div>
      {users.map((user) => (
        <p key={user.id}>{user.name}</p>
      ))}
    </div>
  );
}
```

This avoids unnecessary HTTP requests.

---

# 10. Fetching Inside Route Handlers

Example:

```tsx
// app/api/users/route.ts

import { NextResponse } from "next/server";

export async function GET() {
  const res = await fetch("https://external-api.com/users");

  const users = await res.json();

  return NextResponse.json(users);
}
```

---

# 11. Parallel Data Fetching

Important performance optimization.

Bad:

```tsx
const users = await getUsers();
const posts = await getPosts();
```

Good:

```tsx
const [users, posts] = await Promise.all([getUsers(), getPosts()]);
```

Benefits:
✅ Reduced waterfall requests
✅ Faster TTFB

---

# 12. Streaming + Suspense

Next.js supports streaming data with React Suspense.

Example:

```tsx
<Suspense fallback={<Loading />}>
  <Products />
</Suspense>
```

Benefits:
✅ Faster perceived performance
✅ Progressive rendering

Docs:

- [Streaming Docs](https://nextjs.org/docs/app/building-your-application/routing/loading-ui-and-streaming)

---

# 13. Server Actions for Mutations

Modern Next.js mutation pattern.

Example:

```tsx
"use server";

export async function createPost(formData: FormData) {
  // Database mutation
}
```

Benefits:
✅ No separate API needed
✅ Server-only execution
✅ Secure mutations

Docs:

- [Server Actions Docs](https://nextjs.org/docs/app/building-your-application/data-fetching/server-actions-and-mutations)

---

# 14. Legacy Pages Router Methods

Older Next.js used:

| Method               | Purpose     |
| -------------------- | ----------- |
| `getStaticProps`     | SSG         |
| `getServerSideProps` | SSR         |
| `getStaticPaths`     | Dynamic SSG |

Modern App Router replaces most of these with:

- Async Server Components
- Fetch caching

---

# 15. Production-Level Architecture

Modern enterprise pattern:

```bash
app/
├── products/
│   └── page.tsx
├── api/
├── lib/
│   ├── db.ts
│   ├── fetchers.ts
│   └── cache.ts
```

Best practice:

- Centralize fetch logic
- Use typed APIs
- Cache aggressively
- Prefer server fetching

---

# 16. Common Pitfalls

| Pitfall                                      | Solution                 |
| -------------------------------------------- | ------------------------ |
| Fetching everything client-side              | Prefer Server Components |
| Sequential fetches                           | Use `Promise.all`        |
| No caching strategy                          | Configure fetch caching  |
| Overusing `no-store`                         | Use ISR where possible   |
| Calling internal APIs from Server Components | Access DB directly       |

---

# 17. Performance Best Practices

✅ Fetch on the server whenever possible
✅ Use ISR for semi-static data
✅ Stream slow components
✅ Use Suspense boundaries
✅ Parallelize requests
✅ Cache aggressively

---

# 18. Advanced Interview Insight

Strong senior-level answer:

> “In Next.js 16, data fetching is deeply integrated into the rendering architecture through async Server Components and extended fetch caching.”

Another strong insight:

> “The App Router transforms data fetching from lifecycle-driven patterns into declarative server-first rendering.”

That demonstrates deep framework understanding.

---

# 19. Data Fetching Strategy Comparison

| Strategy          | Best Use             |
| ----------------- | -------------------- |
| Server Components | Default/recommended  |
| ISR               | CMS/product catalogs |
| Client Fetching   | Interactive state    |
| SWR               | Realtime UI          |
| Server Actions    | Mutations            |
| Route Handlers    | Public APIs          |

---

# When to Use

| Scenario           | Recommended       |
| ------------------ | ----------------- |
| SEO pages          | Server Components |
| Realtime dashboard | Client fetching   |
| Product catalog    | ISR               |
| Form submissions   | Server Actions    |
| Public APIs        | Route Handlers    |

---

# Alternatives

| Tool             | Purpose            |
| ---------------- | ------------------ |
| Native `fetch()` | Default            |
| SWR              | Client caching     |
| TanStack Query   | Complex state sync |
| Axios            | HTTP client        |
| GraphQL          | Structured APIs    |

---

# Final Interview Summary

> “In Next.js 16, data fetching is primarily done inside async Server Components using the extended native `fetch()` API, which supports built-in caching, ISR, streaming, and SSR behavior. Developers can also fetch data client-side using hooks, SWR, or TanStack Query for interactive experiences, while Server Actions and Route Handlers handle mutations and backend APIs in a modern full-stack architecture.”

## Question 18. What is the `_app.js` file used for?

**Q: What is the `_app.js` file used for in Next.js?**

**Short Answer (30 seconds):**
`_app.js` is a special file in the Next.js Pages Router used to initialize pages and share common layout or global logic across the application. It allows developers to persist layouts, inject global providers, load global CSS, and maintain state between page navigations.

---

# Detailed Explanation

## 1. What is `_app.js`?

`_app.js` is a custom App component that wraps every page in the Pages Router.

Location:

```bash
pages/_app.js
```

or

```bash
pages/_app.tsx
```

Docs:

- [Custom App Docs](https://nextjs.org/docs/pages/building-your-application/routing/custom-app)

---

# 2. Purpose of `_app.js`

It is mainly used for:

✅ Shared layouts
✅ Global CSS imports
✅ Context Providers
✅ Theme providers
✅ Authentication wrappers
✅ Persistent UI
✅ Global state management

---

# 3. Basic `_app.tsx` Example

```tsx
// pages/_app.tsx

import type { AppProps } from "next/app";

export default function App({ Component, pageProps }: AppProps) {
  return <Component {...pageProps} />;
}
```

Here:

- `Component` → current page
- `pageProps` → props from data fetching methods

---

# 4. How `_app.js` Works

Flow:

```text
Request
   ↓
_app.tsx
   ↓
Current Page Component
```

Meaning:

- Every page gets wrapped by `_app.tsx`

---

# 5. Using Global Layouts

Common usage:

```tsx
// pages/_app.tsx

import type { AppProps } from "next/app";
import Layout from "@/components/Layout";

export default function App({ Component, pageProps }: AppProps) {
  return (
    <Layout>
      <Component {...pageProps} />
    </Layout>
  );
}
```

Now:

- Navbar
- Sidebar
- Footer

persist across route changes.

---

# 6. Global CSS Imports

Important interview point.

Global CSS can ONLY be imported inside:

```tsx
_app.tsx;
```

Example:

```tsx
import "@/styles/globals.css";
```

---

# 7. Adding Context Providers

Example:

```tsx
// pages/_app.tsx

import { SessionProvider } from "next-auth/react";

export default function App({ Component, pageProps }: AppProps) {
  return (
    <SessionProvider session={pageProps.session}>
      <Component {...pageProps} />
    </SessionProvider>
  );
}
```

Common providers:

- Auth providers
- Redux
- Zustand
- Theme providers
- Query clients

---

# 8. Persisting State Between Pages

Without `_app.js`:

- Layout remounts on navigation

With `_app.js`:

- Shared components persist

Benefits:
✅ Better UX
✅ Faster navigation
✅ Preserved UI state

---

# 9. Adding Analytics

Example:

```tsx
import { useEffect } from "react";
import { useRouter } from "next/router";

export default function App({ Component, pageProps }) {
  const router = useRouter();

  useEffect(() => {
    // Analytics tracking
  }, [router.pathname]);

  return <Component {...pageProps} />;
}
```

Common use cases:

- Google Analytics
- Segment
- PostHog
- Mixpanel

---

# 10. Per-Page Layout Pattern

Advanced Pages Router pattern.

Example:

```tsx
// pages/_app.tsx

export default function App({ Component, pageProps }: any) {
  const getLayout = Component.getLayout ?? ((page: React.ReactNode) => page);

  return getLayout(<Component {...pageProps} />);
}
```

Page example:

```tsx
// pages/dashboard.tsx

Dashboard.getLayout = function getLayout(page) {
  return <DashboardLayout>{page}</DashboardLayout>;
};
```

Benefits:
✅ Different layouts per page
✅ Flexible architecture

---

# 11. Data Fetching in `_app.js`

Possible but generally discouraged.

Example:

```tsx
App.getInitialProps = async () => {
  return {};
};
```

Problem:
❌ Disables automatic static optimization

Important interview point.

---

# 12. Why `getInitialProps` is Discouraged

Using it in `_app.js` forces:
❌ All pages into SSR

This hurts:

- Performance
- Caching
- Scalability

Modern recommendation:
✅ Fetch closer to the page/component

Docs:

- [getInitialProps Docs](https://nextjs.org/docs/pages/api-reference/functions/get-initial-props)

---

# 13. `_app.js` vs App Router Layouts

Very important Next.js 16 interview insight.

`_app.js` belongs ONLY to:

```text
Pages Router
```

Modern App Router replaces it with:

```bash
layout.tsx
```

---

# 14. App Router Equivalent

Modern replacement:

```bash
app/layout.tsx
```

Example:

```tsx
// app/layout.tsx

import "./globals.css";

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

This is now the preferred architecture in Next.js 16.

Docs:

- [App Router Layouts Docs](https://nextjs.org/docs/app/building-your-application/routing/layouts-and-templates)

---

# 15. `_app.js` vs `layout.tsx`

| Feature                   | `_app.js`    | `layout.tsx` |
| ------------------------- | ------------ | ------------ |
| Router Type               | Pages Router | App Router   |
| Shared UI                 | Yes          | Yes          |
| Nested Layouts            | Limited      | Excellent    |
| Streaming Support         | No           | Yes          |
| Server Components         | No           | Yes          |
| Recommended in Next.js 16 | Legacy       | Preferred    |

---

# 16. Production-Level `_app.tsx`

```tsx
// pages/_app.tsx

import "@/styles/globals.css";

import type { AppProps } from "next/app";

import { ThemeProvider } from "next-themes";

export default function App({ Component, pageProps }: AppProps) {
  return (
    <ThemeProvider attribute="class" defaultTheme="system">
      <Component {...pageProps} />
    </ThemeProvider>
  );
}
```

Production features:
✅ Global styles
✅ Theme support
✅ Shared providers
✅ Persistent layout

---

# 17. Common Pitfalls

| Pitfall                                     | Solution                            |
| ------------------------------------------- | ----------------------------------- |
| Using `getInitialProps` unnecessarily       | Prefer page-level fetching          |
| Putting too much logic in `_app.js`         | Keep it lightweight                 |
| Global state bloat                          | Modularize providers                |
| Large provider trees                        | Optimize provider placement         |
| Mixing Pages Router and App Router concepts | Understand architecture differences |

---

# 18. Advanced Interview Insight

Strong senior-level statement:

> “`_app.js` acts as the root component for the Pages Router and is primarily responsible for shared application concerns.”

Another strong insight:

> “In Next.js 16, App Router layouts provide a more scalable replacement with nested layouts, streaming, and Server Component support.”

That demonstrates modern framework understanding.

---

# When to Use `_app.js`

Use `_app.js` only when working with:
✅ Pages Router applications
✅ Legacy Next.js projects
✅ Incremental migrations

---

# Alternatives

| Approach          | Best Use                  |
| ----------------- | ------------------------- |
| `_app.js`         | Pages Router              |
| `layout.tsx`      | App Router                |
| Context Providers | Global state              |
| Templates         | Route-specific remounting |

---

# Final Interview Summary

> “`_app.js` is a special file in the Next.js Pages Router that wraps every page and is used for global application concerns such as layouts, providers, analytics, and global CSS. It enables persistent UI and shared state across navigations. However, in Next.js 16, the App Router’s `layout.tsx` system is the modern replacement, offering nested layouts, streaming, and Server Component support.”

## Question 19. What is the `_document.js` file used for?

**Q: What is the `_document.js` file used for in Next.js?**

**Short Answer (30 seconds):**
`_document.js` is a special file in the Next.js Pages Router used to customize the overall HTML document structure rendered on the server. It allows developers to modify the `<html>`, `<body>`, and document-level markup, typically for SSR customization, fonts, language settings, analytics scripts, or CSS-in-JS integrations.

---

# Detailed Explanation

## 1. What is `_document.js`?

`_document.js` is a custom Document component that controls the server-rendered HTML document.

Location:

```bash
pages/_document.js
```

or

```bash
pages/_document.tsx
```

Docs:

- [Custom Document Docs](https://nextjs.org/docs/pages/building-your-application/routing/custom-document)

---

# 2. Purpose of `_document.js`

It customizes:
✅ `<html>`
✅ `<body>`
✅ Global document structure
✅ Server-rendered markup

Typical use cases:

- Setting language
- Adding custom fonts
- Injecting analytics scripts
- CSS-in-JS SSR setup
- Custom metadata
- Performance optimizations

---

# 3. Basic `_document.tsx` Example

```tsx
// pages/_document.tsx

import { Html, Head, Main, NextScript } from "next/document";

export default function Document() {
  return (
    <Html lang="en">
      <Head />

      <body>
        <Main />
        <NextScript />
      </body>
    </Html>
  );
}
```

---

# 4. Important Components

| Component      | Purpose                 |
| -------------- | ----------------------- |
| `<Html>`       | Root HTML tag           |
| `<Head>`       | Document head           |
| `<Main>`       | Renders app content     |
| `<NextScript>` | Injects Next.js scripts |

---

# 5. How `_document.js` Works

Flow:

```text
Server Request
   ↓
_document.tsx
   ↓
_app.tsx
   ↓
Page Component
```

Important:

- `_document.js` only renders on the server
- Never runs in the browser

---

# 6. Setting HTML Language

Common example:

```tsx
<Html lang="en">
```

Benefits:
✅ Accessibility
✅ SEO
✅ Internationalization support

---

# 7. Adding Global Scripts

Example:

```tsx
<Head>
  <script
    dangerouslySetInnerHTML={{
      __html: `
        console.log('Analytics')
      `,
    }}
  />
</Head>
```

Use carefully.

Modern recommendation:
✅ Prefer `next/script`

Docs:

- [Script Optimization Docs](https://nextjs.org/docs/pages/building-your-application/optimizing/scripts)

---

# 8. Adding Fonts

Example:

```tsx
<Head>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
</Head>
```

Modern Next.js 16 recommendation:
✅ Use `next/font`

Docs:

- [next/font Docs](https://nextjs.org/docs/app/building-your-application/optimizing/fonts)

---

# 9. CSS-in-JS SSR Support

Historically used for:

- Styled-components
- Emotion

Example:

```tsx
Document.getInitialProps = async (ctx) => {
  // Extract styles here
};
```

Important for:
✅ Server-side style rendering
✅ Preventing FOUC (Flash of Unstyled Content)

---

# 10. Difference Between `_app.js` and `_document.js`

Very common interview question.

| Feature          | `_app.js`           | `_document.js`              |
| ---------------- | ------------------- | --------------------------- |
| Purpose          | Page initialization | HTML document customization |
| Runs Client-side | Yes                 | No                          |
| Runs Server-side | Yes                 | Yes                         |
| Wraps Pages      | Yes                 | No                          |
| Modify `<html>`  | No                  | Yes                         |
| Global Providers | Yes                 | No                          |

---

# 11. What Should NOT Go in `_document.js`

Avoid:
❌ Event handlers
❌ Application state
❌ React hooks
❌ Browser APIs
❌ Client-side logic

Because:

```text
_document.js is server-only
```

---

# 12. Common Use Cases

Use `_document.js` for:
✅ Setting `lang`
✅ Setting `dir`
✅ Injecting critical scripts
✅ CSS-in-JS SSR
✅ Preconnect/preload tags
✅ Custom body classes

---

# 13. Production-Level Example

```tsx
// pages/_document.tsx

import { Html, Head, Main, NextScript } from "next/document";

export default function Document() {
  return (
    <Html lang="en">
      <Head>
        <meta charSet="UTF-8" />

        <link rel="preconnect" href="https://fonts.googleapis.com" />

        <link rel="icon" href="/favicon.ico" />
      </Head>

      <body className="bg-white">
        <Main />
        <NextScript />
      </body>
    </Html>
  );
}
```

Features:
✅ SEO support
✅ Font optimization
✅ Accessibility
✅ Global document customization

---

# 14. `_document.js` and SEO

Useful for:
✅ Language attributes
✅ Metadata structure
✅ Preload/preconnect hints

However:
❌ Page-specific SEO should use:

- `next/head`
- Metadata API (App Router)

---

# 15. Important Modern Context (Next.js 16)

Very important interview insight:

`_document.js` belongs ONLY to:

```text
Pages Router
```

In App Router:

- `_document.js` is NOT used

Instead:

```bash
app/layout.tsx
```

controls the HTML structure.

---

# 16. App Router Equivalent

Modern replacement:

```tsx
// app/layout.tsx

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

Benefits:
✅ Nested layouts
✅ Server Components
✅ Streaming support
✅ Modern architecture

Docs:

- [App Router Layouts Docs](https://nextjs.org/docs/app/building-your-application/routing/layouts-and-templates)

---

# 17. `_document.js` vs App Router Layouts

| Feature           | `_document.js` | `layout.tsx` |
| ----------------- | -------------- | ------------ |
| Router Type       | Pages Router   | App Router   |
| Server-only       | Yes            | Mostly       |
| Nested Layouts    | No             | Yes          |
| Streaming         | No             | Yes          |
| Server Components | No             | Yes          |
| Recommended       | Legacy         | Modern       |

---

# 18. Common Pitfalls

| Pitfall                            | Solution                 |
| ---------------------------------- | ------------------------ |
| Adding app logic in `_document.js` | Keep it document-only    |
| Using browser APIs                 | Server-only file         |
| Forgetting `<Main />`              | Required                 |
| Forgetting `<NextScript />`        | Required                 |
| Using `_document.js` in App Router | Use `layout.tsx` instead |

---

# 19. Advanced Interview Insight

Strong senior-level statement:

> “`_document.js` customizes the server-rendered HTML shell in the Pages Router.”

Another strong insight:

> “In modern Next.js architecture, App Router layouts supersede much of `_document.js` functionality while adding streaming and nested layout support.”

That demonstrates Next.js 16 understanding.

---

# When to Use `_document.js`

Use `_document.js` only for:
✅ Legacy Pages Router projects
✅ HTML shell customization
✅ SSR CSS extraction
✅ Global document-level tags

---

# Alternatives

| Approach       | Best Use                            |
| -------------- | ----------------------------------- |
| `_document.js` | Pages Router document customization |
| `layout.tsx`   | App Router root layout              |
| Metadata API   | SEO                                 |
| `next/font`    | Fonts                               |
| `next/script`  | Script loading                      |

---

# Final Interview Summary

> “`_document.js` is a special server-only file in the Next.js Pages Router used to customize the overall HTML document structure, including the `<html>` and `<body>` tags. It is commonly used for language settings, SSR styling integrations, fonts, and global document-level configuration. In Next.js 16 App Router, much of this responsibility has shifted to `layout.tsx`, which provides a more modern and scalable architecture.”

## Question 20. How can you add global CSS in Next.js?

**Q: How can you add global CSS in Next.js?**

**Short Answer (30 seconds):**
In Next.js, global CSS is added by importing a global stylesheet once at the root of the application. In the App Router (Next.js 16), global CSS is typically imported inside `app/layout.tsx`, while in the legacy Pages Router it is imported inside `pages/_app.tsx`.

---

# Detailed Explanation

## 1. What is Global CSS?

Global CSS applies styles across the entire application.

Examples:
✅ CSS resets
✅ Typography
✅ Tailwind base styles
✅ Theme variables
✅ Utility classes
✅ Shared animations

Unlike CSS Modules:

- Global CSS is NOT scoped to components

---

# 2. Global CSS in Next.js 16 (App Router)

Modern Next.js uses:

```bash
app/layout.tsx
```

Example project structure:

```bash
app/
├── globals.css
├── layout.tsx
└── page.tsx
```

---

# 3. Importing Global CSS

Example:

```tsx
// app/layout.tsx

import "./globals.css";

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

Docs:

- [CSS Styling Docs](https://nextjs.org/docs/app/building-your-application/styling)

---

# 4. Example `globals.css`

```css
/* app/globals.css */

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: Arial, sans-serif;
  background: #fff;
  color: #111;
}
```

These styles apply globally.

---

# 5. Why Global CSS Must Be Imported at the Root

Next.js restricts global CSS imports to:

- Root layouts (App Router)
- `_app.tsx` (Pages Router)

Reason:
✅ Prevent style conflicts
✅ Optimize CSS ordering
✅ Avoid duplicated styles

---

# 6. Pages Router Approach (Legacy)

In older Next.js projects:

```bash
pages/_app.tsx
```

Example:

```tsx
// pages/_app.tsx

import "@/styles/globals.css";

import type { AppProps } from "next/app";

export default function App({ Component, pageProps }: AppProps) {
  return <Component {...pageProps} />;
}
```

---

# 7. Tailwind CSS Example

Very common in production apps.

Example:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

Imported in:

```tsx
import "./globals.css";
```

Docs:

- [Tailwind + Next.js Docs](https://nextjs.org/docs/app/building-your-application/styling/tailwind-css)

---

# 8. Global CSS vs CSS Modules

| Feature           | Global CSS    | CSS Modules           |
| ----------------- | ------------- | --------------------- |
| Scope             | Entire app    | Component-scoped      |
| Risk of conflicts | Higher        | Lower                 |
| Best for          | Resets/themes | Components            |
| Naming collisions | Possible      | Avoided automatically |

---

# 9. CSS Modules Example

Preferred for component styles.

Example:

```bash
Button.module.css
```

```css
.button {
  background: blue;
}
```

Component:

```tsx
import styles from "./Button.module.css";

export default function Button() {
  return <button className={styles.button}>Click</button>;
}
```

---

# 10. Production-Level Global Styling Architecture

Common structure:

```bash
app/
├── globals.css
├── theme.css
├── layout.tsx
components/
styles/
```

Best practice:
✅ Keep globals minimal
✅ Use CSS Modules/components for most styling
✅ Centralize design tokens

---

# 11. Using CSS Variables Globally

Example:

```css
:root {
  --primary-color: #2563eb;
  --background: #ffffff;
}

body {
  background: var(--background);
}
```

Benefits:
✅ Theming
✅ Dark mode
✅ Design system support

---

# 12. Dark Mode Example

```css
:root {
  --bg: white;
}

.dark {
  --bg: black;
}

body {
  background: var(--bg);
}
```

Often combined with:

- `next-themes`
- Tailwind dark mode

---

# 13. Font Styling

Modern Next.js recommendation:
✅ Use `next/font`

Example:

```tsx
import { Inter } from "next/font/google";

const inter = Inter({
  subsets: ["latin"],
});

export default function RootLayout({ children }: any) {
  return (
    <html lang="en">
      <body className={inter.className}>{children}</body>
    </html>
  );
}
```

Docs:

- [Font Optimization Docs](https://nextjs.org/docs/app/building-your-application/optimizing/fonts)

---

# 14. Common Pitfalls

| Pitfall                                       | Solution               |
| --------------------------------------------- | ---------------------- |
| Importing global CSS inside components        | Import only at root    |
| Too much global styling                       | Prefer CSS Modules     |
| Naming collisions                             | Use scoped styles      |
| Large CSS bundles                             | Split styles logically |
| Using old Pages Router patterns in App Router | Use `layout.tsx`       |

---

# 15. Performance Considerations

Best practices:
✅ Minimize global CSS size
✅ Prefer component-scoped styles
✅ Use Tailwind JIT/tree-shaking
✅ Avoid unused styles
✅ Use CSS variables for themes

---

# 16. Advanced Interview Insight

Strong senior-level statement:

> “Global CSS should mainly contain resets, typography, tokens, and truly shared styles, while most component styling should remain scoped.”

Another strong insight:

> “In App Router, global CSS integrates naturally with layout-based architecture and streaming rendering.”

That demonstrates production-level understanding.

---

# 17. Styling Options in Next.js

| Styling Method    | Best Use              |
| ----------------- | --------------------- |
| Global CSS        | App-wide styles       |
| CSS Modules       | Component styles      |
| Tailwind CSS      | Utility-first styling |
| Styled Components | Dynamic CSS-in-JS     |
| Emotion           | CSS-in-JS             |
| Inline Styles     | Small dynamic styles  |

---

# 18. Recommended Modern Approach

In Next.js 16:

✅ Global CSS for:

- Reset styles
- Theme variables
- Typography

✅ CSS Modules/Tailwind for:

- Components
- UI styling

---

# Production Example

```tsx
// app/layout.tsx

import "./globals.css";

import { Inter } from "next/font/google";

const inter = Inter({
  subsets: ["latin"],
});

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body className={inter.className}>{children}</body>
    </html>
  );
}
```

---

# When to Use Global CSS

Use global CSS for:
✅ Resets
✅ Themes
✅ Typography
✅ CSS variables
✅ Shared utility classes

---

# Alternatives

| Technique         | Best Use                 |
| ----------------- | ------------------------ |
| CSS Modules       | Component isolation      |
| Tailwind CSS      | Utility-first styling    |
| CSS-in-JS         | Dynamic styles           |
| Styled Components | Themed component styling |

---

# Final Interview Summary

> “In Next.js 16, global CSS is added by importing a stylesheet inside the root `app/layout.tsx` file. Global styles apply across the entire application and are typically used for resets, typography, themes, and shared utilities. While global CSS is useful for app-wide styling, component-level styling is generally handled with CSS Modules, Tailwind CSS, or CSS-in-JS solutions for better scalability and maintainability.”
