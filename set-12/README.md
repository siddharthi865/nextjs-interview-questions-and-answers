# Set 12

| #   | Question                                                                                                                                                     |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1   | [How do you handle 500 server errors gracefully?](#question-1-how-do-you-handle-500-server-errors-gracefully)                                                |
| 2   | [How do you add meta tags for social sharing (Open Graph)?](#question-2-how-do-you-add-meta-tags-for-social-sharing-open-graph)                              |
| 3   | [How do you handle optional route parameters?](#question-3-how-do-you-handle-optional-route-parameters)                                                      |
| 4   | [How do you implement a robots.txt file in Next.js?](#question-4-how-do-you-implement-a-robotstxt-file-in-nextjs)                                            |
| 5   | [How do you enable React Strict Mode in next.config.js?](#question-5-how-do-you-enable-react-strict-mode-in-nextconfigjs)                                    |
| 6   | [How do you add a manifest file for PWA support?](#question-6-how-do-you-add-a-manifest-file-for-pwa-support)                                                |
| 7   | [How do you include Google Fonts in Next.js?](#question-7-how-do-you-include-google-fonts-in-nextjs)                                                         |
| 8   | [How do you implement a simple navigation menu?](#question-8-how-do-you-implement-a-simple-navigation-menu)                                                  |
| 9   | [How do you handle user input and state in a functional component?](#question-9-how-do-you-handle-user-input-and-state-in-a-functional-component)            |
| 10  | [How do you add a default layout for all pages?](#question-10-how-do-you-add-a-default-layout-for-all-pages)                                                 |
| 11  | [How do you implement server-side authentication in getServerSideProps?](#question-11-how-do-you-implement-server-side-authentication-in-getserversideprops) |
| 12  | [How do you handle API rate limiting in Next.js API routes?](#question-12-how-do-you-handle-api-rate-limiting-in-nextjs-api-routes)                          |
| 13  | [How do you implement server-side redirects?](#question-13-how-do-you-implement-server-side-redirects)                                                       |
| 14  | [How do you implement client-side redirects after login?](#question-14-how-do-you-implement-client-side-redirects-after-login)                               |
| 15  | [How do you create a reusable card component with props?](#question-15-how-do-you-create-a-reusable-card-component-with-props)                               |
| 16  | [How do you implement infinite scroll with Next.js?](#question-16-how-do-you-implement-infinite-scroll-with-nextjs)                                          |
| 17  | [How do you implement search functionality with SSR?](#question-17-how-do-you-implement-search-functionality-with-ssr)                                       |
| 18  | [How do you cache API responses in getServerSideProps?](#question-18-how-do-you-cache-api-responses-in-getserversideprops)                                   |
| 19  | [How do you fetch data from multiple APIs in parallel?](#question-19-how-do-you-fetch-data-from-multiple-apis-in-parallel)                                   |
| 20  | [How do you integrate Next.js with Axios interceptors?](#question-20-how-do-you-integrate-nextjs-with-axios-interceptors)                                    |

## Question 1. How do you handle 500 server errors gracefully?

## **Q: How do you handle 500 server errors gracefully in Next.js?**

### **Short Answer (30 seconds):**

In **Next.js 16**, the recommended way to handle **500 server errors** is by using **`error.tsx`** route segment error boundaries for the App Router, combined with proper server-side error logging, graceful fallback UI, and retry mechanisms. In production, errors should be logged to monitoring tools like Sentry while users see a friendly error page instead of a stack trace.

---

# **Detailed Explanation**

## 1. Use `error.tsx` (App Router Error Boundary)

In the App Router, every route segment can have an **`error.tsx`** file.

Whenever a Server Component, Server Action, or rendering logic throws an exception, Next.js automatically renders this error boundary instead of crashing the application.

Directory structure:

```text
app/
 ├── dashboard/
 │    ├── page.tsx
 │    ├── error.tsx
 │    └── loading.tsx
```

This isolates failures to a specific route instead of affecting the entire application.

**Docs:** Next.js → App Router → Error Handling (`error.tsx`)

---

## 2. Create a User-Friendly Error UI

Instead of exposing technical details, show:

- Friendly message
- Retry button
- Back/Home navigation
- Optional support contact

Example:

```tsx
"use client";

import { useEffect } from "react";

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  useEffect(() => {
    console.error(error);
  }, [error]);

  return (
    <div>
      <h2>Something went wrong.</h2>

      <button onClick={() => reset()}>Try Again</button>
    </div>
  );
}
```

`reset()` retries rendering the route without requiring a full page refresh.

---

## 3. Throw Errors from Server Components

Example:

```tsx
async function getUsers() {
  const res = await fetch("https://api.example.com/users");

  if (!res.ok) {
    throw new Error("Failed to fetch users");
  }

  return res.json();
}

export default async function Page() {
  const users = await getUsers();

  return <UsersList users={users} />;
}
```

If the fetch fails with a server error, Next.js renders `error.tsx`.

---

## 4. Handle Errors in Server Actions

```tsx
"use server";

export async function createPost(data: FormData) {
  try {
    // Database call
  } catch (err) {
    console.error(err);
    throw new Error("Unable to create post");
  }
}
```

The nearest `error.tsx` handles unexpected failures. For expected validation errors, prefer returning structured error data rather than throwing exceptions.

---

## 5. Log Errors in Production

Never rely solely on `console.error`.

Production applications should send errors to monitoring platforms like:

- Sentry
- Datadog
- New Relic
- Azure Application Insights

Example:

```tsx
useEffect(() => {
  console.error(error);

  // Sentry.captureException(error);
}, [error]);
```

This lets developers investigate issues while users receive a clean experience.

---

## 6. Add a Global Error Boundary

For application-wide failures, create:

```text
app/global-error.tsx
```

This catches errors that occur outside individual route segments.

Example:

```tsx
"use client";

export default function GlobalError({
  error,
  reset,
}: {
  error: Error;
  reset: () => void;
}) {
  return (
    <html>
      <body>
        <h1>Application Error</h1>
        <button onClick={reset}>Retry</button>
      </body>
    </html>
  );
}
```

Use this for catastrophic rendering failures.

---

## 7. Avoid Exposing Internal Errors

Do **not** display:

- Stack traces
- SQL errors
- API keys
- File paths
- Database messages

Instead:

```text
Something went wrong.
Please try again later.
```

Log the detailed error on the server for debugging.

---

## 8. Graceful API Route Handling

For Route Handlers:

```tsx
import { NextResponse } from "next/server";

export async function GET() {
  try {
    return NextResponse.json({
      success: true,
    });
  } catch (err) {
    console.error(err);

    return NextResponse.json(
      { message: "Internal Server Error" },
      { status: 500 },
    );
  }
}
```

Clients receive a consistent error response without leaking implementation details.

---

## 9. Performance Considerations

- Keep `error.tsx` lightweight so it renders quickly.
- Use **route-level** error boundaries to isolate failures.
- Log asynchronously to avoid delaying responses.
- Combine `loading.tsx` with `error.tsx` for a smoother user experience.
- Use retries (`reset()`) for transient issues, but avoid infinite retry loops.

---

## 10. Common Pitfalls & Solutions

| Pitfall                                          | Solution                                                                              |
| ------------------------------------------------ | ------------------------------------------------------------------------------------- |
| Showing stack traces to users                    | Display friendly messages; log details server-side                                    |
| Wrapping the entire app in one error boundary    | Prefer route-segment `error.tsx` files for better isolation                           |
| Throwing errors for expected validation failures | Return structured validation errors instead                                           |
| Not monitoring production errors                 | Integrate with an error tracking service                                              |
| Assuming `error.tsx` catches everything          | Use `global-error.tsx` for application-wide failures and handle API errors explicitly |

---

# **Code (Production-Ready Example)**

### `app/dashboard/error.tsx`

```tsx
"use client";

import { useEffect } from "react";

type ErrorProps = {
  error: Error & { digest?: string };
  reset: () => void;
};

export default function Error({ error, reset }: ErrorProps) {
  useEffect(() => {
    console.error(error);
    // Send to Sentry, Datadog, etc.
  }, [error]);

  return (
    <div className="p-6">
      <h2>Something went wrong.</h2>
      <p>Please try again.</p>

      <button
        onClick={() => reset()}
        className="mt-4 rounded bg-blue-600 px-4 py-2 text-white"
      >
        Retry
      </button>
    </div>
  );
}
```

---

## **When to Use**

Use this approach when:

- Building production applications with the App Router.
- Handling unexpected rendering failures in Server Components.
- Managing Server Action exceptions.
- Providing resilient route-level error recovery.
- Integrating with monitoring tools for observability.

---

## **Alternatives**

| Approach                         | Best Use Case                                                                       |
| -------------------------------- | ----------------------------------------------------------------------------------- |
| **`error.tsx`**                  | Route-level rendering errors (recommended for App Router)                           |
| **`global-error.tsx`**           | Application-wide fallback for uncaught rendering errors                             |
| **Route Handlers (`try/catch`)** | API endpoints returning HTTP 500 responses                                          |
| **React Error Boundaries**       | Client Component rendering errors within the React tree                             |
| **Expected error returns**       | Validation failures or business logic where throwing an exception isn't appropriate |

### **Interview Tip**

A strong interview answer is: _"In Next.js 16, I use route-level `error.tsx` files to catch unexpected rendering errors, `global-error.tsx` for application-wide failures, and `try/catch` in Route Handlers and Server Actions. I never expose internal error details to users; instead, I log them to an observability platform like Sentry and present a friendly fallback UI with a retry option using `reset()`. This keeps the application resilient, secure, and user-friendly."_

## Question 2. How do you add meta tags for social sharing (Open Graph)?

## Question 3. How do you handle optional route parameters?

## Question 4. How do you implement a robots.txt file in Next.js?

## Question 5. How do you enable React Strict Mode in next.config.js?

## Question 6. How do you add a manifest file for PWA support?

## Question 7. How do you include Google Fonts in Next.js?

## Question 8. How do you implement a simple navigation menu?

## Question 9. How do you handle user input and state in a functional component?

## Question 10. How do you add a default layout for all pages?

## Question 11. How do you implement server-side authentication in getServerSideProps?

## Question 12. How do you handle API rate limiting in Next.js API routes?

## Question 13. How do you implement server-side redirects?

## Question 14. How do you implement client-side redirects after login?

## Question 15. How do you create a reusable card component with props?

## Question 16. How do you implement infinite scroll with Next.js?

## Question 17. How do you implement search functionality with SSR?

## Question 18. How do you cache API responses in getServerSideProps?

## Question 19. How do you fetch data from multiple APIs in parallel?

## Question 20. How do you integrate Next.js with Axios interceptors?
