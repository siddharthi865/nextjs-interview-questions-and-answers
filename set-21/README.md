# Set 21

| #   | Question                                                                                                                                           |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [How do you explain SSR to a non-technical stakeholder?](#question-1-how-do-you-explain-ssr-to-a-non-technical-stakeholder)                        |
| 2   | [How do you set up a Next.js project using Yarn instead of NPM?](#question-2-how-do-you-set-up-a-nextjs-project-using-yarn-instead-of-npm)         |
| 3   | [How do you implement a simple 2-column layout in Next.js?](#question-3-how-do-you-implement-a-simple-2-column-layout-in-nextjs)                   |
| 4   | [How do you add a custom favicon per page?](#question-4-how-do-you-add-a-custom-favicon-per-page)                                                  |
| 5   | [How do you implement a simple footer with links?](#question-5-how-do-you-implement-a-simple-footer-with-links)                                    |
| 6   | [How do you conditionally render a component only on the homepage?](#question-6-how-do-you-conditionally-render-a-component-only-on-the-homepage)  |
| 7   | [How do you implement navigation using both `<Link>` and router.push?](#question-7-how-do-you-implement-navigation-using-both-link-and-routerpush) |
| 8   | [How do you implement optional chaining in a Next.js component?](#question-8-how-do-you-implement-optional-chaining-in-a-nextjs-component)         |
| 9   | [How do you display dynamic content from props?](#question-9-how-do-you-display-dynamic-content-from-props)                                        |
| 10  | [How do you handle basic string formatting in JSX?](#question-10-how-do-you-handle-basic-string-formatting-in-jsx)                                 |
| 11  | [How do you use template literals in JSX?](#question-11-how-do-you-use-template-literals-in-jsx)                                                   |
| 12  | [How do you implement simple event handling, like onClick?](#question-12-how-do-you-implement-simple-event-handling-like-onclick)                  |
| 13  | [How do you pass functions as props in Next.js?](#question-13-how-do-you-pass-functions-as-props-in-nextjs)                                        |
| 14  | [How do you conditionally apply CSS classes in JSX?](#question-14-how-do-you-conditionally-apply-css-classes-in-jsx)                               |
| 15  | [How do you include an external script in `_document.js`?](#question-15-how-do-you-include-an-external-script-in-_documentjs)                      |
| 16  | [How do you handle default props in a functional component?](#question-16-how-do-you-handle-default-props-in-a-functional-component)               |
| 17  | [How do you implement a simple card component using props?](#question-17-how-do-you-implement-a-simple-card-component-using-props)                 |
| 18  | [How do you implement a simple image carousel using state?](#question-18-how-do-you-implement-a-simple-image-carousel-using-state)                 |
| 19  | [How do you implement a simple tabbed interface?](#question-19-how-do-you-implement-a-simple-tabbed-interface)                                     |
| 20  | [How do you implement a simple counter with increment/decrement?](#question-20-how-do-you-implement-a-simple-counter-with-incrementdecrement)      |

## Question 1. How do you explain SSR to a non-technical stakeholder?

**Q: How do you explain SSR to a non-technical stakeholder?**

**Short Answer (30 seconds):**

I explain **Server-Side Rendering (SSR)** using a restaurant analogy. Instead of sending customers raw ingredients and asking them to cook the meal themselves, the kitchen prepares the meal first and serves it ready to eat. Similarly, with SSR, the server prepares the webpage before sending it to the user's browser, resulting in faster initial page loads, better SEO, and a smoother user experience.

---

# Detailed Explanation

## 1. Simple Business Analogy

Imagine you're ordering food from a restaurant.

### Without SSR (Client-Side Rendering)

The restaurant sends you:

- Raw vegetables
- Rice
- Spices
- Recipe

You have to cook everything yourself before eating.

This is similar to the browser downloading JavaScript first and then building the page.

Result:

- Longer waiting time
- Blank loading screen initially
- Worse experience on slow devices

---

### With SSR

The restaurant sends you:

🍽️ A fully cooked meal.

You can start eating immediately.

That's exactly what SSR does.

The server prepares the HTML first and sends the finished page to the browser.

Benefits:

- Faster first impression
- Better user experience
- Works well even on slower devices
- Search engines immediately see the content

---

## 2. Real Business Example

Suppose we own an e-commerce website.

A customer searches:

> "Nike Running Shoes"

### Without SSR

Customer opens the page.

They may briefly see:

```
Loading...
```

Then products appear after JavaScript finishes.

---

### With SSR

Customer immediately sees:

- Product images
- Prices
- Ratings
- Buy button

This increases confidence and reduces the chance they'll leave before the page loads.

---

## 3. Why Businesses Care

Instead of technical details, focus on business outcomes:

- Faster first page load
- Better Google rankings because search engines can index content easily
- Improved user experience
- Higher conversion rates
- Lower bounce rates
- Better performance on slower networks and devices

A stakeholder usually cares more about these outcomes than the rendering mechanism itself.

---

## 4. When SSR Is Most Valuable

SSR is a good fit for pages where content needs to be fresh for every request, such as:

- Product pages with live pricing or stock
- News websites
- Dashboards with user-specific data
- Booking systems
- Personalized homepages

For mostly static content, alternatives like Static Site Generation (SSG) may be more efficient.

---

## 5. Common Questions from Stakeholders

### "Will this make the website faster?"

Yes—for the **initial page load**. Users see meaningful content sooner because the server sends a fully rendered page.

---

### "Why not use SSR everywhere?"

SSR requires the server to generate each page request, which increases server work and infrastructure cost. For pages that rarely change (such as marketing pages or documentation), **Static Site Generation (SSG)** is often faster and more cost-effective.

---

### "Does SSR improve SEO?"

Yes. Since the server returns complete HTML, search engines can crawl and index the page more effectively, making SSR a strong choice for public, content-rich pages.

---

## 6. How Next.js Implements SSR

In **Next.js 16 (App Router)**, Server Components render on the server by default. For pages that need fresh data on every request, you can opt out of caching so the page is rendered dynamically:

```tsx
// app/products/page.tsx

export const dynamic = "force-dynamic";

async function getProducts() {
  const res = await fetch("https://api.example.com/products", {
    cache: "no-store",
  });

  return res.json();
}

export default async function ProductsPage() {
  const products = await getProducts();

  return (
    <main>
      <h1>Products</h1>

      {products.map((product: any) => (
        <div key={product.id}>
          {product.name} - ${product.price}
        </div>
      ))}
    </main>
  );
}
```

This ensures the page is rendered on the server for each request with the latest data.

**Docs:** App Router → Rendering (Server Components, Dynamic Rendering), Data Fetching (`fetch` caching), and Routing in the Next.js 16 documentation.

---

## Performance Considerations

- SSR improves **First Contentful Paint (FCP)** and **Largest Contentful Paint (LCP)** because users receive HTML immediately.
- It can increase **Time to First Byte (TTFB)** since the server must render the page before responding.
- Next.js mitigates this with **streaming**, allowing parts of the page to appear as soon as they're ready rather than waiting for everything to finish.
- Use SSR only for data that truly needs to be generated per request; otherwise, prefer caching or static generation to reduce server load.

---

## Common Pitfalls & Solutions

| Pitfall                                   | Solution                                                                                                  |
| ----------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| Using SSR for every page                  | Use SSG or ISR for mostly static content.                                                                 |
| Expecting SSR to eliminate all JavaScript | SSR improves the initial render, but client-side JavaScript may still be needed for interactive features. |
| Ignoring server load                      | Cache responses where appropriate and use dynamic rendering only when necessary.                          |
| Focusing only on technical benefits       | Explain SSR in terms of business outcomes like user experience, SEO, and conversions.                     |

---

## When to Use

Use SSR when:

- Data changes frequently.
- SEO is important.
- Users should see content immediately.
- Pages are personalized or request-specific.

Examples include e-commerce product pages, news articles, travel booking sites, financial dashboards, and authenticated user experiences.

---

## Alternatives

| Approach | Best For                                                   |
| -------- | ---------------------------------------------------------- |
| **SSR**  | Dynamic, SEO-sensitive, request-specific pages             |
| **SSG**  | Static marketing pages, blogs, documentation               |
| **ISR**  | Mostly static pages that need periodic updates             |
| **CSR**  | Highly interactive dashboards and applications after login |

### **Interview Tip (Non-Technical Audience)**

Avoid terms like _hydration_, _render pipeline_, or _React Server Components_ unless asked. Instead, explain SSR in terms of **customer experience and business value**:

> _"Think of SSR as delivering a fully prepared webpage instead of making every visitor assemble it themselves. They see useful content immediately, search engines can understand it more easily, and that typically leads to happier users and better business outcomes."_

## Question 2. How do you set up a Next.js project using Yarn instead of NPM?

## Question 3. How do you implement a simple 2-column layout in Next.js?

## Question 4. How do you add a custom favicon per page?

## Question 5. How do you implement a simple footer with links?

## Question 6. How do you conditionally render a component only on the homepage?

## Question 7. How do you implement navigation using both `<Link>` and router.push?

## Question 8. How do you implement optional chaining in a Next.js component?

## Question 9. How do you display dynamic content from props?

## Question 10. How do you handle basic string formatting in JSX?

## Question 11. How do you use template literals in JSX?

## Question 12. How do you implement simple event handling, like onClick?

## Question 13. How do you pass functions as props in Next.js?

## Question 14. How do you conditionally apply CSS classes in JSX?

## Question 15. How do you include an external script in `_document.js`?

## Question 16. How do you handle default props in a functional component?

## Question 17. How do you implement a simple card component using props?

## Question 18. How do you implement a simple image carousel using state?

## Question 19. How do you implement a simple tabbed interface?

## Question 20. How do you implement a simple counter with increment/decrement?
