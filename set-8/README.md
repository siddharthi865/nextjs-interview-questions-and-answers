# Set 8

| #   | Question                                                                                                                                      |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [How do you configure rewrites in next.config.js?](#question-1-how-do-you-configure-rewrites-in-nextconfigjs)                                 |
| 2   | [How do you implement custom server logic in Next.js?](#question-2-how-do-you-implement-custom-server-logic-in-nextjs)                        |
| 3   | [How do you configure a custom Express.js server with Next.js?](#question-3-how-do-you-configure-a-custom-expressjs-server-with-nextjs)       |
| 4   | [How do you implement SSR with authentication cookies?](#question-4-how-do-you-implement-ssr-with-authentication-cookies)                     |
| 5   | [How do you handle protected API routes?](#question-5-how-do-you-handle-protected-api-routes)                                                 |
| 6   | [How do you implement throttling or rate-limiting in API routes?](#question-6-how-do-you-implement-throttling-or-rate-limiting-in-api-routes) |
| 7   | [How do you use SWR for client-side data fetching?](#question-7-how-do-you-use-swr-for-client-side-data-fetching)                             |
| 8   | [How do you implement polling with SWR?](#question-8-how-do-you-implement-polling-with-swr)                                                   |
| 9   | [How can you implement optimistic UI updates in Next.js?](#question-9-how-can-you-implement-optimistic-ui-updates-in-nextjs)                  |
| 10  | [How do you use Next.js with React Query?](#question-10-how-do-you-use-nextjs-with-react-query)                                               |
| 11  | [How can you integrate Next.js with Firebase?](#question-11-how-can-you-integrate-nextjs-with-firebase)                                       |
| 12  | [How can you integrate Next.js with Supabase?](#question-12-how-can-you-integrate-nextjs-with-supabase)                                       |
| 13  | [How do you implement serverless functions in Next.js?](#question-13-how-do-you-implement-serverless-functions-in-nextjs)                     |
| 14  | [How do you handle image optimization for external URLs?](#question-14-how-do-you-handle-image-optimization-for-external-urls)                |
| 15  | [How do you use the `<Script>` component in Next.js?](#question-15-how-do-you-use-the-script-component-in-nextjs)                             |
| 16  | [How do you lazy load third-party scripts?](#question-16-how-do-you-lazy-load-third-party-scripts)                                            |
| 17  | [How do you handle internationalized routing (i18n)?](#question-17-how-do-you-handle-internationalized-routing-i18n)                          |
| 18  | [How do you implement cookie consent banners?](#question-18-how-do-you-implement-cookie-consent-banners)                                      |
| 19  | [How do you implement persistent layouts across pages?](#question-19-how-do-you-implement-persistent-layouts-across-pages)                    |
| 20  | [How do you handle nested API routes?](#question-20-how-do-you-handle-nested-api-routes)                                                      |

## Question 1. How do you configure rewrites in next.config.js?

## **Q: How do you configure rewrites in `next.config.js`?**

### **Short Answer (30 seconds):**

Rewrites in Next.js allow you to **map one URL to another without changing the URL shown in the browser**. They're configured using the `async rewrites()` function inside `next.config.ts` (or `next.config.js`) and are commonly used for clean URLs, API proxying, legacy migrations, and hiding backend endpoints.

> **Docs:** Next.js 16 → Configuration → `next.config.js` → Rewrites

---

# **Detailed Explanation**

## 1. What are Rewrites?

A **rewrite** transparently routes a request from a **source** path to a **destination** path.

For example:

```
Browser URL:
https://example.com/blog/react

Actually serves:
https://example.com/articles/react
```

The browser still displays:

```
/blog/react
```

Unlike redirects:

- **Rewrite:** URL stays the same
- **Redirect:** Browser URL changes

Think of rewrites as an **internal URL mapping layer**.

---

## 2. Basic Configuration

In Next.js, rewrites are configured in `next.config.ts` or `next.config.js`.

```ts
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  async rewrites() {
    return [
      {
        source: "/blog/:slug",
        destination: "/articles/:slug",
      },
    ];
  },
};

export default nextConfig;
```

Now:

```
/blog/nextjs
```

renders

```
/articles/nextjs
```

without changing the browser URL.

---

## 3. Dynamic Routes

Dynamic parameters can be forwarded.

```ts
async rewrites() {
  return [
    {
      source: "/product/:id",
      destination: "/shop/products/:id",
    },
  ];
}
```

Request:

```
/product/100
```

Internally becomes:

```
/shop/products/100
```

---

## 4. Wildcard Matching

You can match multiple segments using `:path*`.

```ts
async rewrites() {
  return [
    {
      source: "/docs/:path*",
      destination: "/documentation/:path*",
    },
  ];
}
```

Examples:

```
/docs/api
```

↓

```
/documentation/api
```

and

```
/docs/react/hooks/useState
```

↓

```
/documentation/react/hooks/useState
```

---

## 5. Rewriting to External APIs

One common production use case is proxying backend APIs.

```ts
async rewrites() {
  return [
    {
      source: "/api/:path*",
      destination: "https://backend.example.com/:path*",
    },
  ];
}
```

Client calls:

```ts
fetch("/api/users");
```

Internally:

```
https://backend.example.com/users
```

Benefits:

- Hide backend URLs
- Avoid exposing infrastructure details
- Simplify frontend configuration

---

## 6. Conditional Rewrites

You can apply rewrites based on headers, cookies, query parameters, or the host.

Example using a header:

```ts
async rewrites() {
  return [
    {
      source: "/dashboard",
      has: [
        {
          type: "header",
          key: "x-admin",
          value: "true",
        },
      ],
      destination: "/admin/dashboard",
    },
  ];
}
```

Only requests with:

```
x-admin: true
```

are rewritten.

---

## 7. Rewrite Order

Next.js evaluates rewrites in a specific order:

1. `beforeFiles`
2. Filesystem routes
3. `afterFiles`
4. Dynamic routes
5. `fallback`

Example:

```ts
async rewrites() {
  return {
    beforeFiles: [
      {
        source: "/about",
        destination: "/company/about",
      },
    ],
    afterFiles: [
      {
        source: "/legacy/:slug",
        destination: "/blog/:slug",
      },
    ],
    fallback: [
      {
        source: "/:path*",
        destination: "https://legacy.example.com/:path*",
      },
    ],
  };
}
```

This ordering is especially useful during migrations from legacy applications.

---

## 8. Production Example

Suppose you're migrating from an older website.

```
Old URLs

/blog/*
/products/*
/support/*
```

New application:

```
/articles/*
/shop/*
/help/*
```

Configuration:

```ts
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  async rewrites() {
    return [
      {
        source: "/blog/:slug",
        destination: "/articles/:slug",
      },
      {
        source: "/products/:id",
        destination: "/shop/:id",
      },
      {
        source: "/support/:path*",
        destination: "/help/:path*",
      },
    ];
  },
};

export default nextConfig;
```

Users keep using the old URLs while the application serves content from the new routes.

---

## 9. Performance Considerations

- Rewrites are resolved at the routing layer and are generally efficient.
- Use rewrites to maintain clean, user-friendly URLs while reorganizing your application's internal structure.
- Avoid chaining many rewrites, as they can make routing harder to understand and debug.
- For API proxying, consider latency and caching behavior of the upstream service.
- Document rewrite rules clearly, especially in large applications or during migrations.

---

## 10. Common Pitfalls & Solutions

### ❌ Using rewrites instead of redirects

If the URL should visibly change (for SEO, canonical URLs, or permanent moves), use a redirect instead.

---

### ❌ Conflicting rewrite rules

Overlapping `source` patterns can produce unexpected routing. Order your rules carefully and use `beforeFiles`, `afterFiles`, or `fallback` appropriately.

---

### ❌ Forgetting parameter forwarding

Incorrect:

```ts
destination: "/articles";
```

Correct:

```ts
destination: "/articles/:slug";
```

Otherwise, route parameters are lost.

---

### ❌ Assuming rewrites enforce security

Rewrites only change routing. They do **not** provide authentication or authorization. Protect sensitive routes with middleware or server-side checks.

---

# **Code (Production-Ready)**

```ts
// next.config.ts
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  async rewrites() {
    return {
      beforeFiles: [
        {
          source: "/blog/:slug",
          destination: "/articles/:slug",
        },
      ],
      afterFiles: [
        {
          source: "/product/:id",
          destination: "/shop/products/:id",
        },
      ],
      fallback: [
        {
          source: "/api/:path*",
          destination: "https://backend.example.com/:path*",
        },
      ],
    };
  },
};

export default nextConfig;
```

---

## **When to Use**

Use rewrites when you need to:

- Maintain legacy URLs during migrations
- Create cleaner, more user-friendly URLs
- Proxy requests to external APIs or backend services
- Support versioned APIs (e.g., `/v1/*` → `/api/*`)
- Implement multi-tenant or conditional routing based on headers, cookies, or hostnames

---

## **Alternatives**

| Feature            | URL Changes? | Best For                                                                     |
| ------------------ | ------------ | ---------------------------------------------------------------------------- |
| **Rewrites**       | ❌ No        | Internal routing, API proxying, legacy URL support                           |
| **Redirects**      | ✅ Yes       | Permanent (301/308) or temporary (302/307) URL changes                       |
| **Middleware**     | Optional     | Authentication, localization, A/B testing, request inspection before routing |
| **Route Handlers** | N/A          | Implementing server-side API endpoints within the App Router                 |

> **Interview Tip:** A common interview question is the difference between rewrites and redirects. The key distinction is that **rewrites serve content from a different destination while keeping the original URL visible**, whereas **redirects instruct the browser to navigate to a new URL**, making them the appropriate choice for SEO-driven URL changes.

## Question 2. How do you implement custom server logic in Next.js?

## Question 3. How do you configure a custom Express.js server with Next.js?

## Question 4. How do you implement SSR with authentication cookies?

## Question 5. How do you handle protected API routes?

## Question 6. How do you implement throttling or rate-limiting in API routes?

## Question 7. How do you use SWR for client-side data fetching?

## Question 8. How do you implement polling with SWR?

## Question 9. How can you implement optimistic UI updates in Next.js?

## Question 10. How do you use Next.js with React Query?

## Question 11. How can you integrate Next.js with Firebase?

## Question 12. How can you integrate Next.js with Supabase?

## Question 13. How do you implement serverless functions in Next.js?

## Question 14. How do you handle image optimization for external URLs?

## Question 15. How do you use the `<Script>` component in Next.js?

## Question 16. How do you lazy load third-party scripts?

## Question 17. How do you handle internationalized routing (i18n)?

## Question 18. How do you implement cookie consent banners?

## Question 19. How do you implement persistent layouts across pages?

## Question 20. How do you handle nested API routes?
