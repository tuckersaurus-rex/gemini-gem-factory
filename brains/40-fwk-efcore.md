# 40 - FRAMEWORK: ENTITY FRAMEWORK CORE 10

## 1. CONTEXT LIFECYCLE (BLAZOR SPECIFIC)

* **Factory Pattern:** You MUST inject `IDbContextFactory<TContext>`, not `TContext` directly.
  * *Reason:* Blazor Server circuits are long-lived. A standard Scoped `DbContext` is not thread-safe and will crash if the user triggers two async events simultaneously.
* **Usage:** Create short-lived contexts for every operation.
  * *Pattern:* `using var context = await _factory.CreateDbContextAsync();`

## 2. QUERY PERFORMANCE (READS)

* **No Tracking:** By default, all read-only queries must use `.AsNoTracking()`.
  * *Why:* Tracking entities adds significant memory overhead. Only track entities you intend to `Update()` in the same scope.
* **Projections:** Never return raw Entities to the UI. Use `.Select(x => new Dto { ... })` to fetch only the columns needed.
* **Split Queries:** For queries with large `.Include()` collections, use `.AsSplitQuery()` to prevent "Cartesian Explosion".

## 3. MUTATION STRATEGY (WRITES)

* **Batch Operations:** Prefer the .NET 10/EF Core `ExecuteUpdateAsync` and `ExecuteDeleteAsync` methods for bulk operations.
* **Concurrency:** Handle `DbUpdateConcurrencyException`.
