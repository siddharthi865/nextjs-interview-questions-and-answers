# Set 25

| #   | Question                                                                                                                                                                            |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [How do you implement edge authentication with token verification?](#question-1-how-do-you-implement-edge-authentication-with-token-verification)                                   |
| 2   | [How do you implement partial caching for high-traffic pages?](#question-2-how-do-you-implement-partial-caching-for-high-traffic-pages)                                             |
| 3   | [How do you implement server-side image generation dynamically?](#question-3-how-do-you-implement-server-side-image-generation-dynamically)                                         |
| 4   | [How do you implement AB testing for personalized SSR content?](#question-4-how-do-you-implement-ab-testing-for-personalized-ssr-content)                                           |
| 5   | [How do you implement multi-language support with server-side redirects?](#question-5-how-do-you-implement-multi-language-support-with-server-side-redirects)                       |
| 6   | [How do you implement incremental builds for very large SSG sites?](#question-6-how-do-you-implement-incremental-builds-for-very-large-ssg-sites)                                   |
| 7   | [How do you implement server-side rate-limiting for high-traffic APIs?](#question-7-how-do-you-implement-server-side-rate-limiting-for-high-traffic-apis)                           |
| 8   | [How do you implement SSR monitoring for performance bottlenecks?](#question-8-how-do-you-implement-ssr-monitoring-for-performance-bottlenecks)                                     |
| 9   | [How do you debug memory leaks in SSR components?](#question-9-how-do-you-debug-memory-leaks-in-ssr-components)                                                                     |
| 10  | [How do you implement optimized bundle splitting for large libraries?](#question-10-how-do-you-implement-optimized-bundle-splitting-for-large-libraries)                            |
| 11  | [How do you implement background tasks triggered from API routes?](#question-11-how-do-you-implement-background-tasks-triggered-from-api-routes)                                    |
| 12  | [How do you integrate edge caching with Vercel or Cloudflare?](#question-12-how-do-you-integrate-edge-caching-with-vercel-or-cloudflare)                                            |
| 13  | [How do you implement hybrid CSR + SSR for personalized content?](#question-13-how-do-you-implement-hybrid-csr--ssr-for-personalized-content)                                       |
| 14  | [How do you implement server-side analytics tracking?](#question-14-how-do-you-implement-server-side-analytics-tracking)                                                            |
| 15  | [How do you implement secure cookie handling in SSR and CSR?](#question-15-how-do-you-implement-secure-cookie-handling-in-ssr-and-csr)                                              |
| 16  | [How do you implement authentication and role-based authorization at the edge?](#question-16-how-do-you-implement-authentication-and-role-based-authorization-at-the-edge)          |
| 17  | [How do you implement server-side rendering for real-time chat apps?](#question-17-how-do-you-implement-server-side-rendering-for-real-time-chat-apps)                              |
| 18  | [How do you implement efficient data fetching with ISR and caching?](#question-18-how-do-you-implement-efficient-data-fetching-with-isr-and-caching)                                |
| 19  | [How do you optimize SSR pages for SEO and performance?](#question-19-how-do-you-optimize-ssr-pages-for-seo-and-performance)                                                        |
| 20  | [What are the best practices for scaling Next.js applications to millions of users?](#question-20-what-are-the-best-practices-for-scaling-nextjs-applications-to-millions-of-users) |

## Question 1. How do you implement edge authentication with token verification?

**Q: How do you implement edge authentication with token verification?**

**Short Answer (30 seconds):**

In **Next.js 16**, edge authentication is typically implemented using **Middleware** running on the **Edge Runtime**. The middleware intercepts requests before they reach the application, verifies a JWT or session token, and either allows the request, redirects to login, or returns an unauthorized response. This approach reduces latency and protects routes globally without invoking your application server.

**Detailed Explanation:**

### 1. Core concept (Next.js 16 + Edge Runtime)

Next.js Middleware executes at the **Edge**, meaning authentication happens closer to the user before rendering the page or invoking API routes.

Typical authentication flow:

1. User logs in.
2. Server issues a signed JWT or session token.
3. Token is stored in an **HttpOnly secure cookie**.
4. Middleware reads the cookie.
5. Middleware verifies the token.
6. If valid → request continues.
7. If invalid or expired → redirect to login.

**Relevant Docs:**

- Next.js App Router → Middleware
- Next.js Authentication Guide
- Edge Runtime documentation

This is preferred because:

- Authentication happens before rendering.
- Reduces unnecessary server work.
- Protects entire route groups.
- Low latency due to Edge execution.

---

### 2. Implementation Approach

A production authentication flow looks like this:

```
Browser
     │
     ▼
Request → Middleware (Edge)
              │
     Read HttpOnly Cookie
              │
      Verify JWT Signature
              │
     ┌────────┴────────┐
     │                 │
   Valid            Invalid
     │                 │
     ▼                 ▼
Next.js Route      Redirect/Login
```

For JWT verification at the Edge:

- Store JWT in HttpOnly cookie.
- Use Edge-compatible libraries like **jose**.
- Avoid Node.js-only libraries because Middleware runs in the Edge Runtime.

---

### 3. Production Example

#### `middleware.ts`

```tsx
import { NextRequest, NextResponse } from "next/server";
import { jwtVerify } from "jose";

const secret = new TextEncoder().encode(process.env.JWT_SECRET);

export async function middleware(request: NextRequest) {
  const token = request.cookies.get("token")?.value;

  if (!token) {
    return NextResponse.redirect(new URL("/login", request.url));
  }

  try {
    await jwtVerify(token, secret);

    return NextResponse.next();
  } catch {
    return NextResponse.redirect(new URL("/login", request.url));
  }
}

export const config = {
  matcher: ["/dashboard/:path*", "/profile/:path*"],
};
```

---

#### Creating the JWT

```tsx
import { SignJWT } from "jose";

const token = await new SignJWT({
  userId: "123",
  role: "admin",
})
  .setProtectedHeader({ alg: "HS256" })
  .setIssuedAt()
  .setExpirationTime("1h")
  .sign(new TextEncoder().encode(process.env.JWT_SECRET));
```

---

#### Reading the authenticated user in a Server Component

```tsx
import { cookies } from "next/headers";
import { jwtVerify } from "jose";

export default async function DashboardPage() {
  const cookieStore = await cookies();
  const token = cookieStore.get("token")?.value;

  if (!token) {
    throw new Error("Unauthorized");
  }

  const { payload } = await jwtVerify(
    token,
    new TextEncoder().encode(process.env.JWT_SECRET),
  );

  return <h1>Welcome {payload.userId}</h1>;
}
```

---

### 4. Performance Considerations

**Use Edge Runtime**

- Authentication executes near users.
- Lower latency.
- Faster redirects.

**Verify lightweight JWTs**

- Keep token payload small.
- Avoid embedding large user objects.
- Store only essential claims like:
  - userId
  - role
  - permissions
  - expiration

**Use HttpOnly cookies**

Benefits:

- Prevents JavaScript access.
- Mitigates XSS attacks.
- Automatically sent with requests.

**Use short-lived access tokens**

For example:

- Access Token → 15–60 minutes
- Refresh Token → Longer lifetime with secure rotation

**Avoid database lookups in Middleware**

Middleware should remain fast. If you need additional user information, verify the token at the edge and fetch user data later in a Server Component or Route Handler when necessary.

---

### 5. Common Pitfalls & Solutions

**❌ Using `jsonwebtoken` in Middleware**

- Many Node.js crypto APIs are unavailable in the Edge Runtime.
- **Solution:** Use **`jose`**, which is Edge-compatible.

---

**❌ Storing JWT in `localStorage`**

- Vulnerable to XSS.
- **Solution:** Store tokens in HttpOnly cookies.

---

**❌ Putting sensitive data inside JWT**

JWT payloads are only Base64-encoded, not encrypted.

Store:

- User ID
- Role
- Permissions

Don't store:

- Passwords
- API keys
- Personal information

---

**❌ Forgetting token expiration**

Always validate:

- `exp`
- `nbf` (if used)
- Signature
- Issuer (`iss`)
- Audience (`aud`) when applicable

---

**❌ Protecting only frontend routes**

Always enforce authorization on:

- Route Handlers
- Server Actions
- Backend APIs

Middleware provides an initial gate, but backend endpoints should independently verify authentication and authorization.

---

### Production Architecture

```
User
 │
 ▼
Edge Middleware
 │
 ├── Read HttpOnly Cookie
 ├── Verify JWT Signature
 ├── Validate Expiry
 └── Check Basic Claims
        │
        ▼
Server Components
Route Handlers
Server Actions
        │
        ▼
Database/API
```

This layered approach ensures every sensitive operation remains protected even if a request bypasses client-side navigation.

---

**When to use:**

- Protecting dashboards and authenticated pages.
- Securing Route Handlers and Server Actions.
- Role-based access control using JWT claims.
- Low-latency global authentication with the Edge Runtime.
- Applications deployed on edge platforms where requests should be authenticated before reaching the application server.

**Alternatives:**

- **Middleware + JWT (Edge):** Best for stateless, globally distributed authentication with fast route protection.
- **Session-based authentication (e.g., database-backed):** Better when you need immediate session revocation or centralized session management.
- **Authentication libraries (e.g., Auth.js):** Ideal for production applications needing OAuth providers, session management, refresh token handling, and built-in integration with the App Router.
- **Server Component verification:** Suitable when only specific pages require authentication, though it does not prevent the request from reaching the application like Middleware does.

## Question 2. How do you implement partial caching for high-traffic pages?

## Question 3. How do you implement server-side image generation dynamically?

## Question 4. How do you implement AB testing for personalized SSR content?

## Question 5. How do you implement multi-language support with server-side redirects?

## Question 6. How do you implement incremental builds for very large SSG sites?

## Question 7. How do you implement server-side rate-limiting for high-traffic APIs?

## Question 8. How do you implement SSR monitoring for performance bottlenecks?

## Question 9. How do you debug memory leaks in SSR components?

## Question 10. How do you implement optimized bundle splitting for large libraries?

## Question 11. How do you implement background tasks triggered from API routes?

## Question 12. How do you integrate edge caching with Vercel or Cloudflare?

## Question 13. How do you implement hybrid CSR + SSR for personalized content?

## Question 14. How do you implement server-side analytics tracking?

## Question 15. How do you implement secure cookie handling in SSR and CSR?

## Question 16. How do you implement authentication and role-based authorization at the edge?

## Question 17. How do you implement server-side rendering for real-time chat apps?

## Question 18. How do you implement efficient data fetching with ISR and caching?

## Question 19. How do you optimize SSR pages for SEO and performance?

## Question 20. What are the best practices for scaling Next.js applications to millions of users?
