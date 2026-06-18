# Set 7

| #   | Question                                                                                                                                                    |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [How do you enable strict mode in Next.js?](#question-1-how-do-you-enable-strict-mode-in-nextjs)                                                            |
| 2   | [How do you configure TypeScript in an existing Next.js project?](#question-2-how-do-you-configure-typescript-in-an-existing-nextjs-project)                |
| 3   | [What is the difference between a static page and a dynamic page?](#question-3-what-is-the-difference-between-a-static-page-and-a-dynamic-page)             |
| 4   | [How do you disable SSR for a specific component?](#question-4-how-do-you-disable-ssr-for-a-specific-component)                                             |
| 5   | [What are the benefits of using the next/head component?](#question-5-what-are-the-benefits-of-using-the-nexthead-component)                                |
| 6   | [How do you add a global JavaScript file?](#question-6-how-do-you-add-a-global-javascript-file)                                                             |
| 7   | [How do you inspect page performance in Next.js?](#question-7-how-do-you-inspect-page-performance-in-nextjs)                                                |
| 8   | [How do you handle meta tags dynamically?](#question-8-how-do-you-handle-meta-tags-dynamically)                                                             |
| 9   | [How do you define environment variables for development vs production?](#question-9-how-do-you-define-environment-variables-for-development-vs-production) |
| 10  | [How does Next.js handle client-side caching?](#question-10-how-does-nextjs-handle-client-side-caching)                                                     |
| 11  | [How do you implement middleware for logging requests?](#question-11-how-do-you-implement-middleware-for-logging-requests)                                  |
| 12  | [How do you handle query strings in Next.js pages?](#question-12-how-do-you-handle-query-strings-in-nextjs-pages)                                           |
| 13  | [How can you conditionally render pages based on authentication?](#question-13-how-can-you-conditionally-render-pages-based-on-authentication)              |
| 14  | [How do you integrate third-party APIs in Next.js?](#question-14-how-do-you-integrate-third-party-apis-in-nextjs)                                           |
| 15  | [How do you handle errors in getServerSideProps?](#question-15-how-do-you-handle-errors-in-getserversideprops)                                              |
| 16  | [How do you handle errors in getStaticProps?](#question-16-how-do-you-handle-errors-in-getstaticprops)                                                      |
| 17  | [How do you handle fallback pages for ISR?](#question-17-how-do-you-handle-fallback-pages-for-isr)                                                          |
| 18  | [How can you prefetch data for dynamic routes?](#question-18-how-can-you-prefetch-data-for-dynamic-routes)                                                  |
| 19  | [How do you implement a loading state during navigation?](#question-19-how-do-you-implement-a-loading-state-during-navigation)                              |
| 20  | [How do you configure redirects in next.config.js?](#question-20-how-do-you-configure-redirects-in-nextconfigjs)                                            |

## Question 1. How do you enable strict mode in Next.js?

**Q: How do you enable Strict Mode in Next.js?**

**Short Answer (30 seconds):**

In **Next.js 16**, React Strict Mode is enabled by configuring the `reactStrictMode` option in `next.config.ts` (or `next.config.js`). Strict Mode is a **development-only feature** that helps identify unsafe lifecycle methods, unexpected side effects, deprecated APIs, and other potential issues before they reach production.

> **Docs:** Next.js Configuration → `reactStrictMode`

---

## Detailed Explanation

### 1. What is React Strict Mode?

React Strict Mode is a development tool provided by React that performs additional checks and warnings. It **does not affect production builds**.

It helps detect:

- Side effects during rendering
- Missing cleanup in `useEffect`
- Deprecated lifecycle methods
- Unsafe legacy APIs
- Components that are not resilient to React's concurrent rendering

Since Next.js is built on React, it supports Strict Mode out of the box.

---

### 2. Enable Strict Mode

In your `next.config.ts`:

```ts
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  reactStrictMode: true,
};

export default nextConfig;
```

Or in JavaScript:

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
};

module.exports = nextConfig;
```

After restarting the development server, React Strict Mode becomes active.

---

### 3. What happens in development?

One thing that surprises many developers is that React intentionally invokes certain operations twice in development.

For example:

```tsx
"use client";

import { useEffect } from "react";

export default function Home() {
  useEffect(() => {
    console.log("Effect executed");

    return () => {
      console.log("Cleanup");
    };
  }, []);

  return <h1>Hello Next.js</h1>;
}
```

Development output:

```
Effect executed
Cleanup
Effect executed
```

This is expected behavior.

React intentionally mounts, unmounts, and mounts again to verify that your effects are properly cleaned up.

In **production**, it runs only once.

---

### 4. Why is Strict Mode important?

For production-grade applications, Strict Mode helps you catch bugs early.

Examples include:

- Memory leaks
- Missing event listener cleanup
- Duplicate API requests
- State mutations
- Impure render functions
- Components incompatible with concurrent rendering

It prepares your application for modern React features.

---

### 5. Performance Considerations

Strict Mode:

- ✅ Runs only in development
- ✅ Has no production runtime cost
- ✅ Makes debugging easier
- ❌ May make development appear slower because components render twice intentionally

This behavior should **not** be treated as a performance problem.

---

### 6. Common Pitfalls & Solutions

#### Pitfall 1: Duplicate API calls

```tsx
useEffect(() => {
  fetch("/api/users");
}, []);
```

You may notice two requests during development.

This is expected under Strict Mode.

Instead of disabling Strict Mode, ensure your effects are idempotent and properly handle cleanup.

---

#### Pitfall 2: Forgetting cleanup

Incorrect:

```tsx
useEffect(() => {
  window.addEventListener("resize", handler);
}, []);
```

Correct:

```tsx
useEffect(() => {
  window.addEventListener("resize", handler);

  return () => {
    window.removeEventListener("resize", handler);
  };
}, []);
```

Strict Mode helps expose issues like these.

---

## Code

```ts
// next.config.ts

import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  reactStrictMode: true,
};

export default nextConfig;
```

Example component:

```tsx
"use client";

import { useEffect } from "react";

export default function Dashboard() {
  useEffect(() => {
    console.log("Fetching data...");

    return () => {
      console.log("Cleanup resources");
    };
  }, []);

  return <h1>Dashboard</h1>;
}
```

---

## When to use

Use React Strict Mode for:

- Large production applications
- Team development
- Debugging side effects
- Preparing for modern React features
- Finding memory leaks early

It is recommended to keep it enabled throughout development.

---

## Alternatives

- **React Strict Mode (recommended):** Development-time checks with no production impact.
- **Disable Strict Mode:** Only for temporary debugging if third-party libraries behave incorrectly; it is generally not recommended.
- **Manual Testing:** Useful, but it cannot automatically detect many lifecycle and cleanup issues that Strict Mode exposes.

**Interview Tip:** A strong answer is to mention that **Strict Mode is a development-only feature**, is enabled via `reactStrictMode: true` in `next.config.ts`, and that the **double execution of effects in development is intentional** to reveal side effects and missing cleanup—not a Next.js bug. This demonstrates an understanding of both React and Next.js best practices.

## Question 2. How do you configure TypeScript in an existing Next.js project?

## Question 3. What is the difference between a static page and a dynamic page?

## Question 4. How do you disable SSR for a specific component?

## Question 5. What are the benefits of using the next/head component?

## Question 6. How do you add a global JavaScript file?

## Question 7. How do you inspect page performance in Next.js?

## Question 8. How do you handle meta tags dynamically?

## Question 9. How do you define environment variables for development vs production?

## Question 10. How does Next.js handle client-side caching?

## Question 11. How do you implement middleware for logging requests?

## Question 12. How do you handle query strings in Next.js pages?

## Question 13. How can you conditionally render pages based on authentication?

## Question 14. How do you integrate third-party APIs in Next.js?

## Question 15. How do you handle errors in getServerSideProps?

## Question 16. How do you handle errors in getStaticProps?

## Question 17. How do you handle fallback pages for ISR?

## Question 18. How can you prefetch data for dynamic routes?

## Question 19. How do you implement a loading state during navigation?

## Question 20. How do you configure redirects in next.config.js?
