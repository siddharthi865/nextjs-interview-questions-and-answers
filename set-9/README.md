# Set 9

| #   | Question                                                                                                                                  |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [How do you manage state in a Next.js app with Redux Toolkit?](#question-1-how-do-you-manage-state-in-a-nextjs-app-with-redux-toolkit)    |
| 2   | [How do you implement session management with NextAuth.js?](#question-2-how-do-you-implement-session-management-with-nextauthjs)          |
| 3   | [How do you customize the 404 page globally?](#question-3-how-do-you-customize-the-404-page-globally)                                     |
| 4   | [How do you implement preview mode for draft content?](#question-4-how-do-you-implement-preview-mode-for-draft-content)                   |
| 5   | [How do you handle query caching in SSR pages?](#question-5-how-do-you-handle-query-caching-in-ssr-pages)                                 |
| 6   | [How do you integrate a CMS like Contentful or Strapi?](#question-6-how-do-you-integrate-a-cms-like-contentful-or-strapi)                 |
| 7   | [How do you implement incremental builds for large sites?](#question-7-how-do-you-implement-incremental-builds-for-large-sites)           |
| 8   | [How do you handle dynamic SEO meta tags for pages?](#question-8-how-do-you-handle-dynamic-seo-meta-tags-for-pages)                       |
| 9   | [How do you implement a search feature in Next.js?](#question-9-how-do-you-implement-a-search-feature-in-nextjs)                          |
| 10  | [How do you implement breadcrumbs for dynamic pages?](#question-10-how-do-you-implement-breadcrumbs-for-dynamic-pages)                    |
| 11  | [How do you implement real-time updates in a Next.js app?](#question-11-how-do-you-implement-real-time-updates-in-a-nextjs-app)           |
| 12  | [How can you use WebSockets with Next.js?](#question-12-how-can-you-use-websockets-with-nextjs)                                           |
| 13  | [How do you implement GraphQL server-side fetching in Next.js?](#question-13-how-do-you-implement-graphql-server-side-fetching-in-nextjs) |
| 14  | [How do you implement GraphQL client-side fetching in Next.js?](#question-14-how-do-you-implement-graphql-client-side-fetching-in-nextjs) |
| 15  | [How do you handle caching with GraphQL in Next.js?](#question-15-how-do-you-handle-caching-with-graphql-in-nextjs)                       |
| 16  | [How do you implement server-side caching for API routes?](#question-16-how-do-you-implement-server-side-caching-for-api-routes)          |
| 17  | [How do you implement CDN caching with ISR?](#question-17-how-do-you-implement-cdn-caching-with-isr)                                      |
| 18  | [How do you stream server-rendered pages?](#question-18-how-do-you-stream-server-rendered-pages)                                          |
| 19  | [How do you implement partial hydration in Next.js 13+?](#question-19-how-do-you-implement-partial-hydration-in-nextjs-13)                |
| 20  | [How do you implement React Suspense in SSR?](#question-20-how-do-you-implement-react-suspense-in-ssr)                                    |

## Question 1. How do you manage state in a Next.js app with Redux Toolkit?

**Q: How do you manage state in a Next.js app with Redux Toolkit?**

**Short Answer (30 seconds):**

Redux Toolkit is the recommended way to use Redux because it reduces boilerplate and provides built-in best practices. In **Next.js 16 with the App Router**, Redux should primarily be used for **client-side global state** like authentication UI, theme, shopping carts, filters, and application state, while **server data should remain in Server Components** or be fetched using Next.js data fetching mechanisms instead of storing everything in Redux.

---

# Detailed Explanation

## 1. Core Concept (Next.js 16 + Redux Toolkit)

According to the Next.js App Router architecture:

- **Server Components** should fetch data.
- **Client Components** handle interactive UI.
- **Redux Toolkit** manages client-side global state.

A common mistake is trying to replace Server Components with Redux. Instead:

| Use Redux For | Don't Use Redux For   |
| ------------- | --------------------- |
| Theme         | Initial server data   |
| Shopping cart | Database queries      |
| Auth UI state | SEO content           |
| Sidebar state | Server-rendered pages |
| Notifications | Static CMS content    |
| Filters       | Route data            |

This aligns with the App Router philosophy:

> Server = Data
>
> Client = UI State

**Docs:**

- Next.js App Router → [https://nextjs.org/docs/app](https://nextjs.org/docs/app)
- Redux Toolkit → [https://redux-toolkit.js.org/tutorials/quick-start](https://redux-toolkit.js.org/tutorials/quick-start)

---

# 2. Folder Structure

A production structure typically looks like:

```text
app/
   layout.tsx
   providers.tsx

lib/
   store.ts
   hooks.ts

features/
   counter/
      counterSlice.ts
```

Keep Redux logic outside the `app` directory.

---

# 3. Create the Store

```tsx
// lib/store.ts

import { configureStore } from "@reduxjs/toolkit";
import counterReducer from "@/features/counter/counterSlice";

export const store = configureStore({
  reducer: {
    counter: counterReducer,
  },
});

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

`configureStore()` automatically:

- Adds Redux DevTools
- Enables Immer
- Adds middleware
- Enables serializable checks

This is why Redux Toolkit is preferred over classic Redux.

---

# 4. Create a Slice

```tsx
// features/counter/counterSlice.ts

import { createSlice } from "@reduxjs/toolkit";

const initialState = {
  value: 0,
};

const counterSlice = createSlice({
  name: "counter",
  initialState,
  reducers: {
    increment(state) {
      state.value++;
    },
    decrement(state) {
      state.value--;
    },
  },
});

export const { increment, decrement } = counterSlice.actions;

export default counterSlice.reducer;
```

Redux Toolkit uses **Immer**, so writing:

```ts
state.value++;
```

is safe because it produces immutable updates under the hood.

---

# 5. Create Typed Hooks

```tsx
// lib/hooks.ts

import { TypedUseSelectorHook, useDispatch, useSelector } from "react-redux";

import type { RootState, AppDispatch } from "./store";

export const useAppDispatch = useDispatch.withTypes<AppDispatch>();

export const useAppSelector = useSelector.withTypes<RootState>();
```

This avoids repetitive TypeScript annotations throughout the app.

---

# 6. Provide the Store

Because Redux uses React Context, the provider must be a **Client Component**.

```tsx
// app/providers.tsx

"use client";

import { Provider } from "react-redux";
import { store } from "@/lib/store";

export default function Providers({ children }: { children: React.ReactNode }) {
  return <Provider store={store}>{children}</Provider>;
}
```

Wrap the root layout:

```tsx
// app/layout.tsx

import Providers from "./providers";

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html>
      <body>
        <Providers>{children}</Providers>
      </body>
    </html>
  );
}
```

---

# 7. Use Redux in Client Components

```tsx
"use client";

import { increment, decrement } from "@/features/counter/counterSlice";

import { useAppDispatch, useAppSelector } from "@/lib/hooks";

export default function Counter() {
  const count = useAppSelector((state) => state.counter.value);

  const dispatch = useAppDispatch();

  return (
    <>
      <h2>{count}</h2>

      <button onClick={() => dispatch(increment())}>+</button>

      <button onClick={() => dispatch(decrement())}>-</button>
    </>
  );
}
```

Notice that Redux hooks can only be used in **Client Components**.

---

# 8. Async Operations

Redux Toolkit provides `createAsyncThunk`, but in Next.js 16 it's often better to:

- Fetch data in a **Server Component**.
- Use **Server Actions** for mutations.
- Use Redux only if the fetched data needs to be shared and updated interactively across many client components.

For example:

```tsx
const products = await getProducts();
```

instead of:

```tsx
dispatch(fetchProducts());
```

This reduces client-side JavaScript and leverages React Server Components.

---

# 9. Performance Considerations

### ✅ Keep server data on the server

Avoid duplicating fetched data into Redux unless necessary.

---

### ✅ Split slices

Instead of one large store:

```text
authSlice
cartSlice
themeSlice
notificationSlice
```

This improves maintainability and scalability.

---

### ✅ Memoized selectors

Use `createSelector` for derived state to prevent unnecessary recalculations.

```ts
const totalPrice = createSelector([(state) => state.cart.items], (items) =>
  items.reduce((sum, item) => sum + item.price, 0),
);
```

---

### ✅ Lazy load heavy features

For large applications, dynamically import feature components so Redux-related code is only loaded when needed.

---

### ✅ Minimize client boundaries

Only wrap parts of the application that require client-side interactivity, keeping the rest as Server Components for better performance.

---

# 10. Common Pitfalls & Solutions

### ❌ Using Redux for everything

Don't move all server-fetched data into Redux. Prefer Server Components and pass data as props where possible.

---

### ❌ Using Redux in Server Components

Redux hooks like `useSelector` and `useDispatch` only work in Client Components marked with `"use client"`.

---

### ❌ Mutating state manually

Without Redux Toolkit, manual immutable updates are error-prone. Immer lets you write simpler update logic safely.

---

### ❌ One giant slice

Separate state by domain (auth, cart, settings, etc.) instead of creating a monolithic slice.

---

### ❌ Ignoring TypeScript typing

Always export `RootState` and `AppDispatch` and use typed hooks to ensure type safety across the app.

---

# Code (Production-Ready Example)

```tsx
// lib/store.ts
import { configureStore } from "@reduxjs/toolkit";
import cartReducer from "@/features/cart/cartSlice";

export const store = configureStore({
  reducer: {
    cart: cartReducer,
  },
});

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

```tsx
// app/providers.tsx
"use client";

import { Provider } from "react-redux";
import { store } from "@/lib/store";

export default function Providers({ children }: { children: React.ReactNode }) {
  return <Provider store={store}>{children}</Provider>;
}
```

```tsx
// app/layout.tsx
import Providers from "./providers";

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html>
      <body>
        <Providers>{children}</Providers>
      </body>
    </html>
  );
}
```

---

## When to Use

Use Redux Toolkit when:

- Multiple client components need shared state.
- Managing shopping carts, authentication UI, themes, notifications, or complex workflows.
- State transitions are complex and benefit from centralized logic.
- You need predictable updates, DevTools, and middleware.

Avoid Redux for:

- Server-fetched data that can stay in Server Components.
- Static content or route data.
- Local component state (prefer `useState` or `useReducer`).

---

## Alternatives

| Solution                  | Best Use Case                                                            |
| ------------------------- | ------------------------------------------------------------------------ |
| **Redux Toolkit**         | Large applications with complex global client state                      |
| **React Context**         | Theme, authentication context, small global state                        |
| **useState / useReducer** | Local component state                                                    |
| **Server Components**     | Data fetching and rendering on the server                                |
| **Server Actions**        | Form submissions and server-side mutations                               |
| **TanStack Query / SWR**  | Client-side server state with caching, revalidation, and synchronization |

**Interview Tip:** A strong Next.js 16 answer highlights that **Redux Toolkit is for client-side global UI state—not as a replacement for React Server Components.** The best practice is to fetch data in Server Components, use Server Actions for mutations, and reserve Redux for interactive state that must be shared across multiple Client Components.

## Question 2. How do you implement session management with NextAuth.js?

## Question 3. How do you customize the 404 page globally?

## Question 4. How do you implement preview mode for draft content?

## Question 5. How do you handle query caching in SSR pages?

## Question 6. How do you integrate a CMS like Contentful or Strapi?

## Question 7. How do you implement incremental builds for large sites?

## Question 8. How do you handle dynamic SEO meta tags for pages?

## Question 9. How do you implement a search feature in Next.js?

## Question 10. How do you implement breadcrumbs for dynamic pages?

## Question 11. How do you implement real-time updates in a Next.js app?

## Question 12. How can you use WebSockets with Next.js?

## Question 13. How do you implement GraphQL server-side fetching in Next.js?

## Question 14. How do you implement GraphQL client-side fetching in Next.js?

## Question 15. How do you handle caching with GraphQL in Next.js?

## Question 16. How do you implement server-side caching for API routes?

## Question 17. How do you implement CDN caching with ISR?

## Question 18. How do you stream server-rendered pages?

## Question 19. How do you implement partial hydration in Next.js 13+?

## Question 20. How do you implement React Suspense in SSR?
