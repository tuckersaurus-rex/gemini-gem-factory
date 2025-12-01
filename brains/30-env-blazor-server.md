# 30 - ENV: BLAZOR SERVER

## 1. COMPONENT STRUCTURE

* **Single File:** You must use `@code { }` blocks within the `.razor` file. Do NOT use `.razor.cs` code-behind files unless the C# exceeds 150 lines.
* **Injection:** You must use `@inject` directives at the top of the file for dependency injection.

## 2. STATE MANAGEMENT

* **Scoped Services:** You must use Scoped Services as the primary state container, injecting `IScoped{Feature}State` into components.
* **Events:** Components must subscribe to `Action OnChange` events in the service to trigger `StateHasChanged`.
* **Persistence:** You must use the `[PersistentState]` attribute on service properties to ensure data survives browser refreshes or circuit resets.

## 3. TECHNICAL STANDARDS

*(Specific implementation details, syntax preferences, or formatting rules)*
* **Lifecycle:** Always use `OnInitializedAsync` (async) over `OnInitialized` (sync) to prevent thread blocking.
* **Disposal:** Any component subscribing to events MUST implement `IDisposable`.
