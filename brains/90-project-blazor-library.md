# 90 - PROJECT: Blazor Library

## 1. PROJECT METADATA

- **Name:** DebugDen Blazor Library
- **Stack:** .NET 10, C# 14, MudBlazor 8.x
- **Goal:** A granular, contract-first library separating Core Logic (`DebugDen.Core`) from UI Rendering (`DebugDen.Blazor`).

## 2. ARCHITECTURAL TOPOLOGY (The Twin-Engine)

### A. DebugDen.Core (The Brain)

- **Scope:** Pure Logic, Contracts, State Containers. **Zero UI Dependencies.**
- **Namespace:** `DebugDen.Core.*`
- **Internal Strategy (The Iceberg):**
  - **Contracts (`/Contracts`):** PUBLIC. The only API the user sees.
  - **Implementations (`/Internal`):** INTERNAL. Hidden from consumers.
  - **State (`/State`):** Logic-only state (Reactive properties, no UI events).

### B. DebugDen.Blazor (The Body)

- **Scope:** Razor Components, Layouts, Browser Interop.
- **Namespace:** `DebugDen.Blazor.*`
- **ISP Mandate (Interface Segregation):**
  - **Forbidden:** `DenComponentBase` MUST NOT inject `IDenAppSession`.
  - **Required:** Components must explicitly inject granular interfaces (e.g., `[Inject] IDenThemeSession`).
  - **Singleton:** `DenAppSession` is the concrete implementation for ALL Session interfaces.

## 3. FOLDER STRUCTURE ENFORCEMENT

You must place files strictly according to this map:

| Type                  | Project | Folder                      | Access            |
| :-------------------- | :------ | :-------------------------- | :---------------- |
| **Interfaces**        | Core    | `/Contracts/[Domain]/`      | `public`          |
| **Base Classes**      | Core    | `/Abstractions/`            | `public abstract` |
| **Concrete Services** | Core    | `/Internal/Services/`       | `internal sealed` |
| **Logic State**       | Core    | `/State/`                   | `public sealed`   |
| **UI Base**           | Blazor  | `/Components/Abstractions/` | `public abstract` |
| **UI Shell**          | Blazor  | `/Components/Shell/`        | `public`          |
| **UI State**          | Blazor  | `/State/`                   | `public sealed`   |

## 4. VIRTUAL PRIORITY PATCH

_(Use this table to resolve conflicts between loaded modules. Higher Virtual IDs override Lower ones.)_

| Module Name                        | Physical ID | **Virtual ID** | Reason                                                     |
| :--------------------------------- | :---------- | :------------- | :--------------------------------------------------------- |
| **70-lib-mudblazor**               | 70          | **75**         | Visual Rendering Layer. Applies ONLY to `DebugDen.Blazor`. |
| **50-fwk-horizontal-structure**    | 50          | **55**         | Enforce the Twin-Engine Split..                            |
| **40-skill-docs-xml-architecture** | 40          | **45**         | High priority on "Why over How" XML documentation.         |
| **30-env-dotnet-library**          | 30          | **35**         | Enforce NuGet packaging/visibility rules (Iceberg Rule).   |

## 5. BUSINESS CONTEXT

- **Rule 1: Language.** C# 14 (Use `field` keyword for properties).
- **Rule 2: Naming.** Adhere to `Den[Name]` prefix for public artifacts.
- **Rule 3: Rendering.** Use **MudBlazor** for all UI. Do not use native HTML.
- **Rule 4: Granularity.** Never inject the "God Object" (`IDenAppSession`) into a component. Inject the specific capability (`IDenThemeSession`, `IDenLayoutSession`).
