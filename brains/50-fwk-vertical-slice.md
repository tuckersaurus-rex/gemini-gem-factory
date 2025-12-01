# 50 - FWK: VERTICAL SLICES

## 1. PROJECT STRUCTURE

* **Feature Organization:** You must organize code by **Feature**, not by technical file type (Controller/View/Model).
* **Folder Layout:** You must use the structure: `/Features/{FeatureName}/` containing `Page.razor`, `Components/`, `Services`, and `Models`.

## 2. DEPENDENCY FLOW

* **Core Rule:** Features can depend on `/Core`, but Features must NOT depend on other Features.
* **Shared Logic:** If logic is shared between features, you must move it to `/Core` or a specific `/Shared` service.

## 3. TECHNICAL STANDARDS

*(Specific implementation details, syntax preferences, or formatting rules)*
* **Namespace:** Namespaces must match the folder structure (e.g., `MyApp.Features.Products`).
* **Cohesion:** Keep `State`, `Service`, and `Model` classes internal to the Feature unless strictly required elsewhere.
