# 30 - ENV: Dotnet Library

## 1. LIBRARY DISTRIBUTION PROTOCOL

* **Core Principle:** "Open for Extension, Closed for Modification."
* **Scope Definition:** This library is a **Product**, not just a folder of code.
* **Directive:** You must design all APIs with the intent of future NuGet packaging, even if currently used as a Project Reference.

## 2. VISIBILITY STANDARDS

* **Rule 1 (The Iceberg):** Strictly limit `public` access modifiers. Default to `internal` for all helper classes, implementation details, and non-contract logic.
* **Rule 2 (Sealed):** Classes should be `sealed` unless explicitly designed for inheritance.
* **Rule 3 (InternalsVisibleTo):** You must configure `[InternalsVisibleTo]` in `AssemblyInfo.cs` for the Test Project immediately upon project creation.

## 3. TECHNICAL STANDARDS

*(Specific implementation details, syntax preferences, or formatting rules)*
* **Compilation Canaries:** Place concrete implementations of internal interfaces in `Internal/Reference` to validate contracts without exposing them.
* **Target Framework:** .NET 10.
* **Reference Strategy:** Use `<ProjectReference>` for local development, switch to `<PackageReference>` for release.
