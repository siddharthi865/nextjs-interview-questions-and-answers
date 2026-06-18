# Set 18

| #   | Question                                                                                                                                                   |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [How do you implement pagination using SSR?](#question-1-how-do-you-implement-pagination-using-ssr)                                                        |
| 2   | [How do you implement client-side pagination using SWR?](#question-2-how-do-you-implement-client-side-pagination-using-swr)                                |
| 3   | [How do you implement a multi-step form using state?](#question-3-how-do-you-implement-a-multi-step-form-using-state)                                      |
| 4   | [How do you implement validation in multi-step forms?](#question-4-how-do-you-implement-validation-in-multi-step-forms)                                    |
| 5   | [How do you handle file uploads in Next.js?](#question-5-how-do-you-handle-file-uploads-in-nextjs)                                                         |
| 6   | [How do you handle image optimization for external sources?](#question-6-how-do-you-handle-image-optimization-for-external-sources)                        |
| 7   | [How do you implement lazy loading for images and components?](#question-7-how-do-you-implement-lazy-loading-for-images-and-components)                    |
| 8   | [How do you handle dynamic imports with SSR disabled?](#question-8-how-do-you-handle-dynamic-imports-with-ssr-disabled)                                    |
| 9   | [How do you implement global error handling in API routes?](#question-9-how-do-you-implement-global-error-handling-in-api-routes)                          |
| 10  | [How do you implement a reusable layout component?](#question-10-how-do-you-implement-a-reusable-layout-component)                                         |
| 11  | [How do you implement dark/light theme toggling across pages?](#question-11-how-do-you-implement-darklight-theme-toggling-across-pages)                    |
| 12  | [How do you implement route-based code splitting?](#question-12-how-do-you-implement-route-based-code-splitting)                                           |
| 13  | [How do you prefetch data for multiple dynamic routes?](#question-13-how-do-you-prefetch-data-for-multiple-dynamic-routes)                                 |
| 14  | [How do you implement a preview mode for content drafts?](#question-14-how-do-you-implement-a-preview-mode-for-content-drafts)                             |
| 15  | [How do you implement authentication using cookies and sessions?](#question-15-how-do-you-implement-authentication-using-cookies-and-sessions)             |
| 16  | [How do you integrate OAuth providers like Google or GitHub?](#question-16-how-do-you-integrate-oauth-providers-like-google-or-github)                     |
| 17  | [How do you implement CSRF protection for API routes?](#question-17-how-do-you-implement-csrf-protection-for-api-routes)                                   |
| 18  | [How do you implement server-side logging for API requests?](#question-18-how-do-you-implement-server-side-logging-for-api-requests)                       |
| 19  | [How do you integrate Next.js with a headless CMS like Strapi?](#question-19-how-do-you-integrate-nextjs-with-a-headless-cms-like-strapi)                  |
| 20  | [How do you implement conditional rendering for SSR vs CSR components?](#question-20-how-do-you-implement-conditional-rendering-for-ssr-vs-csr-components) |

## Question 1. How do you implement pagination using SSR?

## **Q: How do you implement pagination using SSR?**

### **Short Answer (30 seconds):**

SSR pagination in Next.js means fetching only the data required for the requested page on the server for every request. The page number is typically read from the URL query parameters, the server fetches the corresponding records from the database or API, and renders the HTML before sending it to the client. This is ideal for frequently changing, SEO-sensitive content like product listings, search results, and news feeds.

---

# **Detailed Explanation**

## **1. Core Concept (Next.js 16 + App Router)**

With the App Router, pages are **Server Components by default**, making SSR pagination straightforward.

The flow is:

1. User visits `/products?page=3`
2. Next.js reads the `searchParams`
3. Server fetches only page 3 data
4. HTML is generated on the server
5. Browser receives fully rendered content

This provides:

- Excellent SEO
- Fast first paint
- Small payloads
- Fresh data on every request

**Official Docs:**

- App Router → Routing
- Data Fetching
- Server Components
- URL Search Params

[https://nextjs.org/docs/app/building-your-application/routing](https://nextjs.org/docs/app/building-your-application/routing)

[https://nextjs.org/docs/app/building-your-application/data-fetching](https://nextjs.org/docs/app/building-your-application/data-fetching)

---

# **2. Implementation Approach**

Assume an API:

```
GET /api/products?page=2&limit=10
```

Response:

```json
{
  "products": [...],
  "totalPages": 12,
  "currentPage": 2
}
```

In App Router:

```
app/
   products/
      page.tsx
```

Read the page number from:

```tsx
searchParams.page;
```

Fetch only the required records:

```tsx
?page=pageNumber
```

Render pagination links.

---

# **3. Production-Ready Example**

## **Server Component**

```tsx
type Product = {
  id: number;
  name: string;
};

async function getProducts(page: number) {
  const res = await fetch(
    `https://api.example.com/products?page=${page}&limit=10`,
    {
      cache: "no-store", // SSR
    },
  );

  if (!res.ok) {
    throw new Error("Failed to fetch products");
  }

  return res.json();
}

export default async function ProductsPage({
  searchParams,
}: {
  searchParams: Promise<{ page?: string }>;
}) {
  const params = await searchParams;
  const page = Number(params.page ?? "1");

  const data = await getProducts(page);

  return (
    <main>
      <h1>Products</h1>

      <ul>
        {data.products.map((product: Product) => (
          <li key={product.id}>{product.name}</li>
        ))}
      </ul>

      <div style={{ marginTop: 20 }}>
        {page > 1 && <a href={`/products?page=${page - 1}`}>Previous</a>}

        <span style={{ margin: "0 12px" }}>
          Page {page} of {data.totalPages}
        </span>

        {page < data.totalPages && (
          <a href={`/products?page=${page + 1}`}>Next</a>
        )}
      </div>
    </main>
  );
}
```

---

## **Using `Link` for Client Navigation (Recommended)**

Instead of `<a>`, use the App Router's `Link` component:

```tsx
import Link from "next/link";

<Link href={`/products?page=${page - 1}`}>
  Previous
</Link>

<Link href={`/products?page=${page + 1}`}>
  Next
</Link>
```

Benefits:

- Client-side navigation
- Prefetching
- Better UX
- No full page reload

---

# **4. Performance Considerations**

### ✅ Fetch only required records

Avoid:

```sql
SELECT *
```

Prefer:

```sql
LIMIT 10 OFFSET 20
```

or cursor-based pagination for very large datasets.

---

### ✅ Use database pagination

Never fetch all records and paginate in JavaScript.

Bad:

```ts
const allProducts = await getAllProducts();
```

Good:

```ts
SELECT * LIMIT 10 OFFSET 20;
```

---

### ✅ Validate page numbers

```tsx
const page = Math.max(1, Number(params.page ?? "1"));
```

Handle invalid values gracefully, and return a 404 or redirect if the page exceeds the available range.

---

### ✅ Stream page content

If fetching multiple independent datasets, use React Suspense and streaming so users see content sooner.

---

### ✅ Cache when appropriate

For frequently updated data:

```tsx
fetch(url, {
  cache: "no-store",
});
```

For data that changes less often, use:

```tsx
fetch(url, {
  next: {
    revalidate: 60,
  },
});
```

to reduce server load while keeping content reasonably fresh.

---

# **5. Common Pitfalls & Solutions**

### ❌ Fetching all records

Bad:

```tsx
const products = await fetchAllProducts();
```

This wastes memory and bandwidth.

**Solution:** Fetch only the current page from the database or API.

---

### ❌ Using client-side pagination for SEO pages

If Google needs to index each page, SSR is a better choice than fetching everything on the client.

---

### ❌ Losing page state

Always store the page number in the URL:

```
?page=4
```

Benefits:

- Shareable URLs
- Browser history support
- SEO-friendly
- Refresh-safe

---

### ❌ Not handling invalid page numbers

Always validate query parameters and handle out-of-range pages appropriately.

---

# **When to Use**

Use SSR pagination when:

- Product catalogs
- Search results
- News websites
- Blog listings
- E-commerce category pages
- Job portals
- Any SEO-critical listing with frequently changing data

---

# **Alternatives**

| Approach             | Best For                               | Pros                                   | Cons                                                                             |
| -------------------- | -------------------------------------- | -------------------------------------- | -------------------------------------------------------------------------------- |
| **SSR Pagination**   | Dynamic, SEO-sensitive data            | Fresh data every request, SEO-friendly | Higher server load                                                               |
| **SSG + Pagination** | Mostly static content                  | Very fast, CDN-cached                  | Can become expensive with many pages; less suitable for frequently changing data |
| **ISR Pagination**   | Semi-static content                    | Balances freshness and performance     | Data may be briefly stale between revalidations                                  |
| **CSR Pagination**   | Authenticated dashboards, admin panels | Smooth client interactions             | Poor SEO and slower initial content                                              |
| **Infinite Scroll**  | Social feeds, media apps               | Seamless UX                            | Harder for SEO and direct linking                                                |

> **Interview Tip:** In Next.js 16, SSR pagination is typically implemented in an **App Router Server Component** by reading `searchParams`, fetching only the requested page on the server (often with `cache: "no-store"` for fresh data), and rendering pagination controls with `next/link`. This keeps URLs shareable, improves SEO, minimizes data transfer, and scales well for large datasets.

## Question 2. How do you implement client-side pagination using SWR?

## Question 3. How do you implement a multi-step form using state?

## Question 4. How do you implement validation in multi-step forms?

## Question 5. How do you handle file uploads in Next.js?

## Question 6. How do you handle image optimization for external sources?

## Question 7. How do you implement lazy loading for images and components?

## Question 8. How do you handle dynamic imports with SSR disabled?

## Question 9. How do you implement global error handling in API routes?

## Question 10. How do you implement a reusable layout component?

## Question 11. How do you implement dark/light theme toggling across pages?

## Question 12. How do you implement route-based code splitting?

## Question 13. How do you prefetch data for multiple dynamic routes?

## Question 14. How do you implement a preview mode for content drafts?

## Question 15. How do you implement authentication using cookies and sessions?

## Question 16. How do you integrate OAuth providers like Google or GitHub?

## Question 17. How do you implement CSRF protection for API routes?

## Question 18. How do you implement server-side logging for API requests?

## Question 19. How do you integrate Next.js with a headless CMS like Strapi?

## Question 20. How do you implement conditional rendering for SSR vs CSR components?
