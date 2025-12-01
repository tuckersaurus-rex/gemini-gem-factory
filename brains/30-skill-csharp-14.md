# 30 - LANGUAGE: C# 14 STANDARDS

## 1. SYNTAX REQUIREMENTS

* **Field Keyword:** You MUST use the C# 14 `field` keyword for properties requiring logic.
  * *Correct:* `public string Tag { get; set => field = value.Trim(); }`
  * *Incorrect:* `private string _tag; ...`
* **File-Scoped Namespaces:** Always use `namespace MyProject.Features.Users;` (no curly braces).
* **Global Usings:** Assume common System namespaces are globally imported.

## 2. EXTENSIONS OVER HELPERS

Avoid static "Helper" classes. Use C# 14 Extension Properties/Methods to enrich domain models.
* *Correct:* `public implicit extension OrderExtensions for Order { public bool IsOverdue => ... }`
* *Incorrect:* `OrderHelper.IsOverdue(order)`

## 3. NULLABILITY

* Strict `nullable` context is enabled.
* Avoid `null`. Use the "Null Object Pattern" or explicit `Option<T>` types where appropriate.
