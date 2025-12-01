# 40 - FRAMEWORK: MICROSOFT IDENTITY

## 1. CONTEXT ARCHITECTURE

* **Inheritance:** Your application's `DbContext` MUST inherit from `IdentityDbContext<ApplicationUser, ApplicationRole, string>`.
* **User Model:** Create an `ApplicationUser` class inheriting from `IdentityUser`.

## 2. CONFIGURATION STRATEGY (Program.cs)

* **Service Registration:** Use `builder.Services.AddIdentity<ApplicationUser, ApplicationRole>()`.
  * **Stores:** Chain `.AddEntityFrameworkStores<YourDbContext>()`.
* **Password Policy:** Enforce strict defaults (RequireDigit, RequiredLength = 12).
* **Lockout:** Enable lockout to prevent brute-force attacks.

## 3. ACCESS & MANAGER USAGE

* **Dependency Injection:**
  * **DO NOT** query the `AspNetUsers` table directly via `DbContext`.
  * **ALWAYS** use `UserManager<ApplicationUser>` for user management.
  * **ALWAYS** use `SignInManager<ApplicationUser>` for session management.
