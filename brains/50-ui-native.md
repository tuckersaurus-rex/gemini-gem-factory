# 50 - UI: NATIVE BLAZOR STRATEGY

## 1. CSS IMPLEMENTATION

* **Framework:** Use the project's standard framework (Bootstrap 5 or `app.css`) for global utility classes.
* **Isolation:** You MUST use **Blazor CSS Isolation** (`Component.razor.css`) for all component-specific styling.
* **Libraries:** NO third-party component libraries (MudBlazor, Radzen, etc.) are permitted by default.

## 2. JAVASCRIPT INTEROP POLICY

* **Blazor-First:** Javascript Interop is forbidden unless there is **zero** C# alternative (e.g., Focus management, Canvas, LocalStorage).
* **Module Isolation:** If JS is absolutely required, you must use **ES Modules** (`export function...`) and import them via `IJSObjectReference`.
