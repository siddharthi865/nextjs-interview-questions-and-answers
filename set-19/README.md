# Set 19

| #   | Question                                                                                                                                                                     |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [How do you implement form submission with server-side validation?](#question-1-how-do-you-implement-form-submission-with-server-side-validation)                            |
| 2   | [How do you implement optimistic UI updates for forms?](#question-2-how-do-you-implement-optimistic-ui-updates-for-forms)                                                    |
| 3   | [How do you integrate Next.js with Redux for global state management?](#question-3-how-do-you-integrate-nextjs-with-redux-for-global-state-management)                       |
| 4   | [How do you integrate Next.js with Zustand or Jotai for lightweight state?](#question-4-how-do-you-integrate-nextjs-with-zustand-or-jotai-for-lightweight-state)             |
| 5   | [How do you implement SWR caching strategies?](#question-5-how-do-you-implement-swr-caching-strategies)                                                                      |
| 6   | [How do you integrate Next.js with Axios interceptors?](#question-6-how-do-you-integrate-nextjs-with-axios-interceptors)                                                     |
| 7   | [How do you implement error boundaries in Next.js?](#question-7-how-do-you-implement-error-boundaries-in-nextjs)                                                             |
| 8   | [How do you implement breadcrumbs dynamically in SSR pages?](#question-8-how-do-you-implement-breadcrumbs-dynamically-in-ssr-pages)                                          |
| 9   | [How do you implement dynamic SEO tags per page?](#question-9-how-do-you-implement-dynamic-seo-tags-per-page)                                                                |
| 10  | [How do you integrate third-party analytics like Google Analytics or Plausible?](#question-10-how-do-you-integrate-third-party-analytics-like-google-analytics-or-plausible) |
| 11  | [How do you implement React Server Components in the new app router?](#question-11-how-do-you-implement-react-server-components-in-the-new-app-router)                       |
| 12  | [How do you implement nested layouts using Next.js 13+ app router?](#question-12-how-do-you-implement-nested-layouts-using-nextjs-13-app-router)                             |
| 13  | [How do you implement streaming SSR for large pages?](#question-13-how-do-you-implement-streaming-ssr-for-large-pages)                                                       |
| 14  | [How do you implement on-demand ISR revalidation at scale?](#question-14-how-do-you-implement-on-demand-isr-revalidation-at-scale)                                           |
| 15  | [How do you implement edge caching using Vercel or Netlify?](#question-15-how-do-you-implement-edge-caching-using-vercel-or-netlify)                                         |
| 16  | [How do you implement authentication at the edge with middleware?](#question-16-how-do-you-implement-authentication-at-the-edge-with-middleware)                             |
| 17  | [How do you implement feature flags in production Next.js apps?](#question-17-how-do-you-implement-feature-flags-in-production-nextjs-apps)                                  |
| 18  | [How do you implement multi-tenant architectures with SSR?](#question-18-how-do-you-implement-multi-tenant-architectures-with-ssr)                                           |
| 19  | [How do you implement high-performance real-time dashboards?](#question-19-how-do-you-implement-high-performance-real-time-dashboards)                                       |
| 20  | [How do you integrate Next.js with GraphQL subscriptions?](#question-20-how-do-you-integrate-nextjs-with-graphql-subscriptions)                                              |

## Question 1. How do you implement form submission with server-side validation?

**Q: How do you implement form submission with server-side validation?**

**Short Answer (30 seconds):**

In **Next.js 16**, the recommended approach is to use **Server Actions** for form submission and perform **validation on the server** before processing or saving data. Client-side validation improves UX, but **server-side validation is mandatory** because client-side checks can be bypassed.

---

# Detailed Explanation

## 1. Core Concept (Next.js 16 Approach)

The modern App Router approach is:

- Create a form in a **Server Component** or **Client Component**
- Submit it to a **Server Action**
- Validate the input on the server
- Return validation errors if any
- Save data only after successful validation
- Revalidate cache or redirect if needed

Next.js integrates this seamlessly with React Server Actions, eliminating the need for separate API routes in most cases.

**Official Docs:**

- App Router → Forms
- App Router → Server Actions
- App Router → Data Revalidation

---

## 2. Implementation Approach

A production flow looks like this:

```
User fills form
       │
       ▼
Client-side validation (optional)
       │
       ▼
Submit Form
       │
       ▼
Server Action
       │
       ▼
Validate using Zod
       │
 ┌─────┴────────┐
 │              │
Valid        Invalid
 │              │
 ▼              ▼
Save Data   Return Errors
 │
 ▼
Redirect/Revalidate
```

A common production stack:

- HTML Forms
- Server Actions
- TypeScript
- Zod
- Database (Prisma/Drizzle/etc.)

---

## 3. Production Example

### Step 1 — Validation Schema

```tsx
// lib/schema.ts

import { z } from "zod";

export const UserSchema = z.object({
  name: z.string().min(3, "Minimum 3 characters"),
  email: z.email("Invalid email"),
});
```

---

### Step 2 — Server Action

```tsx
// app/actions.ts

"use server";

import { UserSchema } from "@/lib/schema";
import { redirect } from "next/navigation";

export async function createUser(formData: FormData) {
  const result = UserSchema.safeParse({
    name: formData.get("name"),
    email: formData.get("email"),
  });

  if (!result.success) {
    return {
      success: false,
      errors: result.error.flatten().fieldErrors,
    };
  }

  // Save to database
  // await prisma.user.create({ data: result.data });

  redirect("/users");
}
```

Notice:

- Validation happens **before** database access.
- Invalid requests never reach business logic.

---

### Step 3 — Form

```tsx
// app/users/page.tsx

import { createUser } from "./actions";

export default function UserForm() {
  return (
    <form action={createUser}>
      <input name="name" placeholder="Name" />

      <input name="email" type="email" placeholder="Email" />

      <button type="submit">Create User</button>
    </form>
  );
}
```

---

### Step 4 — Display Errors (using `useActionState`)

```tsx
"use client";

import { useActionState } from "react";
import { createUser } from "./actions";

const initialState = {
  success: false,
  errors: {},
};

export default function UserForm() {
  const [state, formAction] = useActionState(createUser, initialState);

  return (
    <form action={formAction}>
      <input name="name" />

      {state.errors?.name && <p>{state.errors.name[0]}</p>}

      <input name="email" type="email" />

      {state.errors?.email && <p>{state.errors.email[0]}</p>}

      <button>Create</button>
    </form>
  );
}
```

This provides a great UX while keeping validation authoritative on the server.

---

# 4. Performance Considerations

### ✅ Use Server Actions

No extra API endpoint.

```
Browser
    │
Server Action
    │
Database
```

Less boilerplate and fewer network hops compared to calling a separate REST endpoint.

---

### ✅ Validate Before DB Calls

Always validate first.

Bad:

```
DB Query
↓

Validation
```

Good:

```
Validation
↓

DB Query
```

This reduces unnecessary database load.

---

### ✅ Use Zod

Benefits include:

- Type safety
- Shared schemas
- Consistent validation
- Better error messages
- Easy integration with TypeScript

---

### ✅ Revalidate Cache

After successful submission:

```tsx
import { revalidatePath } from "next/cache";

revalidatePath("/users");
```

This refreshes cached server-rendered data so users immediately see the new record.

---

### ✅ Redirect After Success

```tsx
redirect("/dashboard");
```

Avoid duplicate form submissions and provide a clean navigation flow.

---

# 5. Common Pitfalls & Solutions

### ❌ Only client-side validation

```tsx
if (email.includes("@")) {
  // BAD
}
```

Users can bypass JavaScript or craft HTTP requests directly.

Always validate again on the server.

---

### ❌ Trusting `FormData`

Everything from `FormData` is untrusted input.

Always validate and sanitize it.

---

### ❌ Throwing validation errors

Instead of:

```tsx
throw new Error("Invalid email");
```

Return structured validation errors:

```tsx
return {
  errors: {
    email: ["Invalid email"],
  },
};
```

This integrates cleanly with `useActionState`.

---

### ❌ Mixing business logic and validation

Keep validation separate:

```
Schema
↓

Validation
↓

Business Logic
↓

Database
```

This improves maintainability and testability.

---

### ❌ Forgetting cache invalidation

After mutations:

```tsx
revalidatePath("/users");
```

or

```tsx
redirect("/users");
```

Otherwise, users may see stale data.

---

# Code

```tsx
// app/actions.ts

"use server";

import { z } from "zod";
import { revalidatePath } from "next/cache";

const UserSchema = z.object({
  name: z.string().min(3),
  email: z.email(),
});

export async function createUser(formData: FormData) {
  const result = UserSchema.safeParse({
    name: formData.get("name"),
    email: formData.get("email"),
  });

  if (!result.success) {
    return {
      errors: result.error.flatten().fieldErrors,
    };
  }

  // await prisma.user.create({
  //   data: result.data,
  // });

  revalidatePath("/users");

  return {
    success: true,
  };
}
```

---

## When to use

Use server-side validation for:

- User registration and login
- Contact forms
- Checkout and payment forms
- Profile updates
- Admin dashboards
- File upload metadata validation
- Any form that writes to a database or performs sensitive operations

It ensures data integrity and protects against malformed or malicious requests.

---

## Alternatives

| Approach                                | Best For                    | Pros                                                      | Cons                                       |
| --------------------------------------- | --------------------------- | --------------------------------------------------------- | ------------------------------------------ |
| **Server Actions (Recommended)**        | Next.js 16 App Router       | Minimal boilerplate, secure, integrates with forms        | App Router only                            |
| API Routes / Route Handlers             | Public APIs, mobile clients | Reusable outside the app                                  | More code and manual fetch calls           |
| Client-side validation only             | UX enhancements             | Immediate feedback                                        | Not secure on its own                      |
| Server Actions + Zod + `useActionState` | Production applications     | Best developer experience, type safety, structured errors | Requires React/Next.js App Router patterns |

**Interview Tip:** Emphasize that **client-side validation is for user experience, while server-side validation is for security and data integrity**. In Next.js 16, the preferred production pattern is **Server Actions + Zod + `useActionState`**, followed by `revalidatePath()` or `redirect()` after a successful mutation.

## Question 2. How do you implement optimistic UI updates for forms?

## Question 3. How do you integrate Next.js with Redux for global state management?

## Question 4. How do you integrate Next.js with Zustand or Jotai for lightweight state?

## Question 5. How do you implement SWR caching strategies?

## Question 6. How do you integrate Next.js with Axios interceptors?

## Question 7. How do you implement error boundaries in Next.js?

## Question 8. How do you implement breadcrumbs dynamically in SSR pages?

## Question 9. How do you implement dynamic SEO tags per page?

## Question 10. How do you integrate third-party analytics like Google Analytics or Plausible?

## Question 11. How do you implement React Server Components in the new app router?

## Question 12. How do you implement nested layouts using Next.js 13+ app router?

## Question 13. How do you implement streaming SSR for large pages?

## Question 14. How do you implement on-demand ISR revalidation at scale?

## Question 15. How do you implement edge caching using Vercel or Netlify?

## Question 16. How do you implement authentication at the edge with middleware?

## Question 17. How do you implement feature flags in production Next.js apps?

## Question 18. How do you implement multi-tenant architectures with SSR?

## Question 19. How do you implement high-performance real-time dashboards?

## Question 20. How do you integrate Next.js with GraphQL subscriptions?
