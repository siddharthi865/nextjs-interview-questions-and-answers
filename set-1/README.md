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

## Question 4. What is static site generation (SSG)?

## Question 5. What is client-side rendering (CSR) in Next.js?

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
