# Set 13

| #   | Question                                                                                                                                     |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [How do you implement conditional SSR vs CSR for a page?](#question-1-how-do-you-implement-conditional-ssr-vs-csr-for-a-page)                |
| 2   | [How do you handle authentication cookies securely?](#question-2-how-do-you-handle-authentication-cookies-securely)                          |
| 3   | [How do you handle JWT tokens in SSR pages?](#question-3-how-do-you-handle-jwt-tokens-in-ssr-pages)                                          |
| 4   | [How do you implement a custom `_error.js` page?](#question-4-how-do-you-implement-a-custom-_errorjs-page)                                   |
| 5   | [How do you implement breadcrumbs dynamically?](#question-5-how-do-you-implement-breadcrumbs-dynamically)                                    |
| 6   | [How do you implement lazy loading for images and components?](#question-6-how-do-you-implement-lazy-loading-for-images-and-components)      |
| 7   | [How do you implement dynamic imports with SSR disabled?](#question-7-how-do-you-implement-dynamic-imports-with-ssr-disabled)                |
| 8   | [How do you handle API errors and show fallback UI?](#question-8-how-do-you-handle-api-errors-and-show-fallback-ui)                          |
| 9   | [How do you implement client-side pagination?](#question-9-how-do-you-implement-client-side-pagination)                                      |
| 10  | [How do you implement server-side pagination?](#question-10-how-do-you-implement-server-side-pagination)                                     |
| 11  | [How do you integrate a headless CMS like Sanity with Next.js?](#question-11-how-do-you-integrate-a-headless-cms-like-sanity-with-nextjs)    |
| 12  | [How do you implement preview mode for CMS content?](#question-12-how-do-you-implement-preview-mode-for-cms-content)                         |
| 13  | [How do you implement content filtering on SSR pages?](#question-13-how-do-you-implement-content-filtering-on-ssr-pages)                     |
| 14  | [How do you implement theme switching (dark/light) in Next.js?](#question-14-how-do-you-implement-theme-switching-darklight-in-nextjs)       |
| 15  | [How do you integrate Next.js with Google Analytics?](#question-15-how-do-you-integrate-nextjs-with-google-analytics)                        |
| 16  | [How do you implement global state management with Context API?](#question-16-how-do-you-implement-global-state-management-with-context-api) |
| 17  | [How do you implement global state with Redux Toolkit?](#question-17-how-do-you-implement-global-state-with-redux-toolkit)                   |
| 18  | [How do you implement optimistic UI updates with SWR?](#question-18-how-do-you-implement-optimistic-ui-updates-with-swr)                     |
| 19  | [How do you prefetch multiple dynamic routes programmatically?](#question-19-how-do-you-prefetch-multiple-dynamic-routes-programmatically)   |
| 20  | [How do you implement a real-time chat feature?](#question-20-how-do-you-implement-a-real-time-chat-feature)                                 |

## Question 1. How do you implement conditional SSR vs CSR for a page?

## **Q: How do you implement conditional SSR vs CSR for a page?**

### **Short Answer (30 seconds):**

In **Next.js 16**, you don't typically switch an entire page between SSR and CSR at runtime. Instead, you **compose your page using Server Components and Client Components**. Render data that should be fetched on the server using **Server Components**, and move interactive or browser-dependent functionality into **Client Components**. If a component should only render on the client, you can dynamically import it with `ssr: false`.

---

# **Detailed Explanation**

## **1. Core Concept (Next.js 16 Approach)**

With the **App Router**, every component is a **Server Component by default**.

This means:

- Fetching data → **Server Component (SSR)**
- Browser APIs (`window`, `localStorage`) → **Client Component**
- User interaction → **Client Component**
- SEO content → **Server Component**

Unlike the Pages Router where developers often chose between `getServerSideProps()` and client-side fetching, App Router encourages **mixing SSR and CSR within the same page**.

**Official Docs:**

- App Router → Server Components
- Client Components (`"use client"`)
- Dynamic Imports
- Rendering Strategies

---

# **2. Typical Conditional Rendering Scenarios**

## Scenario 1: SEO Content → SSR, Dashboard Widget → CSR

```
Product Page

├── Product Details (SSR)
├── Reviews (SSR)
├── Recommendation Widget (CSR)
├── Recently Viewed (CSR)
└── Chat Widget (CSR)
```

This gives:

- Fast initial load
- SEO
- Interactive widgets

---

## Scenario 2: Browser APIs

If your component needs:

- localStorage
- window
- document
- navigator
- geolocation

Use a Client Component.

```tsx
"use client";

import { useEffect, useState } from "react";

export default function Theme() {
  const [theme, setTheme] = useState("");

  useEffect(() => {
    setTheme(localStorage.getItem("theme") || "light");
  }, []);

  return <p>{theme}</p>;
}
```

---

## **3. Dynamic Import (Disable SSR)**

Sometimes a component should **never** render on the server.

Example:

- Maps
- Charts
- Rich Text Editors
- Video Players

```tsx
import dynamic from "next/dynamic";

const Chart = dynamic(() => import("./Chart"), {
  ssr: false,
});

export default function Dashboard() {
  return <Chart />;
}
```

Here:

- No server rendering
- Component loads only in browser
- Prevents hydration mismatches

---

# **4. Example: Mixing SSR and CSR**

### Server Component

```tsx
// app/products/page.tsx

import ProductList from "./ProductList";
import UserPreferences from "./UserPreferences";

export default async function ProductsPage() {
  const products = await fetch("https://api.example.com/products", {
    cache: "no-store",
  }).then((r) => r.json());

  return (
    <>
      <ProductList products={products} />
      <UserPreferences />
    </>
  );
}
```

---

### Client Component

```tsx
"use client";

import { useEffect, useState } from "react";

export default function UserPreferences() {
  const [currency, setCurrency] = useState("USD");

  useEffect(() => {
    const saved = localStorage.getItem("currency");

    if (saved) {
      setCurrency(saved);
    }
  }, []);

  return <p>Preferred Currency: {currency}</p>;
}
```

The page itself is server-rendered, while the preferences component hydrates in the browser.

---

# **5. Conditional Rendering Based on Authentication**

Server-side authentication:

```tsx
import { redirect } from "next/navigation";

export default async function Dashboard() {
  const session = await getSession();

  if (!session) {
    redirect("/login");
  }

  return <DashboardContent />;
}
```

Personalized data stays on the server, avoiding unnecessary client-side loading.

---

# **6. Client-Side Conditional Rendering**

Sometimes the decision can only be made in the browser.

Example:

```tsx
"use client";

import { useEffect, useState } from "react";

export default function Banner() {
  const [mobile, setMobile] = useState(false);

  useEffect(() => {
    setMobile(window.innerWidth < 768);
  }, []);

  return mobile ? <MobileBanner /> : <DesktopBanner />;
}
```

---

# **7. Performance Considerations**

### ✅ Prefer Server Components

Benefits:

- Zero JavaScript shipped for server-only code
- Better SEO
- Faster initial page load
- Secure access to secrets and backend resources

---

### ✅ Use Client Components Only When Necessary

Examples:

- Forms
- Buttons
- State management
- Animations
- Browser APIs

Keeping the client bundle small improves performance.

---

### ✅ Use Dynamic Imports for Heavy Libraries

Large dependencies like:

- Monaco Editor
- Leaflet
- Chart.js
- Three.js

should be loaded only when needed:

```tsx
const Editor = dynamic(() => import("./Editor"), {
  ssr: false,
});
```

This reduces the initial JavaScript payload.

---

### ✅ Stream Server Components

For slow data:

```tsx
<Suspense fallback={<Loading />}>
  <SlowAnalytics />
</Suspense>
```

Streaming improves perceived performance by sending ready content without waiting for all data to load.

---

# **8. Common Pitfalls & Solutions**

### ❌ Accessing `window` in Server Components

```tsx
window.location.href;
```

**Problem:** `window is not defined`.

**Solution:** Move the logic into a Client Component or a `useEffect`.

---

### ❌ Making an Entire Page a Client Component

```tsx
"use client";
```

at the top of the page unnecessarily sends all its code to the browser.

**Better:** Keep the page as a Server Component and isolate only the interactive parts into Client Components.

---

### ❌ Using `ssr: false` Everywhere

Disabling SSR for large portions of a page hurts SEO and delays content rendering. Reserve it for components that genuinely require browser-only APIs or cannot be rendered on the server.

---

## **Code**

### Server-rendered page with a client-only widget

```tsx
// app/dashboard/page.tsx
import dynamic from "next/dynamic";

const AnalyticsChart = dynamic(() => import("./AnalyticsChart"), {
  ssr: false,
});

export default async function DashboardPage() {
  const stats = await fetch("https://api.example.com/stats", {
    cache: "no-store",
  }).then((res) => res.json());

  return (
    <main>
      <h1>Dashboard</h1>

      <section>
        <h2>Server-rendered Stats</h2>
        <pre>{JSON.stringify(stats, null, 2)}</pre>
      </section>

      <section>
        <h2>Interactive Chart</h2>
        <AnalyticsChart />
      </section>
    </main>
  );
}
```

---

## **When to Use**

Use this composition approach when:

- You need **SEO-friendly** pages with interactive widgets.
- Sensitive or personalized data should be fetched securely on the server.
- Parts of the UI rely on browser APIs or client-side state.
- Heavy third-party libraries should only load in the browser.

---

## **Alternatives**

| Approach                               | Best For                                      | Pros                                       | Cons                                       |
| -------------------------------------- | --------------------------------------------- | ------------------------------------------ | ------------------------------------------ |
| **Server Components (Default)**        | SEO, data fetching, secure backend access     | Minimal client JS, fast initial render     | No browser APIs or React client hooks      |
| **Client Components (`"use client"`)** | Interactivity, state, browser APIs            | Full React client capabilities             | Larger JavaScript bundle                   |
| **Dynamic Import (`ssr: false`)**      | Maps, editors, charts, browser-only libraries | Avoids SSR issues and reduces initial work | No server-rendered HTML for that component |
| **Suspense + Streaming**               | Slow server data                              | Better perceived performance               | Requires thoughtful loading UI             |

> **Interview tip:** In Next.js 16, the preferred pattern is **not to choose between SSR and CSR for an entire page**. Instead, build pages primarily with **Server Components** and selectively introduce **Client Components** where interactivity or browser-only features are required. This component-level composition provides the benefits of both SSR and CSR while keeping bundles small and performance high.

## Question 2. How do you handle authentication cookies securely?

## Question 3. How do you handle JWT tokens in SSR pages?

## Question 4. How do you implement a custom `_error.js` page?

## Question 5. How do you implement breadcrumbs dynamically?

## Question 6. How do you implement lazy loading for images and components?

## Question 7. How do you implement dynamic imports with SSR disabled?

## Question 8. How do you handle API errors and show fallback UI?

## Question 9. How do you implement client-side pagination?

## Question 10. How do you implement server-side pagination?

## Question 11. How do you integrate a headless CMS like Sanity with Next.js?

## Question 12. How do you implement preview mode for CMS content?

## Question 13. How do you implement content filtering on SSR pages?

## Question 14. How do you implement theme switching (dark/light) in Next.js?

## Question 15. How do you integrate Next.js with Google Analytics?

## Question 16. How do you implement global state management with Context API?

## Question 17. How do you implement global state with Redux Toolkit?

## Question 18. How do you implement optimistic UI updates with SWR?

## Question 19. How do you prefetch multiple dynamic routes programmatically?

## Question 20. How do you implement a real-time chat feature?
