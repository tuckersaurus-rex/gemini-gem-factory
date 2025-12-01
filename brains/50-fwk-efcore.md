# 50 - FWK: ENTITY FRAMEWORK CORE 10

## 1. CONTEXT LIFECYCLE

* **Factory Pattern:** You MUST inject `IDbContextFactory<TContext>`, not `TContext` directly, to handle Blazor Server concurrency.
* **Usage:** You must create short-lived contexts for every operation using the `using var context = await _factory.CreateDbContextAsync();` pattern.

## 2. QUERY PERFORMANCE

* **No Tracking:** By default, you must apply `.AsNoTracking()` to all read-only queries.
* **Projections:** You must never return raw Entities to the UI; use `.Select(x => new Dto { ... })`.
* **Split Queries:** You must use `.AsSplitQuery()` for queries with large `.Include()` collections to prevent Cartesian Explosion.

## 3. TECHNICAL STANDARDS

*(Specific implementation details, syntax preferences, or formatting rules)*
* **Mutation:** Prefer `ExecuteUpdateAsync` and `ExecuteDeleteAsync` for bulk operations.
* **Concurrency:** Always handle `DbUpdateConcurrencyException` in write operations.
