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

```bash id="3nq9s6"
app/
 ├── page.tsx
 ├── about/
 │    └── page.tsx
```

Automatically becomes:

```bash id="qdx3ps"
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

```tsx id="r05j3l"
import Image from "next/image";
```

#### Font Optimization

```tsx id="6vowhk"
import { Inter } from "next/font/google";
```

#### Streaming + Suspense

#### Prefetching Links

```tsx id="g1s12x"
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

```text id="a7s3yr"
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

```tsx id="2br4ff"
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

```tsx id="m5f2m5"
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

```text id="klyukq"
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

```tsx id="jg8x3n"
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

```tsx id="t7gwgt"
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

```tsx id="hfygo3"
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

```text id="s45h4u"
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

```tsx id="g6n6jx"
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

```bash id="ofx7cq"
next build
```

Next.js:

1. Executes server components
2. Fetches data
3. Generates HTML
4. Saves output as static assets

Example generated files:

```bash id="pbjlwm"
.next/server/app/blog/page.html
```

These are deployed to the CDN.

---

# 8. Dynamic Routes with SSG

Next.js can statically generate dynamic pages too.

Example:

```bash id="35qj0w"
app/blog/[slug]/page.tsx
```

Using:

```tsx id="fzxztu"
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

```bash id="0q9e9y"
/blog/react
/blog/nextjs
/blog/typescript
```

Docs:

- [generateStaticParams Docs](https://nextjs.org/docs/app/api-reference/functions/generate-static-params)

---

# 9. Production-Level SSG Example

```tsx id="uxw4aq"
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

```tsx id="44j6et"
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

```text id="6lk8lg"
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

```tsx id="1m81fr"
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

```tsx id="0q6v5v"
"use client";
```

Docs:

- [Client Components Docs](https://nextjs.org/docs/app/building-your-application/rendering/client-components)

---

# 4. Example of CSR in Next.js

```tsx id="y4ib7e"
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

```tsx id="kr13l9"
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

```tsx id="ms1o1m"
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

```tsx id="v0iwp0"
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

## Question 7. Explain the folder structure of a Next.js project

## Question 8. What is the pages directory used for?

## Question 9. What is the public folder in Next.js?

## Question 10. How does Next.js handle routing?

## Question 11. What is a dynamic route in Next.js?

## Question 12. How do you create a dynamic route?

## Question 13. What is a catch-all route?

## Question 14. How can you create API routes in Next.js?

## Question 15. What is the difference between getStaticProps and getServerSideProps?

## Question 16. What is the difference between getStaticPaths and getServerSideProps?

## Question 17. How can you fetch data in Next.js?

## Question 18. What is the `_app.js` file used for?

## Question 19. What is the `_document.js` file used for?

## Question 20. How can you add global CSS in Next.js?
