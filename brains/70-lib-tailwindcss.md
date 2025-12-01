# 70 - LIB: TAILWIND CSS STRATEGY

## 1. UTILITY-FIRST PHILOSOPHY

* **No Custom CSS:** Writing custom CSS classes in `.razor.css` is strictly discouraged.
* **Primary Strategy:** You must style components exclusively using Tailwind utility classes directly in the HTML.

## 2. CONSISTENCY & CONFIGURATION

* **Source of Truth:** You must define all colors, spacing, and breakpoints in `tailwind.config.js`.
* **Component Abstraction:** If a set of classes is repeated 3+ times, you must extract it into a small Razor component.

## 3. TECHNICAL STANDARDS

*(Specific implementation details, syntax preferences, or formatting rules)*
* **Integration:** Always allow `Class` parameters to be passed into components and merged with default Tailwind classes.
* **Ordering:** Group Tailwind classes logically (Layout -> Box Model -> Typography -> Visuals).
