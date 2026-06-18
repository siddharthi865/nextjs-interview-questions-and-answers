# Set 16

| #   | Question                                                                                                                                                  |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [What is the difference between SSR, SSG, and CSR in simple terms?](#question-1-what-is-the-difference-between-ssr-ssg-and-csr-in-simple-terms)           |
| 2   | [How do you decide whether to use SSR or SSG for a page?](#question-2-how-do-you-decide-whether-to-use-ssr-or-ssg-for-a-page)                             |
| 3   | [What is the role of the next.config.js file?](#question-3-what-is-the-role-of-the-nextconfigjs-file)                                                     |
| 4   | [How do you configure environment variables for multiple environments?](#question-4-how-do-you-configure-environment-variables-for-multiple-environments) |
| 5   | [How do you serve static JSON files in Next.js?](#question-5-how-do-you-serve-static-json-files-in-nextjs)                                                |
| 6   | [How do you implement a simple counter component?](#question-6-how-do-you-implement-a-simple-counter-component)                                           |
| 7   | [How do you use the useEffect hook in Next.js?](#question-7-how-do-you-use-the-useeffect-hook-in-nextjs)                                                  |
| 8   | [How do you use the useState hook in Next.js?](#question-8-how-do-you-use-the-usestate-hook-in-nextjs)                                                    |
| 9   | [How do you use the useMemo and useCallback hooks in Next.js?](#question-9-how-do-you-use-the-usememo-and-usecallback-hooks-in-nextjs)                    |
| 10  | [How do you create a reusable button component?](#question-10-how-do-you-create-a-reusable-button-component)                                              |
| 11  | [How do you pass data from a parent component to a child component?](#question-11-how-do-you-pass-data-from-a-parent-component-to-a-child-component)      |
| 12  | [How do you pass data from a child component to a parent component?](#question-12-how-do-you-pass-data-from-a-child-component-to-a-parent-component)      |
| 13  | [How do you implement a conditional class in JSX?](#question-13-how-do-you-implement-a-conditional-class-in-jsx)                                          |
| 14  | [How do you handle simple form inputs using controlled components?](#question-14-how-do-you-handle-simple-form-inputs-using-controlled-components)        |
| 15  | [How do you import images from the public folder?](#question-15-how-do-you-import-images-from-the-public-folder)                                          |
| 16  | [How do you use the next/script component to load external scripts?](#question-16-how-do-you-use-the-nextscript-component-to-load-external-scripts)       |
| 17  | [How do you add a global font using @import or link?](#question-17-how-do-you-add-a-global-font-using-import-or-link)                                     |
| 18  | [How do you implement simple routing using the Link component?](#question-18-how-do-you-implement-simple-routing-using-the-link-component)                |
| 19  | [How do you navigate programmatically using router.push?](#question-19-how-do-you-navigate-programmatically-using-routerpush)                             |
| 20  | [How do you implement a simple 404 page?](#question-20-how-do-you-implement-a-simple-404-page)                                                            |

## Question 1. What is the difference between SSR, SSG, and CSR in simple terms?

## **Q: What is the difference between SSR, SSG, and CSR in simple terms?**

### **Short Answer (30 seconds):**

The simplest way to understand it is **when and where the HTML is generated**:

- **SSR (Server-Side Rendering):** HTML is generated **on every request** on the server.
- **SSG (Static Site Generation):** HTML is generated **once during build time** and served as a static file.
- **CSR (Client-Side Rendering):** The browser downloads JavaScript first, then generates the UI on the client.

In **Next.js 16**, you can use all three approaches depending on your page requirements, although the App Router encourages **Server Components with caching** instead of thinking purely in SSR vs SSG terms.

---

# Detailed Explanation

## 1. SSR (Server-Side Rendering)

**Definition**

For every user request:

1. Browser requests the page.
2. Server fetches data.
3. Server creates HTML.
4. HTML is sent to the browser.

```
User
   │
   ▼
Server
Fetch Data
   │
Generate HTML
   │
   ▼
Browser
```

### Example

An online banking dashboard.

Every request needs fresh information.

- Account balance
- Transactions
- Notifications

These cannot be pre-generated.

### Next.js App Router

Simply fetch without long-term caching.

```tsx
const data = await fetch("https://api.example.com/dashboard", {
  cache: "no-store",
});
```

This renders dynamically for every request.

---

## 2. SSG (Static Site Generation)

**Definition**

The HTML is created **once during build**.

```
Build Time
   │
Generate HTML
   │
Store Static Files
   │
Users receive same HTML
```

No server rendering happens for every visitor.

### Example

- Blog
- Documentation
- Marketing pages
- Company website

Content changes rarely.

---

### Benefits

- Extremely fast
- CDN cached
- Low server cost
- Excellent SEO

---

### Next.js Example

```tsx
const posts = await fetch("https://api.example.com/posts", {
  cache: "force-cache",
});
```

Or use:

```tsx
export const revalidate = 3600;
```

to regenerate every hour (Incremental Static Regeneration).

---

## 3. CSR (Client-Side Rendering)

Here the server sends almost empty HTML.

Browser downloads JavaScript.

JavaScript fetches data.

Then React builds the page.

```
Browser
   │
Download JS
   │
Run React
   │
Fetch Data
   │
Render UI
```

---

### Example

A dashboard after login.

SEO isn't important because only authenticated users can access it.

Examples:

- Admin Panel
- Analytics
- Chat App
- Project Management Tool

---

### Example

```tsx
"use client";

import { useEffect, useState } from "react";

export default function Users() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    fetch("/api/users")
      .then((res) => res.json())
      .then(setUsers);
  }, []);

  return <div>{users.length}</div>;
}
```

---

# Real-world Comparison

| Feature        | SSR                | SSG                           | CSR                            |
| -------------- | ------------------ | ----------------------------- | ------------------------------ |
| HTML generated | Every request      | Build time                    | Browser                        |
| SEO            | ✅ Excellent       | ✅ Excellent                  | ❌ Poor (unless prerendered)   |
| Initial load   | Fast               | Fastest                       | Slower                         |
| Fresh data     | ✅ Always          | ❌ Until rebuild/revalidation | ✅ After fetch                 |
| Server load    | High               | Very low                      | Low                            |
| CDN friendly   | Limited            | Excellent                     | JS assets only                 |
| Best for       | Personalized pages | Blogs, docs, marketing        | Dashboards, authenticated apps |

---

# Production Considerations

In **Next.js 16 App Router**, think in terms of **data fetching and caching** rather than choosing SSR or SSG up front.

- **Static rendering (SSG-like):** Use cached `fetch()` (`force-cache`) for content that changes infrequently.
- **Dynamic rendering (SSR-like):** Use `cache: "no-store"` or other dynamic APIs when each request needs fresh or personalized data.
- **CSR:** Use Client Components only for browser-only features such as local state, event handlers, or accessing browser APIs. Prefer fetching data on the server when possible to reduce JavaScript sent to the client.

This server-first architecture improves performance, SEO, and reduces client-side bundle size.

---

# Common Pitfalls & Solutions

### ❌ Making everything CSR

This hurts SEO, increases JavaScript shipped to the browser, and slows the first meaningful render.

**Solution:** Prefer Server Components and server-side data fetching when possible.

---

### ❌ Using SSR for rarely changing pages

Rendering on every request wastes server resources.

**Solution:** Use static rendering with caching or Incremental Static Regeneration (`revalidate`).

---

### ❌ Using SSG for highly dynamic data

Users may see stale content if the data changes frequently.

**Solution:** Use dynamic rendering (`cache: "no-store"`) or appropriate revalidation intervals.

---

# Code (Next.js 16 – App Router)

```tsx
// Static rendering (SSG-like)
export default async function BlogPage() {
  const posts = await fetch("https://api.example.com/posts", {
    cache: "force-cache",
  }).then((res) => res.json());

  return <Posts posts={posts} />;
}

// Dynamic rendering (SSR-like)
export async function DashboardPage() {
  const user = await fetch("https://api.example.com/user", {
    cache: "no-store",
  }).then((res) => res.json());

  return <Dashboard user={user} />;
}
```

---

## **When to use**

- **SSR (Dynamic Rendering):** User-specific data, dashboards, authenticated content, frequently changing information.
- **SSG (Static Rendering):** Blogs, documentation, landing pages, product catalogs, marketing websites.
- **CSR:** Interactive dashboards, chat applications, browser-only functionality, components requiring local state or browser APIs.

---

## **Alternatives**

| Approach                            | Best For                                                                                              |
| ----------------------------------- | ----------------------------------------------------------------------------------------------------- |
| **Server Components (Recommended)** | Default choice in Next.js 16 for most pages; fetch data on the server with minimal client JavaScript. |
| **Client Components (CSR)**         | Interactive UI, event handling, browser APIs, local state.                                            |
| **Streaming + Suspense**            | Show page content progressively while slower data loads, improving perceived performance.             |

**Next.js 16 Docs:**

- App Router → Building Your Application → Rendering (Static and Dynamic Rendering)
- App Router → Data Fetching
- App Router → Server and Client Components
- App Router → Caching and Revalidating

## Question 2. How do you decide whether to use SSR or SSG for a page?

## Question 3. What is the role of the next.config.js file?

## Question 4. How do you configure environment variables for multiple environments?

## Question 5. How do you serve static JSON files in Next.js?

## Question 6. How do you implement a simple counter component?

## Question 7. How do you use the useEffect hook in Next.js?

## Question 8. How do you use the useState hook in Next.js?

## Question 9. How do you use the useMemo and useCallback hooks in Next.js?

## Question 10. How do you create a reusable button component?

## Question 11. How do you pass data from a parent component to a child component?

## Question 12. How do you pass data from a child component to a parent component?

## Question 13. How do you implement a conditional class in JSX?

## Question 14. How do you handle simple form inputs using controlled components?

## Question 15. How do you import images from the public folder?

## Question 16. How do you use the next/script component to load external scripts?

## Question 17. How do you add a global font using @import or link?

## Question 18. How do you implement simple routing using the Link component?

## Question 19. How do you navigate programmatically using router.push?

## Question 20. How do you implement a simple 404 page?
