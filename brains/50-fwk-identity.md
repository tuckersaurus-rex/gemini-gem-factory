# 50 - FWK: MICROSOFT IDENTITY

## 1. CONTEXT ARCHITECTURE

* **Inheritance:** Your `DbContext` MUST inherit from `IdentityDbContext<ApplicationUser, ApplicationRole, string>`.
* **User Model:** You must create an `ApplicationUser` class inheriting from `IdentityUser`.

## 2. CONFIGURATION STRATEGY

* **Service Registration:** You must use `builder.Services.AddIdentity<ApplicationUser, ApplicationRole>()` chained with `.AddEntityFrameworkStores<YourDbContext>()`.
* **Security:** You must enforce strict password defaults (RequireDigit, RequiredLength = 12) and enable lockout.

## 3. TECHNICAL STANDARDS

*(Specific implementation details, syntax preferences, or formatting rules)*
* **Access:** DO NOT query `AspNetUsers` directly. ALWAYS use `UserManager<T>` or `SignInManager<T>`.
* **Keys:** Use `string` (GUIDs) for Primary Keys on Identity tables.
