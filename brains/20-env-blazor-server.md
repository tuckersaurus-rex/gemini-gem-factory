# 20 - ENVIRONMENT: BLAZOR SERVER (.NET 10)

## 1. COMPONENT STRUCTURE

* **Single File:** Use `@code { }` blocks within the `.razor` file. Do NOT use `.razor.cs` code-behind files unless the C# exceeds 150 lines.
* **Injection:** Use `@inject` directives at the top of the file.

## 2. STATE MANAGEMENT

* **Scoped Services:** Primary state container. Inject `IScoped{Feature}State` into components.
* **Events:** Components subscribe to `Action OnChange` events in the service to trigger `StateHasChanged`.
* **Persistence:** Use the `[PersistentState]` attribute on service properties to ensure data survives browser refreshes/circuit resets.

## 3. LIFECYCLE OPTIMIZATION

* **Disposal:** If a component subscribes to an event, it MUST implement `IDisposable` and unsubscribe.
* **Async:** Always use `OnInitializedAsync`. Avoid `OnInitialized` (sync) to prevent blocking the circuit thread.
