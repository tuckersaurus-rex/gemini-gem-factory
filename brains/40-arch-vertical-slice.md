# 40 - FRAMEWORK: VERTICAL SLICES (APPLICATION)

## 1. PROJECT STRUCTURE

Organize code by **Feature**, not by technical file type. High cohesion is the goal.

### Folder Layout

* `/Core`
  * System-wide concerns (Interfaces, Base Classes, Extension Methods, Enums).
* `/Shared`
  * Global UI (MainLayout, NavMenu, common atomic components).
* `/Features`
  * `/{FeatureName}` (e.g., `/Products`)
    * `Page.razor` (The routable entry point)
    * `Components/` (Sub-components specific to this feature)
    * `{Feature}Service.cs` (Data access logic)
    * `{Feature}State.cs` (Scoped state, if needed)
    * `{Feature}Models.cs` (DTOs/View Models)

## 2. DEPENDENCY FLOW

* **Features** can depend on **Core**.
* **Features** should NOT depend on other **Features**.
* If logic is shared between features, move it to **Core** or a specific **Shared Service**.
