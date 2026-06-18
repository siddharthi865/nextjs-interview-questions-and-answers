# Set 22

| #   | Question                                                                                                                                                 |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [How do you add inline styles in JSX?](#question-1-how-do-you-add-inline-styles-in-jsx)                                                                  |
| 2   | [How do you implement a simple list rendering using .map()?](#question-2-how-do-you-implement-a-simple-list-rendering-using-map)                         |
| 3   | [How do you implement a fallback UI for undefined props?](#question-3-how-do-you-implement-a-fallback-ui-for-undefined-props)                            |
| 4   | [How do you handle text truncation with CSS?](#question-4-how-do-you-handle-text-truncation-with-css)                                                    |
| 5   | [How do you implement a simple collapsible component?](#question-5-how-do-you-implement-a-simple-collapsible-component)                                  |
| 6   | [How do you handle simple accessibility features, like aria-label?](#question-6-how-do-you-handle-simple-accessibility-features-like-aria-label)         |
| 7   | [How do you implement a tooltip component?](#question-7-how-do-you-implement-a-tooltip-component)                                                        |
| 8   | [How do you implement a simple notification toast?](#question-8-how-do-you-implement-a-simple-notification-toast)                                        |
| 9   | [How do you handle conditional rendering of a form input?](#question-9-how-do-you-handle-conditional-rendering-of-a-form-input)                          |
| 10  | [How do you implement a simple sticky header?](#question-10-how-do-you-implement-a-simple-sticky-header)                                                 |
| 11  | [How do you implement dynamic SEO meta tags using getStaticProps?](#question-11-how-do-you-implement-dynamic-seo-meta-tags-using-getstaticprops)         |
| 12  | [How do you implement user-specific redirects using SSR?](#question-12-how-do-you-implement-user-specific-redirects-using-ssr)                           |
| 13  | [How do you implement a protected dashboard using getServerSideProps?](#question-13-how-do-you-implement-a-protected-dashboard-using-getserversideprops) |
| 14  | [How do you implement dynamic breadcrumb generation?](#question-14-how-do-you-implement-dynamic-breadcrumb-generation)                                   |
| 15  | [How do you implement server-side filtering of data?](#question-15-how-do-you-implement-server-side-filtering-of-data)                                   |
| 16  | [How do you implement server-side sorting of data?](#question-16-how-do-you-implement-server-side-sorting-of-data)                                       |
| 17  | [How do you implement SSR pagination with query parameters?](#question-17-how-do-you-implement-ssr-pagination-with-query-parameters)                     |
| 18  | [How do you implement infinite scroll with SWR?](#question-18-how-do-you-implement-infinite-scroll-with-swr)                                             |
| 19  | [How do you implement API error handling and display messages?](#question-19-how-do-you-implement-api-error-handling-and-display-messages)               |
| 20  | [How do you implement middleware to log all API requests?](#question-20-how-do-you-implement-middleware-to-log-all-api-requests)                         |

## Question 1. How do you add inline styles in JSX?

**Q: How do you add inline styles in JSX?**

**Short Answer (30 seconds):**

In JSX, inline styles are added using the `style` prop, which accepts a JavaScript object instead of a CSS string. CSS property names use **camelCase** (e.g., `backgroundColor`, `fontSize`) rather than kebab-case.

---

## Detailed Explanation

### 1. Core Concept

Unlike HTML, where styles are written as strings:

```html
<div style="color: red; background-color: yellow;"></div>
```

In JSX, styles are JavaScript objects:

```tsx
<div style={{ color: "red", backgroundColor: "yellow" }}>Hello World</div>
```

Notice the **double curly braces**:

- Outer `{}` → Enter JavaScript expression
- Inner `{}` → JavaScript object

---

### 2. Syntax Rules

- CSS properties become **camelCase**
- Values can be strings or numbers
- Numeric values automatically receive `px` for applicable properties

Example:

```tsx
<div
  style={{
    color: "blue",
    backgroundColor: "#f5f5f5",
    padding: 20,
    marginTop: 10,
    fontSize: 18,
  }}
>
  Styled Content
</div>
```

This renders roughly as:

```css
color: blue;
background-color: #f5f5f5;
padding: 20px;
margin-top: 10px;
font-size: 18px;
```

---

### 3. Dynamic Inline Styles

One major advantage is that styles can be computed dynamically.

```tsx
type ButtonProps = {
  primary: boolean;
};

export default function Button({ primary }: ButtonProps) {
  return (
    <button
      style={{
        backgroundColor: primary ? "#2563eb" : "#6b7280",
        color: "#fff",
        padding: "10px 20px",
        border: "none",
        borderRadius: 6,
      }}
    >
      Click Me
    </button>
  );
}
```

---

### 4. Reusing Style Objects

Instead of repeating styles, define them separately.

```tsx
const cardStyle = {
  border: "1px solid #ddd",
  padding: "20px",
  borderRadius: "8px",
};

export default function Card() {
  return <div style={cardStyle}>Card Content</div>;
}
```

---

### 5. Performance Considerations

Creating a new style object on every render may trigger unnecessary re-renders when passing styles to child components.

Instead of:

```tsx
<div style={{ color: "red" }} />
```

Prefer:

```tsx
const styles = {
  color: "red",
};

<div style={styles} />;
```

Or memoize dynamic styles if needed:

```tsx
const style = useMemo(
  () => ({
    color: isDark ? "white" : "black",
  }),
  [isDark],
);
```

---

### 6. Common Pitfalls & Solutions

**❌ Using a string**

```tsx
<div style="color:red"></div>
```

Invalid in JSX.

**✔ Correct**

```tsx
<div style={{ color: "red" }}></div>
```

---

**❌ Using kebab-case**

```tsx
style={{
  background-color: "red"
}}
```

**✔ Correct**

```tsx
style={{
  backgroundColor: "red"
}}
```

---

**❌ Forgetting double braces**

```tsx
style = { color: "red" };
```

**✔ Correct**

```tsx
style={{ color: "red" }}
```

---

## Code

```tsx
type ProfileCardProps = {
  online: boolean;
};

export default function ProfileCard({ online }: ProfileCardProps) {
  const styles = {
    container: {
      padding: "20px",
      borderRadius: "8px",
      backgroundColor: online ? "#dcfce7" : "#fee2e2",
      color: "#111827",
      border: `1px solid ${online ? "#22c55e" : "#ef4444"}`,
    } satisfies React.CSSProperties,
  };

  return (
    <div style={styles.container}>
      <h2>John Doe</h2>
      <p>Status: {online ? "Online" : "Offline"}</p>
    </div>
  );
}
```

---

### When to use

- Small dynamic style changes
- Conditional colors, spacing, or visibility
- Component-specific styling driven by props or state
- Quick prototypes

---

### Alternatives

- **CSS Modules**: Best for component-scoped styles in production.
- **Tailwind CSS**: Preferred for utility-first styling with excellent maintainability.
- **CSS-in-JS** (e.g., Emotion, Styled Components): Useful for highly dynamic styling requirements.
- **Inline styles**: Best for simple, dynamic values, but they don't support pseudo-classes (`:hover`), media queries, or keyframe animations, so they're generally not the primary styling approach for large Next.js applications.

## Question 2. How do you implement a simple list rendering using .map()?

## Question 3. How do you implement a fallback UI for undefined props?

## Question 4. How do you handle text truncation with CSS?

## Question 5. How do you implement a simple collapsible component?

## Question 6. How do you handle simple accessibility features, like aria-label?

## Question 7. How do you implement a tooltip component?

## Question 8. How do you implement a simple notification toast?

## Question 9. How do you handle conditional rendering of a form input?

## Question 10. How do you implement a simple sticky header?

## Question 11. How do you implement dynamic SEO meta tags using getStaticProps?

## Question 12. How do you implement user-specific redirects using SSR?

## Question 13. How do you implement a protected dashboard using getServerSideProps?

## Question 14. How do you implement dynamic breadcrumb generation?

## Question 15. How do you implement server-side filtering of data?

## Question 16. How do you implement server-side sorting of data?

## Question 17. How do you implement SSR pagination with query parameters?

## Question 18. How do you implement infinite scroll with SWR?

## Question 19. How do you implement API error handling and display messages?

## Question 20. How do you implement middleware to log all API requests?
