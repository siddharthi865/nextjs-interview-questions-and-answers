# Set 17

| #   | Question                                                                                                                                                             |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [How do you implement a simple homepage with multiple sections?](#question-1-how-do-you-implement-a-simple-homepage-with-multiple-sections)                          |
| 2   | [How do you handle optional dynamic route parameters?](#question-2-how-do-you-handle-optional-dynamic-route-parameters)                                              |
| 3   | [How do you add a favicon dynamically based on the page?](#question-3-how-do-you-add-a-favicon-dynamically-based-on-the-page)                                        |
| 4   | [How do you add global CSS alongside CSS Modules?](#question-4-how-do-you-add-global-css-alongside-css-modules)                                                      |
| 5   | [How do you use React.Fragment in Next.js pages?](#question-5-how-do-you-use-reactfragment-in-nextjs-pages)                                                          |
| 6   | [How do you implement a simple navigation menu with active state?](#question-6-how-do-you-implement-a-simple-navigation-menu-with-active-state)                      |
| 7   | [How do you handle image alt text for SEO?](#question-7-how-do-you-handle-image-alt-text-for-seo)                                                                    |
| 8   | [How do you display JSON data fetched from an API statically?](#question-8-how-do-you-display-json-data-fetched-from-an-api-statically)                              |
| 9   | [How do you handle multiple layouts using `_app.js`?](#question-9-how-do-you-handle-multiple-layouts-using-_appjs)                                                   |
| 10  | [How do you implement a simple modal component?](#question-10-how-do-you-implement-a-simple-modal-component)                                                         |
| 11  | [How do you implement server-side redirection based on user authentication?](#question-11-how-do-you-implement-server-side-redirection-based-on-user-authentication) |
| 12  | [How do you implement client-side redirection based on a condition?](#question-12-how-do-you-implement-client-side-redirection-based-on-a-condition)                 |
| 13  | [How do you implement protected routes in Next.js?](#question-13-how-do-you-implement-protected-routes-in-nextjs)                                                    |
| 14  | [How do you handle server-side errors and display a fallback page?](#question-14-how-do-you-handle-server-side-errors-and-display-a-fallback-page)                   |
| 15  | [How do you fetch data from an external API using getStaticProps?](#question-15-how-do-you-fetch-data-from-an-external-api-using-getstaticprops)                     |
| 16  | [How do you fetch data from an external API using getServerSideProps?](#question-16-how-do-you-fetch-data-from-an-external-api-using-getserversideprops)             |
| 17  | [How do you implement getStaticPaths for dynamic routes?](#question-17-how-do-you-implement-getstaticpaths-for-dynamic-routes)                                       |
| 18  | [How do you implement fallback pages for ISR?](#question-18-how-do-you-implement-fallback-pages-for-isr)                                                             |
| 19  | [How do you handle 404 dynamically for missing content?](#question-19-how-do-you-handle-404-dynamically-for-missing-content)                                         |
| 20  | [How do you implement a search page with query parameters?](#question-20-how-do-you-implement-a-search-page-with-query-parameters)                                   |

## Question 1. How do you implement a simple homepage with multiple sections?

**Q: How do you implement a simple homepage with multiple sections?**

**Short Answer (30 seconds):**
In **Next.js 16**, the recommended approach is to build the homepage using the **App Router** with **Server Components by default**. Each section (Hero, Features, Testimonials, FAQ, Footer, etc.) should be its own reusable component, making the page modular, maintainable, and easy to scale.

---

# Detailed Explanation

## 1. Core Concept (App Router)

In Next.js 16, the homepage is typically created in:

```
app/page.tsx
```

Instead of writing one huge file, split the UI into reusable components.

Example project structure:

```
app/
│
├── page.tsx
│
components/
│
├── Hero.tsx
├── Features.tsx
├── Testimonials.tsx
├── Pricing.tsx
├── FAQ.tsx
└── Footer.tsx
```

This keeps the codebase clean and follows component-based architecture.

**Docs:**

- App Router → [https://nextjs.org/docs/app/building-your-application/routing](https://nextjs.org/docs/app/building-your-application/routing)
- Server Components → [https://nextjs.org/docs/app/building-your-application/rendering/server-components](https://nextjs.org/docs/app/building-your-application/rendering/server-components)

---

## 2. Implementation Approach

The homepage simply imports and renders each section.

```
Home Page
│
├── Hero
├── Features
├── Testimonials
├── Pricing
├── FAQ
└── Footer
```

Each component is responsible for one section only.

Benefits:

- Easier maintenance
- Better reusability
- Independent testing
- Cleaner Git history
- Easier collaboration among developers

---

## 3. Code Example

### app/page.tsx

```tsx
import Hero from "@/components/Hero";
import Features from "@/components/Features";
import Testimonials from "@/components/Testimonials";
import Pricing from "@/components/Pricing";
import FAQ from "@/components/FAQ";
import Footer from "@/components/Footer";

export default function HomePage() {
  return (
    <main>
      <Hero />
      <Features />
      <Testimonials />
      <Pricing />
      <FAQ />
      <Footer />
    </main>
  );
}
```

---

### components/Hero.tsx

```tsx
export default function Hero() {
  return (
    <section className="py-24 text-center">
      <h1 className="text-5xl font-bold">Welcome to Next.js 16</h1>

      <p className="mt-4 text-gray-600">
        Build fast, scalable web applications.
      </p>
    </section>
  );
}
```

---

### components/Features.tsx

```tsx
const features = [
  "Server Components",
  "Streaming",
  "App Router",
  "Server Actions",
];

export default function Features() {
  return (
    <section className="py-20">
      <h2 className="text-3xl font-bold mb-8">Features</h2>

      <ul className="grid gap-4">
        {features.map((feature) => (
          <li key={feature} className="rounded border p-4">
            {feature}
          </li>
        ))}
      </ul>
    </section>
  );
}
```

---

### components/Footer.tsx

```tsx
export default function Footer() {
  return (
    <footer className="py-10 text-center border-t">© 2026 My Company</footer>
  );
}
```

---

## 4. Performance Considerations

Since these components are **Server Components by default**, they offer several performance advantages:

- No unnecessary JavaScript sent to the browser.
- Smaller client bundle size.
- Faster initial page load.
- Better SEO because HTML is rendered on the server.
- Ability to fetch data directly in each section without client-side waterfalls.

If a section requires interactivity (e.g., a carousel or contact form), isolate that functionality into a **Client Component** by adding `"use client"` only where needed. This keeps most of the page optimized while enabling rich interactions.

---

## 5. Common Pitfalls & Solutions

**Pitfall 1:** Putting the entire homepage in one file.
**Solution:** Split into reusable section components.

**Pitfall 2:** Making every component a Client Component.
**Solution:** Keep components as Server Components unless they need browser APIs, state, or event handlers.

**Pitfall 3:** Repeating layout code across pages.
**Solution:** Place shared UI like headers and footers in `app/layout.tsx` if they are used site-wide.

**Pitfall 4:** Fetching all data in the parent page.
**Solution:** Let each section fetch only the data it needs, improving modularity and leveraging Next.js caching.

---

## When to use

This approach is ideal for:

- Marketing websites
- SaaS landing pages
- Company websites
- Portfolio sites
- Product homepages
- Documentation landing pages

---

## Alternatives

| Approach                                       | Best For                                                                                |
| ---------------------------------------------- | --------------------------------------------------------------------------------------- |
| **Server Components (Recommended)**            | Static content, SEO, performance, and data fetching                                     |
| **Client Components**                          | Interactive sections (forms, sliders, modals, tabs)                                     |
| **Dynamic Imports (`next/dynamic`)**           | Heavy or below-the-fold sections to reduce initial bundle size                          |
| **Streaming with `loading.tsx` or `Suspense`** | Pages where some sections depend on slower data sources and should render progressively |

**Interview Tip:** Mention that in **Next.js 16**, you should compose the homepage from **small, reusable Server Components** and only opt into Client Components for interactive parts. This aligns with the App Router architecture, improves maintainability, reduces JavaScript sent to the client, and delivers better SEO and performance.

## Question 2. How do you handle optional dynamic route parameters?

## Question 3. How do you add a favicon dynamically based on the page?

## Question 4. How do you add global CSS alongside CSS Modules?

## Question 5. How do you use React.Fragment in Next.js pages?

## Question 6. How do you implement a simple navigation menu with active state?

## Question 7. How do you handle image alt text for SEO?

## Question 8. How do you display JSON data fetched from an API statically?

## Question 9. How do you handle multiple layouts using `_app.js`?

## Question 10. How do you implement a simple modal component?

## Question 11. How do you implement server-side redirection based on user authentication?

## Question 12. How do you implement client-side redirection based on a condition?

## Question 13. How do you implement protected routes in Next.js?

## Question 14. How do you handle server-side errors and display a fallback page?

## Question 15. How do you fetch data from an external API using getStaticProps?

## Question 16. How do you fetch data from an external API using getServerSideProps?

## Question 17. How do you implement getStaticPaths for dynamic routes?

## Question 18. How do you implement fallback pages for ISR?

## Question 19. How do you handle 404 dynamically for missing content?

## Question 20. How do you implement a search page with query parameters?
