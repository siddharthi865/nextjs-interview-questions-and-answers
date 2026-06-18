# Set 24

| #   | Question                                                                                                                                                        |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [How do you implement client-side route protection?](#question-1-how-do-you-implement-client-side-route-protection)                                             |
| 2   | [How do you implement server-side logging with external logging services?](#question-2-how-do-you-implement-server-side-logging-with-external-logging-services) |
| 3   | [How do you implement file download endpoints in API routes?](#question-3-how-do-you-implement-file-download-endpoints-in-api-routes)                           |
| 4   | [How do you implement conditional layouts per route dynamically?](#question-4-how-do-you-implement-conditional-layouts-per-route-dynamically)                   |
| 5   | [How do you implement persistent user preferences across SSR and CSR?](#question-5-how-do-you-implement-persistent-user-preferences-across-ssr-and-csr)         |
| 6   | [How do you implement multi-step SSR pages with preloaded data?](#question-6-how-do-you-implement-multi-step-ssr-pages-with-preloaded-data)                     |
| 7   | [How do you implement SSR fallback pages for missing dynamic data?](#question-7-how-do-you-implement-ssr-fallback-pages-for-missing-dynamic-data)               |
| 8   | [How do you integrate SSR pages with analytics tracking?](#question-8-how-do-you-integrate-ssr-pages-with-analytics-tracking)                                   |
| 9   | [How do you implement automatic ISR updates using webhooks?](#question-9-how-do-you-implement-automatic-isr-updates-using-webhooks)                             |
| 10  | [How do you implement complex query filtering for SSR pages?](#question-10-how-do-you-implement-complex-query-filtering-for-ssr-pages)                          |
| 11  | [How do you implement hybrid SSR + SSG in a single page?](#question-11-how-do-you-implement-hybrid-ssr--ssg-in-a-single-page)                                   |
| 12  | [How do you implement server-side streaming of large content?](#question-12-how-do-you-implement-server-side-streaming-of-large-content)                        |
| 13  | [How do you implement partial hydration for interactive components?](#question-13-how-do-you-implement-partial-hydration-for-interactive-components)            |
| 14  | [How do you implement React Server Components for complex dashboards?](#question-14-how-do-you-implement-react-server-components-for-complex-dashboards)        |
| 15  | [How do you implement nested layouts using the app router?](#question-15-how-do-you-implement-nested-layouts-using-the-app-router)                              |
| 16  | [How do you implement edge caching with dynamic personalization?](#question-16-how-do-you-implement-edge-caching-with-dynamic-personalization)                  |
| 17  | [How do you implement multi-tenant architecture with shared SSR routes?](#question-17-how-do-you-implement-multi-tenant-architecture-with-shared-ssr-routes)    |
| 18  | [How do you implement feature flags using middleware at the edge?](#question-18-how-do-you-implement-feature-flags-using-middleware-at-the-edge)                |
| 19  | [How do you implement real-time dashboards with WebSockets?](#question-19-how-do-you-implement-real-time-dashboards-with-websockets)                            |
| 20  | [How do you integrate SSR with GraphQL subscriptions?](#question-20-how-do-you-integrate-ssr-with-graphql-subscriptions)                                        |

## Question 1. How do you implement client-side route protection?

## **Q: How do you implement client-side route protection?**

### **Short Answer (30 seconds):**

Client-side route protection in Next.js is implemented by checking the user's authentication state inside a Client Component and redirecting unauthenticated users using `useRouter()` or `redirect()`. However, client-side protection is only for improving user experience—it is **not secure by itself**. Sensitive pages and data must always be protected on the server using Middleware, Server Components, or your authentication provider.

---

# **Detailed Explanation**

## **1. Core Concept**

In **Next.js 16 (App Router)**, client-side route protection is useful when:

- Hiding authenticated UI until the user logs in.
- Preventing navigation to protected pages after hydration.
- Improving UX by redirecting users quickly.

Typical flow:

```
User visits /dashboard
        │
        ▼
Check auth state (JWT/Cookie/Session)
        │
 ┌──────┴────────┐
 │               │
Authenticated   Not Authenticated
 │               │
 ▼               ▼
Render page   Redirect to /login
```

> **Important Interview Point:**
>
> Client-side protection **does not prevent someone from requesting server data directly**. The server must always verify authentication.

**Next.js Docs:**

- App Router
- Authentication
- Client Components
- Navigation (`useRouter`)

---

# **2. Implementation Approach**

A common pattern is:

1. Store authentication state (Context, Redux, Zustand, NextAuth, Clerk, Auth.js, etc.).
2. Create a reusable `ProtectedRoute` component.
3. Check authentication on mount.
4. Redirect unauthenticated users.
5. Show a loading spinner while authentication is being determined.

---

# **3. Production Example**

### `components/ProtectedRoute.tsx`

```tsx
"use client";

import { useEffect } from "react";
import { useRouter } from "next/navigation";
import { useAuth } from "@/context/AuthContext";

type Props = {
  children: React.ReactNode;
};

export default function ProtectedRoute({ children }: Props) {
  const { user, loading } = useAuth();
  const router = useRouter();

  useEffect(() => {
    if (!loading && !user) {
      router.replace("/login");
    }
  }, [loading, user, router]);

  if (loading) {
    return <p>Loading...</p>;
  }

  if (!user) {
    return null;
  }

  return <>{children}</>;
}
```

---

### Using it in a page

```tsx
import ProtectedRoute from "@/components/ProtectedRoute";

export default function DashboardPage() {
  return (
    <ProtectedRoute>
      <h1>Dashboard</h1>
      <p>Only authenticated users can see this.</p>
    </ProtectedRoute>
  );
}
```

---

# **4. Auth Context Example**

```tsx
"use client";

import { createContext, useContext, useEffect, useState } from "react";

type User = {
  id: string;
  name: string;
};

type AuthContextType = {
  user: User | null;
  loading: boolean;
};

const AuthContext = createContext<AuthContextType>({
  user: null,
  loading: true,
});

export function AuthProvider({ children }: { children: React.ReactNode }) {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    async function fetchUser() {
      const res = await fetch("/api/me");

      if (res.ok) {
        setUser(await res.json());
      }

      setLoading(false);
    }

    fetchUser();
  }, []);

  return (
    <AuthContext.Provider value={{ user, loading }}>
      {children}
    </AuthContext.Provider>
  );
}

export const useAuth = () => useContext(AuthContext);
```

---

# **5. Performance Considerations**

### ✅ Show a loading state

Without it, protected content may briefly appear before redirecting (known as a **flash of protected content**).

---

### ✅ Use `router.replace()`

```tsx
router.replace("/login");
```

instead of

```tsx
router.push("/login");
```

This prevents users from navigating back to the protected page using the browser's Back button.

---

### ✅ Cache authentication

Instead of fetching the user on every page, cache the session using libraries like React Query, SWR, or your authentication provider to reduce unnecessary network requests.

---

### ✅ Prefer Server Components for protected data

Even if the page is protected on the client, fetch sensitive data only after server-side authentication checks.

---

# **5. Common Pitfalls & Solutions**

### ❌ Protecting only on the client

```tsx
if (!user) router.push("/login");
```

A user can still inspect the page bundle or call APIs directly.

**Solution:**

Protect APIs, Server Components, or use Middleware to enforce authentication.

---

### ❌ Flashing protected content

Rendering the page before the auth check completes can expose content briefly.

**Solution:**

Render a loading indicator until authentication is resolved.

---

### ❌ Using `push()` instead of `replace()`

`push()` keeps the protected page in browser history.

**Solution:**

Use:

```tsx
router.replace("/login");
```

---

### ❌ Fetching sensitive data before authentication

Avoid requesting protected data before verifying the user's session.

**Solution:**

Authenticate first, then fetch protected resources.

---

# **Production Best Practice (Next.js 16)**

For secure applications, combine client-side protection with server-side enforcement:

- **Client-side:** Improve UX by redirecting unauthenticated users and hiding protected UI.
- **Server-side:** Validate sessions in Server Components, Route Handlers, or Middleware before returning sensitive content.
- **APIs:** Always verify authentication and authorization on every protected endpoint.

This layered approach provides both a smooth user experience and robust security.

---

## **When to use**

Use client-side route protection when:

- Redirecting unauthenticated users after hydration.
- Protecting client-rendered dashboards or settings pages.
- Hiding authenticated UI elements.
- Improving navigation experience.

---

## **Alternatives**

| Approach                                        | Best For                                               | Security   |
| ----------------------------------------------- | ------------------------------------------------------ | ---------- |
| Client-side (`useRouter`)                       | UX improvements and client-rendered pages              | ⭐⭐☆☆☆    |
| Server Component auth checks                    | Protecting server-rendered pages and data              | ⭐⭐⭐⭐⭐ |
| Middleware                                      | Blocking unauthorized access before rendering          | ⭐⭐⭐⭐⭐ |
| Authentication libraries (e.g., Auth.js, Clerk) | Production-ready authentication and session management | ⭐⭐⭐⭐⭐ |

> **Interview takeaway:** Client-side route protection is useful for user experience, but it is **not a security mechanism**. In production, always enforce authentication and authorization on the server using Server Components, Middleware, or protected API routes, and use client-side redirects only as a complementary UX enhancement.

## Question 2. How do you implement server-side logging with external logging services?

## Question 3. How do you implement file download endpoints in API routes?

## Question 4. How do you implement conditional layouts per route dynamically?

## Question 5. How do you implement persistent user preferences across SSR and CSR?

## Question 6. How do you implement multi-step SSR pages with preloaded data?

## Question 7. How do you implement SSR fallback pages for missing dynamic data?

## Question 8. How do you integrate SSR pages with analytics tracking?

## Question 9. How do you implement automatic ISR updates using webhooks?

## Question 10. How do you implement complex query filtering for SSR pages?

## Question 11. How do you implement hybrid SSR + SSG in a single page?

## Question 12. How do you implement server-side streaming of large content?

## Question 13. How do you implement partial hydration for interactive components?

## Question 14. How do you implement React Server Components for complex dashboards?

## Question 15. How do you implement nested layouts using the app router?

## Question 16. How do you implement edge caching with dynamic personalization?

## Question 17. How do you implement multi-tenant architecture with shared SSR routes?

## Question 18. How do you implement feature flags using middleware at the edge?

## Question 19. How do you implement real-time dashboards with WebSockets?

## Question 20. How do you integrate SSR with GraphQL subscriptions?
