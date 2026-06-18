# Set 23

| #   | Question                                                                                                                                                         |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [How do you implement conditional rendering based on user roles?](#question-1-how-do-you-implement-conditional-rendering-based-on-user-roles)                    |
| 2   | [How do you implement server-side redirection for banned users?](#question-2-how-do-you-implement-server-side-redirection-for-banned-users)                      |
| 3   | [How do you implement dynamic theming using SSR and context?](#question-3-how-do-you-implement-dynamic-theming-using-ssr-and-context)                            |
| 4   | [How do you implement prefetching for dynamic routes?](#question-4-how-do-you-implement-prefetching-for-dynamic-routes)                                          |
| 5   | [How do you handle 404 pages for dynamic paths?](#question-5-how-do-you-handle-404-pages-for-dynamic-paths)                                                      |
| 6   | [How do you implement a CMS preview mode for drafts?](#question-6-how-do-you-implement-a-cms-preview-mode-for-drafts)                                            |
| 7   | [How do you implement form submission with server-side validation?](#question-7-how-do-you-implement-form-submission-with-server-side-validation)                |
| 8   | [How do you implement optimistic UI updates for server mutations?](#question-8-how-do-you-implement-optimistic-ui-updates-for-server-mutations)                  |
| 9   | [How do you implement SWR caching with revalidation intervals?](#question-9-how-do-you-implement-swr-caching-with-revalidation-intervals)                        |
| 10  | [How do you implement a client-side search with SSR fallback?](#question-10-how-do-you-implement-a-client-side-search-with-ssr-fallback)                         |
| 11  | [How do you implement multi-step forms with server validation?](#question-11-how-do-you-implement-multi-step-forms-with-server-validation)                       |
| 12  | [How do you implement dynamic imports with a loading fallback?](#question-12-how-do-you-implement-dynamic-imports-with-a-loading-fallback)                       |
| 13  | [How do you implement middleware for A/B testing?](#question-13-how-do-you-implement-middleware-for-ab-testing)                                                  |
| 14  | [How do you implement dynamic Open Graph images per page?](#question-14-how-do-you-implement-dynamic-open-graph-images-per-page)                                 |
| 15  | [How do you implement server-side session invalidation?](#question-15-how-do-you-implement-server-side-session-invalidation)                                     |
| 16  | [How do you implement client-side token refresh?](#question-16-how-do-you-implement-client-side-token-refresh)                                                   |
| 17  | [How do you implement SSR with third-party APIs requiring authentication?](#question-17-how-do-you-implement-ssr-with-third-party-apis-requiring-authentication) |
| 18  | [How do you integrate Next.js with a headless CMS for SSR content?](#question-18-how-do-you-integrate-nextjs-with-a-headless-cms-for-ssr-content)                |
| 19  | [How do you implement dynamic sitemap generation?](#question-19-how-do-you-implement-dynamic-sitemap-generation)                                                 |
| 20  | [How do you implement SSR caching using Cache-Control headers?](#question-20-how-do-you-implement-ssr-caching-using-cache-control-headers)                       |

## Question 1. How do you implement conditional rendering based on user roles?

**Q: How do you implement conditional rendering based on user roles?**

**Short Answer (30 seconds):**

In **Next.js 16**, role-based rendering should primarily happen on the **server** by checking the authenticated user's role in a **Server Component** or **Server Action**. Client-side conditional rendering is useful only for improving the UI experience and should never be relied on for security, since client-side code can be modified by users.

---

# Detailed Explanation

## 1. Core Concept (Next.js 16 + App Router)

Role-Based Access Control (RBAC) means showing or hiding UI and restricting access based on the authenticated user's role.

Common roles include:

- Admin
- Manager
- Editor
- User
- Guest

For example:

| Role    | Permissions            |
| ------- | ---------------------- |
| Admin   | Everything             |
| Manager | Manage users & reports |
| Editor  | Edit content           |
| User    | Read own data          |

In Next.js App Router, authentication information is usually retrieved:

- from cookies
- from JWT
- from the session
- using authentication libraries like Auth.js or Clerk

The important rule is:

> **Authorization must happen on the server. UI hiding alone is not authorization.**

**Docs Reference:**

- Next.js 16 → App Router → Server Components
- Next.js → Authentication
- React → Conditional Rendering

---

# 2. Server-side Role Check (Recommended)

A Server Component can fetch the authenticated session and render different UI.

```tsx
// app/dashboard/page.tsx

import { getCurrentUser } from "@/lib/auth";

export default async function Dashboard() {
  const user = await getCurrentUser();

  return (
    <>
      <h1>Dashboard</h1>

      {user.role === "admin" && <a href="/admin">Admin Panel</a>}

      {user.role === "editor" && <a href="/posts">Manage Posts</a>}

      <p>Welcome {user.name}</p>
    </>
  );
}
```

Since this executes on the server:

- secure
- fast
- no role information exposed unnecessarily
- prevents unauthorized UI from being rendered

---

# 3. Client-side Conditional Rendering

Sometimes role information is already available in a Client Component.

```tsx
"use client";

type User = {
  role: "admin" | "editor" | "user";
};

export default function Navbar({ user }: { user: User }) {
  return (
    <nav>
      <a href="/">Home</a>

      {user.role === "admin" && <a href="/admin">Admin</a>}

      {user.role === "editor" && <a href="/posts">Posts</a>}
    </nav>
  );
}
```

This improves UX but **must not** be considered a security mechanism.

---

# 4. Creating a Reusable RoleGuard Component

Instead of repeating role checks everywhere, create a reusable component.

```tsx
type RoleGuardProps = {
  role: string;
  allowedRoles: string[];
  children: React.ReactNode;
};

export function RoleGuard({ role, allowedRoles, children }: RoleGuardProps) {
  if (!allowedRoles.includes(role)) {
    return null;
  }

  return <>{children}</>;
}
```

Usage:

```tsx
<RoleGuard role={user.role} allowedRoles={["admin"]}>
  <AdminPanel />
</RoleGuard>
```

Benefits:

- reusable
- cleaner code
- centralized logic
- easier maintenance

---

# 5. Protecting Entire Pages

UI hiding isn't enough. Protect routes on the server.

```tsx
import { redirect } from "next/navigation";
import { getCurrentUser } from "@/lib/auth";

export default async function AdminPage() {
  const user = await getCurrentUser();

  if (user.role !== "admin") {
    redirect("/unauthorized");
  }

  return <h1>Admin Dashboard</h1>;
}
```

Even if someone manually enters:

```
/admin
```

they still cannot access the page.

---

# 6. Middleware for Route Protection

For application-wide protection, use middleware to intercept requests before they reach your pages.

```ts
// middleware.ts

import { NextResponse } from "next/server";
import type { NextRequest } from "next/server";

export function middleware(request: NextRequest) {
  const role = request.cookies.get("role")?.value;

  if (request.nextUrl.pathname.startsWith("/admin") && role !== "admin") {
    return NextResponse.redirect(new URL("/login", request.url));
  }

  return NextResponse.next();
}

export const config = {
  matcher: ["/admin/:path*"],
};
```

In production, avoid trusting a plain cookie for authorization. Instead, validate a signed session or JWT before making authorization decisions.

---

# 7. Performance Considerations

- Prefer **Server Components** for authorization checks to avoid exposing unnecessary UI.
- Fetch the authenticated user once and pass only the required role information to Client Components.
- Encapsulate repeated permission checks in reusable helpers or guard components.
- Protect sensitive pages with server-side checks or middleware before rendering.
- Keep role information lightweight to minimize client-side hydration.

---

# 8. Common Pitfalls & Solutions

| Pitfall                                   | Solution                                                            |
| ----------------------------------------- | ------------------------------------------------------------------- |
| Hiding buttons only                       | Always enforce authorization on the server.                         |
| Checking roles only in the client         | Validate roles in Server Components, Route Handlers, or Middleware. |
| Duplicating role logic                    | Use shared authorization utilities or guard components.             |
| Trusting client-modified data             | Verify sessions or JWTs on the server.                              |
| Hardcoding permissions throughout the app | Centralize role and permission definitions.                         |

---

## Code

```tsx
// app/admin/page.tsx

import { redirect } from "next/navigation";
import { getCurrentUser } from "@/lib/auth";

export default async function AdminPage() {
  const user = await getCurrentUser();

  if (!user || user.role !== "admin") {
    redirect("/unauthorized");
  }

  return (
    <main>
      <h1>Admin Dashboard</h1>

      <p>Welcome, {user.name}</p>
    </main>
  );
}
```

---

**When to use:**

- Admin dashboards
- CMS applications
- SaaS products with multiple user roles
- Enterprise portals
- E-commerce admin panels
- Internal business tools
- Multi-tenant applications with varying permissions

---

**Alternatives:**

- **Server Components (Recommended):** Best for secure role checks and rendering protected content.
- **Client Components:** Suitable for non-sensitive UI personalization after the server has already authorized access.
- **Middleware:** Ideal for protecting entire groups of routes before rendering.
- **Route Handlers / Server Actions:** Use for enforcing authorization on API requests and server-side mutations, ensuring sensitive operations remain protected regardless of the client UI.

## Question 2. How do you implement server-side redirection for banned users?

## Question 3. How do you implement dynamic theming using SSR and context?

## Question 4. How do you implement prefetching for dynamic routes?

## Question 5. How do you handle 404 pages for dynamic paths?

## Question 6. How do you implement a CMS preview mode for drafts?

## Question 7. How do you implement form submission with server-side validation?

## Question 8. How do you implement optimistic UI updates for server mutations?

## Question 9. How do you implement SWR caching with revalidation intervals?

## Question 10. How do you implement a client-side search with SSR fallback?

## Question 11. How do you implement multi-step forms with server validation?

## Question 12. How do you implement dynamic imports with a loading fallback?

## Question 13. How do you implement middleware for A/B testing?

## Question 14. How do you implement dynamic Open Graph images per page?

## Question 15. How do you implement server-side session invalidation?

## Question 16. How do you implement client-side token refresh?

## Question 17. How do you implement SSR with third-party APIs requiring authentication?

## Question 18. How do you integrate Next.js with a headless CMS for SSR content?

## Question 19. How do you implement dynamic sitemap generation?

## Question 20. How do you implement SSR caching using Cache-Control headers?
