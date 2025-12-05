=== MODULE: 70-lib-debug-den-core ===

# 70 - LIB: DebugDen.Net.Core

## 1. COMPONENT ARCHITECTURE

* **Inheritance:** You must inherit all generic UI components from `DenComponentBase`.
* **Pages:** You must inherit all Routable Pages (`@page`) from `DenPageBase`.
* **Metadata:** You must decorate all Pages with the `[DenPage("Title")]` attribute to ensure correct breadcrumb and document title generation.
* **Layouts:** You must inherit Main Layouts from `DenLayoutComponentBase` or `DenFrameLayoutComponentBase` to automatically wire up the `IDenLayoutSession`.

## 2. STATE MANAGEMENT PROTOCOLS

* **Pattern:** You are forbidden from using raw C# classes for state. You must derive all State Containers from `DenStateBase<TTrackedTask>`.
* **Initialization:** You must override `InitializeCoreAsync()` for data fetching. You must not put async logic in the constructor.
* **Updates:** You must use `ExecuteTrackedAsync` to perform actions that modify state. This automatically handles `IsBusy`, `LoadError`, and `NotifyStateChanged`.
* **Session:** For global app state (Theme, Sidebar), you must inherit from `DenAppSessionBase`.

## 3. SERVICE LAYER STANDARDS

* **Return Types:** Service methods MUST return `DenResult` or `DenResult<T>`. You are forbidden from throwing expected exceptions for validation or logic failures.
* **Feedback:** You must use `IDenNotificationService` to display toasts (Success, Error, Warning) instead of `Console.WriteLine` or JS interop directly.
* **Logging:** You must use the `DenLoggerExtensions` (e.g., `logger.LogComponentDebug()`) for consistent structured logging.

## 4. API CONTRACTS (Reference)

### 4.1. Configuration & Dependency Injection

```csharp
namespace DebugDen.Net.Core.Configuration;

public interface IDebugDenOptions
{
    string SiteTitle { get; }
    string SiteDescription { get; }
    string SiteVersion { get; }
    string PageTitleFormat { get; } // Default: "{0} | {1}"
    ClientFeatureOptions ClientFeatures { get; }
}

public class ClientFeatureOptions
{
    public bool EnableDebugNotifications { get; set; }
}

namespace DebugDen.Net.Core.Utilities;

public static class DebugDenCoreExtensions
{
    // Registers Storage, State Infrastructure, and default Services
    public static IServiceCollection AddDebugDenCore(this IServiceCollection services);
}

public static class DebugDenConfigurationExtensions
{
    // Binds "DebugDen" section from appsettings
    public static IServiceCollection AddDebugDenConfiguration(this IServiceCollection services, IConfigurationSection section);
}
```

### 4.2. Core Models (Results & Tasks)

```csharp
namespace DebugDen.Net.Core.Models;

// Generic Result Pattern
public record DenResult<T> : DenResult
{
    public T? Data { get; }
    public static DenResult<T> Success(T data);
    public static new DenResult<T> Failure(string message, Exception? e = null);
}

// Non-Generic Result
public record DenResult
{
    public bool IsSuccess { get; }
    public string? ErrorMessage { get; }
    public Exception? Exception { get; }

    public static DenResult Success();
    public static DenResult Failure(string message, Exception? e = null);

    // Allows "if (result)" check
    public static implicit operator bool(DenResult result);
}

// Task Tracking Interface
public interface IDenTrackedTask
{
    Guid Id { get; }
    string Title { get; }   // e.g. "Saving..."
    string Message { get; } // e.g. "Writing to database"
    DateTime Timestamp { get; }
}

// Default Implementation
public sealed record DenTrackedTask : IDenTrackedTask;
```

### 4.3. User Identity Models

```csharp
namespace DebugDen.Net.Core.Models;

public interface IDenUser<TKey> where TKey : notnull, IEquatable<TKey>
{
    TKey Key { get; }
    string UserName { get; }
    string Email { get; }
    bool IsAuthenticated { get; }
    IReadOnlyList<string> Roles { get; }
}

// Builder Pattern Interface for Immutable Updates
public interface IDenUserBuilder<TKey, out TSelf> where TSelf : IDenUser<TKey>
{
    static abstract TSelf Create(TKey key, string? username = null, string? email = null, bool isAuthenticated = false, IEnumerable<string>? roles = null);
    static abstract TSelf CreateUnauthenticated(TKey key);
    TSelf With(string? username = null, string? email = null, bool? isAuthenticated = null, IEnumerable<string>? roles = null);
}

// Default Implementation (String Key)
public sealed record DenUser : IDenUser<string>, IDenUserBuilder<string, DenUser>;
```

### 4.4. Services

```csharp
namespace DebugDen.Net.Core.Services;

public interface IDenNotificationService : IDenService
{
    void ShowSuccess(string message, string? title = null);
    void ShowInfo(string message, string? title = null);
    void ShowWarning(string message, string? title = null);
    void ShowError(string message, string? title = null);
    void ShowDebug(string message, string? title = null); // Respects ClientFeatureOptions
}

public interface IDenStorageService : IDenService
{
    Task<T?> GetItemAsync<T>(string key);
    Task SetItemAsync<T>(string key, T value);
    Task RemoveItemAsync(string key);
}
```

### 4.5. State Management (The Core)

```csharp
namespace DebugDen.Net.Core.State;

// --- BASE STATE CONTAINER ---
public abstract class DenStateBase<TTrackedTask> : IDenState, IDisposable, IAsyncDisposable
{
    // Properties
    public bool IsInitialized { get; }
    public bool IsBusy { get; }
    public bool IsReady { get; } // True if Initialized AND (Ready OR Busy) AND No Error
    public Exception? LoadError { get; }
    public DenStateStatus Status { get; } // Uninitialized, Loading, Ready, Busy, Error

    // Lifecycle Methods
    public Task InitializeAsync(); // Called by UI
    protected virtual Task InitializeCoreAsync(); // OVERRIDE THIS for data loading
    protected virtual TTrackedTask InitializeConfig(TTrackedTask task); // Config "Loading..." message

    // Actions
    // Wraps logic in Busy/Ready state, handles errors, triggers StateChanged
    protected Task ExecuteTrackedAsync(Func<Task> action, string? title = null, string? message = null);
    protected Task<T?> ExecuteTrackedAsync<T>(Func<Task<T>> action, string? title = null, string? message = null);

    // Updates an existing task (e.g. progress update)
    protected void UpdateTrackedTask(Guid id, Func<TTrackedTask, TTrackedTask> logic);

    // Events
    public event Action? OnStateChanged;  // Data Changed
    public event Action? OnStatusChanged; // Busy/Error Changed
}

// --- APP SESSION (Global UI State) ---
public abstract class DenAppSessionBase<TTheme, TTrackedTask> : DenStateBase<TTrackedTask>, IDenAppSession<TTheme>
{
    public TTheme CurrentTheme { get; }
    public DenThemeMode ThemeMode { get; } // System, Light, Dark
    public bool IsDarkMode { get; }        // Resolved value
    public bool IsNavOpen { get; }

    // Actions
    public Task SetThemeModeAsync(DenThemeMode mode);
    public Task CycleThemeModeAsync();
    public void ToggleNav();
    public void SetNavState(bool isOpen);
}

// --- USER SESSION (Auth State) ---
public abstract class DenUserSessionBase<TKey, TUser, TTrackedTask> : DenStateBase<TTrackedTask>, IDenUserSession<TKey>
    where TUser : IDenUser<TKey>
{
    public TUser CurrentUser { get; }
    public bool IsAuthenticated { get; }
    public bool IsInRole(string role);

    // Abstract Hooks
    protected abstract Task<TUser?> LoadUserAsync(); // Fetch from API/Storage

    // Transactions (Login/Logout wrappers)
    protected Task<DenResult> ExecuteLoginCommandAsync(Func<Task<TUser?>> action);
    protected Task<DenResult> ExecuteLogoutCommandAsync(Func<Task> action);
    protected Task<DenResult> ExecuteRefreshCommandAsync(Func<Task<TUser?>> action);
}
```

### 4.6. UI Templates (Blazor Components)

```csharp
namespace DebugDen.Net.Core.Templates;

// --- COMPONENT BASE ---
public abstract class DenComponentBase : ComponentBase, IDisposable, IAsyncDisposable
{
    [Parameter] public string? Class { get; set; }
    [Parameter] public string? Style { get; set; }
    [Parameter(CaptureUnmatchedValues = true)] public Dictionary<string, object>? AdditionalAttributes { get; set; }

    // Utilities
    protected void Register(Action dispose); // Register cleanup logic
    protected void Register(Action subscribe, Action unsubscribe); // Auto-manage event subscription
}

// --- PAGE BASE ---
[AttributeUsage(AttributeTargets.Class)]
public sealed class DenPageAttribute(string name) : Attribute;

public abstract class DenPageBase : DenComponentBase
{
    public virtual string PageName { get; } // Reads DenPageAttribute
    public virtual string PageRoute { get; }

    // Generates "Page Name | Site Title"
    protected string Title();
    protected string Title(string pageName);
}

// --- LAYOUTS ---
public abstract class DenLayoutComponentBase : DenComponentBase
{
    [Parameter] public RenderFragment? Body { get; set; }
}

public abstract class DenFrameLayoutComponentBase : DenLayoutComponentBase
{
    // Auto-wires to IDenLayoutSession
    public bool IsNavOpen { get; }
    public void ToggleNav();
}
```

### 4.7. Utilities (Logging)

```csharp
namespace DebugDen.Net.Core.Utilities;

public static class DenLoggerExtensions
{
    // High-performance structured logging
    public static void LogComponentInitialized(this ILogger logger);
    public static void LogComponentDebug(this ILogger logger, string componentType, string message);
    public static void LogNotificationSuccess(this ILogger logger, string title, string message);
    public static void LogErrorClient(this ILogger logger, string title, string message);
    public static void LogSecurityAudit(this ILogger logger, string identifier);
}
```
