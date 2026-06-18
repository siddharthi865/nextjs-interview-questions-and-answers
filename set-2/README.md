# Set 2

| #   | Question                                                                                                                                                      |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [How does Next.js support TypeScript?](#question-1-how-does-nextjs-support-typescript)                                                                        |
| 2   | [How can you add environment variables in Next.js?](#question-2-how-can-you-add-environment-variables-in-nextjs)                                              |
| 3   | [What is the difference between public and server environment variables?](#question-3-what-is-the-difference-between-public-and-server-environment-variables) |
| 4   | [How do you handle images in Next.js?](#question-4-how-do-you-handle-images-in-nextjs)                                                                        |
| 5   | [What is the Next.js Image component?](#question-5-what-is-the-nextjs-image-component)                                                                        |
| 6   | [How does Next.js optimize images?](#question-6-how-does-nextjs-optimize-images)                                                                              |
| 7   | [What is the Link component used for?](#question-7-what-is-the-link-component-used-for)                                                                       |
| 8   | [How is navigation handled in Next.js?](#question-8-how-is-navigation-handled-in-nextjs)                                                                      |
| 9   | [What are Next.js built-in Head components?](#question-9-what-are-nextjs-built-in-head-components)                                                            |
| 10  | [How do you add metadata like title and description in Next.js?](#question-10-how-do-you-add-metadata-like-title-and-description-in-nextjs)                   |
| 11  | [How do you handle CSS Modules in Next.js?](#question-11-how-do-you-handle-css-modules-in-nextjs)                                                             |
| 12  | [What are styled-jsx and styled-components?](#question-12-what-are-styled-jsx-and-styled-components)                                                          |
| 13  | [How do you do client-side navigation with prefetching?](#question-13-how-do-you-do-client-side-navigation-with-prefetching)                                  |
| 14  | [What are the advantages of SSR?](#question-14-what-are-the-advantages-of-ssr)                                                                                |
| 15  | [What are the advantages of SSG?](#question-15-what-are-the-advantages-of-ssg)                                                                                |
| 16  | [How does ISR work in Next.js?](#question-16-how-does-isr-work-in-nextjs)                                                                                     |
| 17  | [Explain how fallback: true works in getStaticPaths](#question-17-explain-how-fallback-true-works-in-getstaticpaths)                                          |
| 18  | [Explain fallback: false and fallback: blocking](#question-18-explain-fallback-false-and-fallback-blocking)                                                   |
| 19  | [How do you handle query parameters in Next.js?](#question-19-how-do-you-handle-query-parameters-in-nextjs)                                                   |
| 20  | [How can you redirect a page in Next.js?](#question-20-how-can-you-redirect-a-page-in-nextjs)                                                                 |

## Question 1. How does Next.js support TypeScript?

**Q: How does Next.js support TypeScript?**

**Short Answer (30 seconds):**
Next.js has first-class, zero-config TypeScript support. You can simply add a `tsconfig.json`, and Next.js automatically configures compilation, type-checking, and development tooling for both App Router and Pages Router.

---

**Detailed Explanation:**

### 1. Core concept with docs reference

Next.js provides **built-in TypeScript support out of the box**, meaning you don’t need to manually configure Babel or Webpack for TypeScript.

📚 Docs: [https://nextjs.org/docs/app/building-your-application/configuring/typescript](https://nextjs.org/docs/app/building-your-application/configuring/typescript)

When you run:

```bash
npx create-next-app@latest
```

Next.js automatically asks:

> “Would you like to use TypeScript?”

If yes, it:

- Creates `tsconfig.json`
- Installs `typescript`, `@types/react`, `@types/node`
- Enables type-aware compilation during dev and build

---

### 2. Implementation approach

Next.js supports TypeScript across:

- **App Router (`app/`)**
- **Pages Router (`pages/`)**
- **API Routes / Route Handlers**
- **Middleware**

Key behavior:

- `.ts` / `.tsx` files are automatically detected
- Type-checking runs during `next dev` and `next build`
- No Babel configuration required

---

### 3. Code example - App Router preferred

#### Example: Type-safe Server Component

```tsx
// app/users/page.tsx

type User = {
  id: string;
  name: string;
  email: string;
};

async function getUsers(): Promise<User[]> {
  const res = await fetch("https://api.example.com/users", {
    cache: "no-store",
  });

  if (!res.ok) throw new Error("Failed to fetch users");

  return res.json();
}

export default async function UsersPage() {
  const users = await getUsers();

  return (
    <div>
      <h1>Users</h1>
      <ul>
        {users.map((user) => (
          <li key={user.id}>
            {user.name} — {user.email}
          </li>
        ))}
      </ul>
    </div>
  );
}
```

#### Example: Typed API Route (Route Handler)

```ts
// app/api/users/route.ts

import { NextResponse } from "next/server";

type User = {
  id: string;
  name: string;
};

export async function GET(): Promise<NextResponse<User[]>> {
  return NextResponse.json([
    { id: "1", name: "John" },
    { id: "2", name: "Jane" },
  ]);
}
```

---

### 4. Performance considerations

- TypeScript does **not affect runtime performance** (it’s erased at build time)
- However, Next.js improves **developer performance** by:
  - Incremental type checking during dev
  - Fast Refresh compatibility with TSX
  - Isolated module compilation for faster builds

- Large codebases may benefit from:

  ```json
  {
    "compilerOptions": {
      "skipLibCheck": true
    }
  }
  ```

  to reduce type-checking overhead

---

### 5. Common pitfalls & solutions

**❌ Missing or misconfigured tsconfig.json**

- Solution: let Next.js generate it or extend recommended config

**❌ Using `any` everywhere**

- Breaks type safety benefits; instead use:
  - Generics
  - Zod / runtime validation for API data

**❌ Mixing client/server types incorrectly**

- Server Components may use Node APIs
- Client Components must avoid server-only types

**❌ API response not typed**

- Leads to runtime mismatches
- Fix using shared types or schema validation

---

**Code: Shared type-safe pattern (best practice)**

```ts
// lib/types/user.ts
export type User = {
  id: string;
  name: string;
  email: string;
};
```

```tsx
// app/users/page.tsx
import type { User } from "@/lib/types/user";
```

---

**When to use:**

- Enterprise-scale React applications
- APIs requiring strict contract safety
- Teams needing maintainable, scalable codebases
- Full-stack Next.js apps using App Router + Server Actions

---

**Alternatives:**

- **JavaScript + JSDoc** (lighter but less strict)
- **TypeScript + Zod** (recommended for runtime validation)
- **Pure TS without Next.js integration** (requires manual setup, not recommended)

## Question 2. How can you add environment variables in Next.js?

## Question 3. What is the difference between public and server environment variables?

## Question 4. How do you handle images in Next.js?

## Question 5. What is the Next.js Image component?

## Question 6. How does Next.js optimize images?

## Question 7. What is the Link component used for?

## Question 8. How is navigation handled in Next.js?

## Question 9. What are Next.js built-in Head components?

## Question 10. How do you add metadata like title and description in Next.js?

## Question 11. How do you handle CSS Modules in Next.js?

## Question 12. What are styled-jsx and styled-components?

## Question 13. How do you do client-side navigation with prefetching?

## Question 14. What are the advantages of SSR?

## Question 15. What are the advantages of SSG?

## Question 16. How does ISR work in Next.js?

## Question 17. Explain how fallback: true works in getStaticPaths

## Question 18. Explain fallback: false and fallback: blocking

## Question 19. How do you handle query parameters in Next.js?

## Question 20. How can you redirect a page in Next.js?
