# 90 - PROJECT: Blazor Library Engineer

## 1. INSTRUCTION

* **Filename:** Rename this file to `90-project-blazor-library-engineer.md`.

## 2. PROJECT METADATA

* **Name:** Blazor Library Engineer
* **Stack:** Blazor Component Library (.NET 10), C# 14, MudBlazor
* **Goal:** Create a unified, reusable library containing both core logic (Data/Services) and UI components (Blazor/MudBlazor) with robust architectural documentation.

## 3. VIRTUAL PRIORITY PATCH

*(Use this table to resolve conflicts between loaded modules. Higher Virtual IDs override Lower ones.)*

| Module Name | Physical ID | **Virtual ID** | Reason |
| :--- | :--- | :--- | :--- |
| **70-lib-mudblazor** | 70 | **75** | Visual Rendering Layer. Overrides 60-ui-native CSS/Interop rules. |
| **50-fwk-horizontal-structure** | 50 | **55** | Enforce Horizontal Topology (UI/Service/Data/State). |
| **40-skill-docs-xml-architecture** | 40 | **45** | High priority on "Why over How" XML documentation. |
| **30-env-dotnet-library** | 30 | **35** | Enforce NuGet packaging/visibility rules (Iceberg Rule). |

## 4. BUSINESS CONTEXT

*(Add specific rules that override general best practices)*
* **Rule 1: Language.** C# 14 (Use `field` keyword for properties).
* **Rule 2: The Contract Constraint.** The output of this project **IS** the `DebugDen.NET.Blazor` library. All new components/services must adhere to the *pattern* and *naming* of the `DebugDen.NET.Blazor` contracts (e.g., `DenComponentBase`, `DenStateBase`, `DenResult`).
* **Rule 3: The Inheritance Chain (UI).** You are **strictly forbidden** from inheriting directly from `ComponentBase` or `MudComponentBase`. Components must inherit from the equivalent `Den*Base` class defined in the project's **Templates** folder.
* **Rule 4: The Rendering Layer (UI).** You must use **MudBlazor** components for all visual elements. Do not use native HTML (`div`, `button`, `input`) if a MudBlazor equivalent exists.
* **Environment:** Intended for Project Reference initially, transitioning to NuGet later.
