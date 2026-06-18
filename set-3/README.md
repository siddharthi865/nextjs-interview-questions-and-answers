# Set 3

| #   | Question                                                                                                                                         |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1   | [How do you do a 404 page in Next.js?](#question-1-how-do-you-do-a-404-page-in-nextjs)                                                           |
| 2   | [What is a custom error page in Next.js?](#question-2-what-is-a-custom-error-page-in-nextjs)                                                     |
| 3   | [How do you handle API routes for CRUD operations?](#question-3-how-do-you-handle-api-routes-for-crud-operations)                                |
| 4   | [What are middleware functions in Next.js?](#question-4-what-are-middleware-functions-in-nextjs)                                                 |
| 5   | [How can you use Next.js middleware for authentication?](#question-5-how-can-you-use-nextjs-middleware-for-authentication)                       |
| 6   | [What is the difference between Next.js middleware and API routes?](#question-6-what-is-the-difference-between-nextjs-middleware-and-api-routes) |
| 7   | [How do you implement authentication in Next.js?](#question-7-how-do-you-implement-authentication-in-nextjs)                                     |
| 8   | [How can you implement JWT authentication in Next.js?](#question-8-how-can-you-implement-jwt-authentication-in-nextjs)                           |
| 9   | [What is server-side session handling in Next.js?](#question-9-what-is-server-side-session-handling-in-nextjs)                                   |
| 10  | [How do you handle cookies in Next.js?](#question-10-how-do-you-handle-cookies-in-nextjs)                                                        |
| 11  | [How do you deploy a Next.js app on Vercel?](#question-11-how-do-you-deploy-a-nextjs-app-on-vercel)                                              |
| 12  | [Can Next.js be deployed on other platforms? Which ones?](#question-12-can-nextjs-be-deployed-on-other-platforms-which-ones)                     |
| 13  | [How do you handle environment variables in production?](#question-13-how-do-you-handle-environment-variables-in-production)                     |
| 14  | [How do you handle caching in Next.js?](#question-14-how-do-you-handle-caching-in-nextjs)                                                        |
| 15  | [How does Next.js support incremental static regeneration?](#question-15-how-does-nextjs-support-incremental-static-regeneration)                |
| 16  | [How can you pre-render pages conditionally?](#question-16-how-can-you-pre-render-pages-conditionally)                                           |
| 17  | [How do you implement dynamic imports in Next.js?](#question-17-how-do-you-implement-dynamic-imports-in-nextjs)                                  |
| 18  | [What is the difference between dynamic import and lazy loading?](#question-18-what-is-the-difference-between-dynamic-import-and-lazy-loading)   |
| 19  | [How do you handle error boundaries in Next.js?](#question-19-how-do-you-handle-error-boundaries-in-nextjs)                                      |
| 20  | [How does Next.js handle fonts?](#question-20-how-does-nextjs-handle-fonts)                                                                      |

## Question 1. How do you do a 404 page in Next.js?

**Q: How do you do a 404 page in Next.js?**

**Short Answer (30 seconds):**
In Next.js 16 (App Router), you create a custom 404 page by defining a `not-found.tsx` file inside the `app/` directory or a route segment. Next.js automatically renders it when a route is not found or when you explicitly call `notFound()` from `next/navigation`.

---

**Detailed Explanation:**

### 1. Core concept (App Router-based 404 handling)

In Next.js App Router, 404 handling is **file-system based and declarative**.

📚 Docs: [https://nextjs.org/docs/app/api-reference/file-conventions/not-found](https://nextjs.org/docs/app/api-reference/file-conventions/not-found)
📚 Routing: [https://nextjs.org/docs/app/building-your-application/routing](https://nextjs.org/docs/app/building-your-application/routing)

There are **two ways** a 404 is triggered:

- Automatic: When no route matches
- Manual: When you call `notFound()` inside a Server Component or Route Handler

---

### 2. Implementation approach

#### Global 404 page

Create this file:

```
app/not-found.tsx
```

This becomes the **global fallback 404 page**.

#### Route-level 404 (optional)

You can also define:

```
app/blog/not-found.tsx
```

This overrides 404 UI only for `/blog/*` routes.

---

### 3. Code example (Production-ready App Router)

#### Global 404 page

```tsx
// app/not-found.tsx

import Link from "next/link";

export default function NotFound() {
  return (
    <div style={{ padding: "2rem", textAlign: "center" }}>
      <h1>404 - Page Not Found</h1>
      <p>The page you’re looking for doesn’t exist or has been moved.</p>

      <Link
        href="/"
        style={{ color: "blue", marginTop: "1rem", display: "inline-block" }}
      >
        Go back home
      </Link>
    </div>
  );
}
```

---

#### Programmatic 404 inside a page

```tsx
// app/products/[id]/page.tsx

import { notFound } from "next/navigation";

async function getProduct(id: string) {
  const res = await fetch(`https://api.example.com/products/${id}`, {
    cache: "no-store",
  });

  if (!res.ok) return null;
  return res.json();
}

export default async function ProductPage({
  params,
}: {
  params: { id: string };
}) {
  const product = await getProduct(params.id);

  if (!product) {
    notFound(); // triggers nearest not-found.tsx
  }

  return <div>{product.name}</div>;
}
```

---

### 4. Performance considerations

- `not-found.tsx` is a **Server Component by default**, keeping bundle size minimal
- Avoid heavy client-side logic in 404 pages
- Keep 404 UI lightweight for fast rendering
- Use shared layouts carefully—404 bypasses normal page rendering flow

---

### 5. Common pitfalls & solutions

**Pitfall 1: Using `pages/404.tsx` in App Router**

- ❌ Works only in Pages Router
- ✅ Use `app/not-found.tsx` instead

---

**Pitfall 2: Forgetting `notFound()` in dynamic routes**

- If data is missing but you still return a page, users see blank/incorrect UI
- Always explicitly call `notFound()` for missing resources

---

**Pitfall 3: Overly complex 404 pages**

- 404 pages should not fetch data or depend on external APIs heavily
- Keep them static and resilient

---

**When to use:**

- Missing routes (automatic fallback)
- Invalid dynamic route params
- Missing DB/API entities in dynamic pages

---

**Alternatives:**

- **Pages Router:** `pages/404.tsx`
- **Middleware-based redirects:** For redirecting instead of showing 404
- **Custom error pages:** `error.tsx` for runtime errors (not 404s)

## Question 2. What is a custom error page in Next.js?

## Question 3. How do you handle API routes for CRUD operations?

## Question 4. What are middleware functions in Next.js?

## Question 5. How can you use Next.js middleware for authentication?

## Question 6. What is the difference between Next.js middleware and API routes?

## Question 7. How do you implement authentication in Next.js?

## Question 8. How can you implement JWT authentication in Next.js?

## Question 9. What is server-side session handling in Next.js?

## Question 10. How do you handle cookies in Next.js?

## Question 11. How do you deploy a Next.js app on Vercel?

## Question 12. Can Next.js be deployed on other platforms? Which ones?

## Question 13. How do you handle environment variables in production?

## Question 14. How do you handle caching in Next.js?

## Question 15. How does Next.js support incremental static regeneration?

## Question 16. How can you pre-render pages conditionally?

## Question 17. How do you implement dynamic imports in Next.js?

## Question 18. What is the difference between dynamic import and lazy loading?

## Question 19. How do you handle error boundaries in Next.js?

## Question 20. How does Next.js handle fonts?
