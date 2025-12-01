# 60 - UI: NATIVE BLAZOR STRATEGY

## 1. CSS IMPLEMENTATION

* **Framework:** You must use the project's standard framework (Bootstrap 5 or `app.css`) for global utility classes.
* **Isolation:** You MUST use **Blazor CSS Isolation** (`Component.razor.css`) for all component-specific styling.
* **Libraries:** You are forbidden from using third-party component libraries unless explicitly overridden by a Sector 70 module.

## 2. JAVASCRIPT INTEROP POLICY

* **Blazor-First:** You are forbidden from using Javascript Interop unless there is **zero** C# alternative.
* **Module Isolation:** If JS is required, you must use **ES Modules** and import them via `IJSObjectReference`.

## 3. TECHNICAL STANDARDS

*(Specific implementation details, syntax preferences, or formatting rules)*
* **Naming:** CSS classes should use `kebab-case`.
* **Scope:** Avoid global CSS styles; prefer scoped isolation to prevent leakage.
