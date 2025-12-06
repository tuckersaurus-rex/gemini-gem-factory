# 90 - PROJECT: DebugDen UI Library

## 1. PROJECT METADATA

* **Name:** DebugDen UI Component Library
* **Type:** Razor Class Library (RCL)
* **Stack:** Blazor Server (.NET 10), MudBlazor, DebugDen.Net.Core
* **Goal:** Create a reusable UI library where logic/state is handled by DebugDen.Net.Blazor, and rendering is handled by MudBlazor.

## 2. VIRTUAL PRIORITY PATCH

| Module Name | Physical ID | **Virtual ID** | Reason |
| :--- | :--- | :--- | :--- |
| **70-lib-debugden-net-blazor** | 70 | **78** | Base Classes & State (Must inherit from here). |
| **70-lib-mudblazor** | 70 | **75** | Visual Rendering (Must use these tags). |
| **30-env-dotnet-library** | 30 | **35** | Packaging rules. |

## 3. THE "HYBRID INHERITANCE" PROTOCOL

* **Rule 1: The Inheritance Chain.**
  * You are **strictly forbidden** from inheriting directly from `ComponentBase` or `MudComponentBase`.
  * **Components:** Must inherit `DebugDen.Net.Core.Templates.DenComponentBase`.
  * **Layouts:** Must inherit `DebugDen.Net.Core.Templates.DenLayoutComponentBase` or `DenFrameLayoutComponentBase`.
  * **Pages:** Must inherit `DebugDen.Net.Core.Templates.DenPageBase`.

* **Rule 2: The Rendering Layer.**
  * You must use **MudBlazor** components for all visual elements.
  * **Do not** use native HTML (`div`, `button`, `input`) if a MudBlazor equivalent exists.
  * **Pattern:** Wire the properties from the `Den*Base` class into the MudBlazor component parameters.
    * *Example:* `<MudDrawer Open="@IsNavOpen">` (Where `IsNavOpen` comes from `DenFrameLayoutComponentBase`).

## 4. CONTRACT EVOLUTION PROTOCOL

* **Context:** You are building the *Consumer* (UI Lib), not the *Producer* (Core Lib).
* **Missing Contracts:** If you require a base class, interface, or state property that does not exist in `DebugDen.Net.Core`:
  1.  **Do not** implement it locally in the UI library (this creates technical debt).
  2.  **Do** define the missing contract in a "Proposed Changes" block using the format:
      > **[Request to Core Team]**: Please add `IDenBreadcrumbService` to Core so the UI library can render breadcrumbs generically.

## 5. TECHNICAL STANDARDS

* **Namespace:** `DebugDen.UI.Mud.*` (Do not use `DebugDen.Net.Core` namespace for UI artifacts).
* **Styles:** Use MudBlazor `Class` utilities or `MudTheme` definitions. Avoid custom CSS files (`.razor.css`) unless strictly necessary for layout geometry.
