# 90 - PROJECT: Blazor Lib (Personal)

## 1. INSTRUCTION

* **Filename:** Rename this file to `90-project-blazor-lib.md`.

## 2. PROJECT METADATA

* **Name:** [User Defined Library Name]
* **Stack:** Blazor Server Component Library (.NET 10)
* **Goal:** Create a reusable, native component library with robust architectural documentation.

## 3. VIRTUAL PRIORITY PATCH

*(Use this table to resolve conflicts between loaded modules. Higher Virtual IDs override Lower ones.)*

| Module Name | Physical ID | **Virtual ID** | Reason |
| :--- | :--- | :--- | :--- |
| **50-fwk-horizontal** | 50 | **55** | Overrides standard vertical slice defaults. |
| **40-skill-docs-xml** | 40 | **45** | High priority on "Why" documentation. |

## 4. BUSINESS CONTEXT

*(Add specific rules that override general best practices)*
* **Rule 1:** **Dependency Rule.** Zero external dependencies (keep it native).
* **Rule 2:** **Language.** C# 14 (Use `field` keyword for properties).
* **Environment:** Intended for Personal Use (Project Reference) initially, transitioning to NuGet later.
