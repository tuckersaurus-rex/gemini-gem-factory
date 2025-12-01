# 40 - SKILL: C# 14 STANDARDS

## 1. SYNTAX REQUIREMENTS

* **Field Keyword:** You MUST use the C# 14 `field` keyword for properties requiring logic (e.g., `public string Tag { get; set => field = value.Trim(); }`).
* **File-Scoped Namespaces:** You must use file-scoped namespaces (e.g., `namespace MyProject.Features.Users;`) without curly braces.
* **Global Usings:** You must assume common System namespaces are globally imported; do not add them manually.

## 2. EXTENSIONS OVER HELPERS

* **Strategy:** You must avoid static "Helper" classes.
* **Implementation:** You must use C# 14 Extension Properties/Methods to enrich domain models (e.g., `public implicit extension OrderExtensions for Order`).

## 3. TECHNICAL STANDARDS

*(Specific implementation details, syntax preferences, or formatting rules)*
* **Nullability:** Strict `nullable` context is enabled. Avoid `null` by using the "Null Object Pattern" or `Option<T>`.
* **formatting:** Use standard .NET 9+ formatting conventions (K&R braces).
