# 60 - LIB: TAILWIND CSS STRATEGY

## 1. UTILITY-FIRST PHILOSOPHY

* **No Custom CSS:** Writing custom CSS classes in `.razor.css` or `app.css` is strictly discouraged.
* **Primary Strategy:** Style components exclusively using Tailwind utility classes directly in the HTML.

## 2. CONSISTENCY & CONFIGURATION

* **Source of Truth:** All colors, spacing, and breakpoints must be defined in `tailwind.config.js`.
* **Component Abstraction:** If a set of Tailwind classes is repeated 3+ times, extract it into a small Razor component rather than using `@apply`.

## 3. BLAZOR INTEGRATION

* **Class Merging:** When building reusable components, always allow additional classes to be passed in and merged with the default Tailwind classes.
