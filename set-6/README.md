# Set 6

| #   | Question                                                                                                                                                              |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [What versions of React does Next.js support?](#question-1-what-versions-of-react-does-nextjs-support)                                                                |
| 2   | [How do you initialize a Next.js project?](#question-2-how-do-you-initialize-a-nextjs-project)                                                                        |
| 3   | [What is the difference between npm create next-app and npx create-next-app?](#question-3-what-is-the-difference-between-npm-create-next-app-and-npx-create-next-app) |
| 4   | [What is Fast Refresh in Next.js?](#question-4-what-is-fast-refresh-in-nextjs)                                                                                        |
| 5   | [What are the benefits of using Next.js over Create React App?](#question-5-what-are-the-benefits-of-using-nextjs-over-create-react-app)                              |
| 6   | [How does Next.js handle JavaScript bundling?](#question-6-how-does-nextjs-handle-javascript-bundling)                                                                |
| 7   | [What is prefetching in Next.js Link?](#question-7-what-is-prefetching-in-nextjs-link)                                                                                |
| 8   | [How do you handle static assets like fonts or images?](#question-8-how-do-you-handle-static-assets-like-fonts-or-images)                                             |
| 9   | [What is the difference between .js and .ts files in Next.js?](#question-9-what-is-the-difference-between-js-and-ts-files-in-nextjs)                                  |
| 10  | [How do you create a new page in Next.js?](#question-10-how-do-you-create-a-new-page-in-nextjs)                                                                       |
| 11  | [How do you navigate programmatically in Next.js?](#question-11-how-do-you-navigate-programmatically-in-nextjs)                                                       |
| 12  | [How do you pass props to a page component?](#question-12-how-do-you-pass-props-to-a-page-component)                                                                  |
| 13  | [What is the difference between functional and class components in Next.js?](#question-13-what-is-the-difference-between-functional-and-class-components-in-nextjs)   |
| 14  | [What is the default export requirement for Next.js pages?](#question-14-what-is-the-default-export-requirement-for-nextjs-pages)                                     |
| 15  | [How do you handle 500 internal server errors?](#question-15-how-do-you-handle-500-internal-server-errors)                                                            |
| 16  | [What are the supported file extensions for pages?](#question-16-what-are-the-supported-file-extensions-for-pages)                                                    |
| 17  | [How do you make a page SEO-friendly in Next.js?](#question-17-how-do-you-make-a-page-seo-friendly-in-nextjs)                                                         |
| 18  | [How do you handle multiple layouts for pages?](#question-18-how-do-you-handle-multiple-layouts-for-pages)                                                            |
| 19  | [How do you set up a favicon in Next.js?](#question-19-how-do-you-set-up-a-favicon-in-nextjs)                                                                         |
| 20  | [How can you link external CSS libraries in Next.js?](#question-20-how-can-you-link-external-css-libraries-in-nextjs)                                                 |

## Question 1. What versions of React does Next.js support?

**Q: What versions of React does Next.js support?**

**Short Answer (30 seconds):**

Next.js closely follows the latest stable React releases. **Next.js 16 supports React 19 as the primary supported version**, including modern React features like **Server Components, Server Actions, Suspense, streaming, and the latest hooks**. In production, I always recommend using the React version specified by the Next.js release to ensure full compatibility and access to all framework features.

---

# Detailed Explanation

## 1. React Version Support

Next.js is built on top of React and is maintained by Vercel in close collaboration with the React team. Because of this, new Next.js releases are designed around specific React versions.

For **Next.js 16**:

- ✅ React 19 is the supported version
- ✅ React DOM 19
- ✅ React Server Components (RSC)
- ✅ Suspense
- ✅ Streaming
- ✅ Concurrent Rendering
- ✅ Server Actions
- ✅ Latest React Hooks and APIs

Official documentation:

- Next.js 16 → [https://nextjs.org/docs](https://nextjs.org/docs)
- React → [https://react.dev](https://react.dev)

---

## 2. Why Matching Versions Matters

Using the React version bundled with the corresponding Next.js release ensures:

- Full framework compatibility
- Access to latest performance improvements
- Stable Server Component behavior
- Better compiler optimizations
- No peer dependency conflicts
- Proper support for App Router features

For example:

```
Next.js 16
        ↓
React 19
        ↓
App Router
        ↓
Server Components
        ↓
Streaming
        ↓
Server Actions
```

Trying to force an older React version can lead to:

- Missing APIs
- Hydration mismatches
- Unsupported features
- Build warnings
- Dependency conflicts

---

## 3. package.json Example

```json
{
  "dependencies": {
    "next": "16.x",
    "react": "19.x",
    "react-dom": "19.x"
  }
}
```

When creating a new application:

```bash
npx create-next-app@latest
```

Next.js automatically installs the correct React version.

---

## 4. React Features Available in Next.js 16

Some important React capabilities supported in Next.js 16 include:

### Server Components

```tsx
export default async function Page() {
  const users = await getUsers();

  return (
    <div>
      {users.map((user) => (
        <p key={user.id}>{user.name}</p>
      ))}
    </div>
  );
}
```

Benefits:

- Zero client-side JavaScript for server-rendered UI
- Smaller bundles
- Faster page loads

---

### Suspense

```tsx
import { Suspense } from "react";

export default function Dashboard() {
  return (
    <Suspense fallback={<p>Loading...</p>}>
      <Analytics />
    </Suspense>
  );
}
```

Enables streaming UI while slower components continue rendering.

---

### Server Actions

```tsx
"use server";

export async function createPost(formData: FormData) {
  // Save data
}
```

No separate API route is needed for many form submissions and mutations.

---

### Concurrent Rendering

React automatically prioritizes urgent updates, keeping applications responsive during expensive rendering.

---

## 5. Performance Considerations

Using the supported React version provides:

- Improved hydration
- Smaller JavaScript bundles
- Faster rendering
- Better streaming performance
- Optimized Server Components
- Improved caching behavior
- Better compatibility with the React Compiler ecosystem as it evolves

These improvements are especially important for large production applications.

---

## 6. Common Pitfalls & Solutions

### ❌ Mixing unsupported React versions

Using an older React release with a newer Next.js version can cause dependency conflicts and unsupported behavior.

**Solution:** Keep `next`, `react`, and `react-dom` on compatible versions.

---

### ❌ Ignoring upgrade guides

Major Next.js releases may introduce changes that require updates to application code.

**Solution:** Follow the official upgrade guide before moving between major versions.

---

### ❌ Assuming all React APIs work the same everywhere

Server Components and Client Components have different capabilities. Hooks like `useState` and `useEffect` only work in Client Components.

```tsx
"use client";

import { useState } from "react";

export default function Counter() {
  const [count, setCount] = useState(0);

  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}
```

---

## Code

### `package.json`

```json
{
  "dependencies": {
    "next": "16.0.0",
    "react": "19.x",
    "react-dom": "19.x"
  }
}
```

### Server Component (default)

```tsx
// app/page.tsx

export default async function Page() {
  const data = await fetch("https://api.example.com/posts", {
    cache: "force-cache",
  }).then((res) => res.json());

  return (
    <main>
      <h1>Posts</h1>
      {data.map((post: { id: number; title: string }) => (
        <p key={post.id}>{post.title}</p>
      ))}
    </main>
  );
}
```

---

## When to use

- Building applications with **Next.js 16 and the App Router**
- Leveraging **React 19** features such as Server Components, Suspense, and Server Actions
- Developing production applications that benefit from improved performance, streaming, and modern React capabilities

---

## Alternatives

| Option                              | When to use                                                                                                    |
| ----------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| **Server Components (Recommended)** | Data fetching, rendering UI on the server, improved performance, SEO                                           |
| **Client Components**               | Interactive UI requiring state, effects, or browser APIs                                                       |
| **Pages Router**                    | Legacy applications that haven't migrated to the App Router                                                    |
| **App Router (Recommended)**        | New applications using React 19 features like Server Components, nested layouts, streaming, and Server Actions |

> **Interview Tip:** A strong answer is: _"Next.js 16 supports React 19 and is designed to work with the React version bundled for that release. This enables modern features like React Server Components, Suspense, streaming, concurrent rendering, and Server Actions. In production, I always keep `next`, `react`, and `react-dom` on compatible versions recommended by the Next.js release to avoid peer dependency issues and ensure access to the latest optimizations."_

## Question 2. How do you initialize a Next.js project?

## Question 3. What is the difference between npm create next-app and npx create-next-app?

## Question 4. What is Fast Refresh in Next.js?

## Question 5. What are the benefits of using Next.js over Create React App?

## Question 6. How does Next.js handle JavaScript bundling?

## Question 7. What is prefetching in Next.js Link?

## Question 8. How do you handle static assets like fonts or images?

## Question 9. What is the difference between .js and .ts files in Next.js?

## Question 10. How do you create a new page in Next.js?

## Question 11. How do you navigate programmatically in Next.js?

## Question 12. How do you pass props to a page component?

## Question 13. What is the difference between functional and class components in Next.js?

## Question 14. What is the default export requirement for Next.js pages?

## Question 15. How do you handle 500 internal server errors?

## Question 16. What are the supported file extensions for pages?

## Question 17. How do you make a page SEO-friendly in Next.js?

## Question 18. How do you handle multiple layouts for pages?

## Question 19. How do you set up a favicon in Next.js?

## Question 20. How can you link external CSS libraries in Next.js?
