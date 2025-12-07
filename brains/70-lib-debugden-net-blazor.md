# 70 - LIB: DebugDen.NET.Blazor

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
namespace DebugDen.NET.Blazor.Configuration
{
    public interface IDebugDenOptions
    {
        // Gets the global name of the application (e.g., "My Dashboard").
        string SiteTitle { get; }
        // Gets a brief description of the application used for metadata.
        string SiteDescription { get; }
        // Gets the semantic version string of the application (e.g., "1.0.0").
        string SiteVersion { get; }
        /* * Gets the format string used to compose the page title.
         * Format placeholders: {0} = Page Name, {1} = Site Title.
         * Default: "{0} | {1}"
         */
        string PageTitleFormat { get; }
        // Gets the specific configuration options for client-side features.
        ClientFeatureOptions ClientFeatures { get; }
    }

    public class ClientFeatureOptions
    {
        /* * Determines if Debug-level notifications (Toasts/Snackbars) are rendered in the UI.
         * If false, calls to ShowDebug will be ignored visually.
         */
        public bool EnableDebugNotifications { get; set; }
    }

    // Concrete implementation of IDebugDenOptions for binding to appsettings.json
    public class DebugDenOptions : IDebugDenOptions { /* ... */ }
}

namespace DebugDen.NET.Blazor.Utilities
{
    public static class DebugDenCoreExtensions
    {
        // Registers core DebugDen services (Storage, State Infrastructure).
        public static IServiceCollection AddDebugDenCore(this IServiceCollection services);
    }

    public static class DebugDenConfigurationExtensions
    {
        /* Registers the DebugDen configuration with default options. */
        public static IServiceCollection AddDebugDenConfiguration(this IServiceCollection services, IConfigurationSection section);

        /* Registers the configuration using the "Shadow Options" pattern to merge custom settings. */
        public static IServiceCollection AddDebugDenConfiguration<TUserConfig>(this IServiceCollection services, IConfigurationSection section)
            where TUserConfig : DebugDen.NET.Blazor.Configuration.DebugDenOptions;
    }
}
```

### 4.2. Core Models (Results & Tasks)

```csharp
namespace DebugDen.NET.Blazor.Models
{
    /* Generic Result Pattern */
    public record DenResult<T> : DenResult
    {
        // The data resulting from the operation on success; otherwise, the default value.
        public T? Data { get; init; }

        // Creates a successful result containing data.
        public static DenResult<T> Success(T data);

        // Creates a failed result with a message and optional exception.
        public static new DenResult<T> Failure(string message, Exception? e = null);
    }

    /* Non-Generic Result */
    public record DenResult
    {
        // True if the operation completed successfully; otherwise, False.
        public bool IsSuccess { get; init; }
        // The optional human-readable message describing the error.
        public string? ErrorMessage { get; init; }
        // The optional exception that caused the failure.
        public Exception? Exception { get; init; }

        // Creates a successful result.
        public static DenResult Success();
        // Creates a failed result with a message and optional exception.
        public static DenResult Failure(string message, Exception? e = null);

        /* Implicitly converts the result to a boolean indicating success.
         * Allows usage like: if (result) { ... }
         */
        public static implicit operator bool(DenResult result);
    }

    /* Task Tracking Interface */
    public interface IDenTrackedTask
    {
        // Unique identifier for the task.
        Guid Id { get; }
        // The short title of the task (e.g., "Logging In").
        string Title { get; }
        // The detailed status message (e.g., "Verifying credentials...").
        string Message { get; }
        // The UTC timestamp when the task was created.
        DateTime Timestamp { get; }
    }

    /* Builder Pattern Interface for Immutable Updates */
    public interface IDenTrackedTaskBuilder<out TSelf> where TSelf : IDenTrackedTask
    {
        // Factory method to create a new instance of the task.
        static abstract TSelf Create(Guid id, string? title = null, string? message = null, DateTime? timestamp = null);
        // Creates a new instance with updated properties (Immutable "Wither").
        TSelf With(string? title = null, string? message = null);
    }

    // Default, lightweight implementation of IDenTrackedTask.
    public sealed record DenTrackedTask : IDenTrackedTask, IDenTrackedTaskBuilder<DenTrackedTask>;
}
```

### 4.3. User Identity Models

```csharp
namespace DebugDen.NET.Blazor.Models
{
    /* Core identity contract required by the User Session state. */
    public interface IDenUser<TKey> where TKey : notnull, IEquatable<TKey>
    {
        // Unique identifier for the user.
        TKey Key { get; }
        // The display name or login handle.
        string UserName { get; }
        // The user's email address.
        string Email { get; }
        // True if the user is currently logged in; otherwise, False.
        bool IsAuthenticated { get; }
        // A list of roles or permissions assigned to the user.
        IReadOnlyList<string> Roles { get; }
    }

    /* Defines the contract for creating and modifying immutable user objects. */
    public interface IDenUserBuilder<TKey, out TSelf> where TSelf : IDenUser<TKey>
    {
        /* Factory method to create a fully hydrated user instance. */
        static abstract TSelf Create(TKey key, string? username = null, string? email = null, bool isAuthenticated = false, IEnumerable<string>? roles = null);
        /* Factory method to create a default "Guest" or "Anonymous" user. */
        static abstract TSelf CreateUnauthenticated(TKey key);
        /* Creates a new instance with updated properties (Immutable "Wither"). */
        TSelf With(string? username = null, string? email = null, bool? isAuthenticated = null, IEnumerable<string>? roles = null);
    }

    // Default, lightweight implementation of IDenUser (String Key).
    public sealed record DenUser : IDenUser<string>, IDenUserBuilder<string, DenUser>;
}
```

### 4.4. Services

```csharp
namespace DebugDen.NET.Blazor.Services
{
    // Marker interface for all domain services.
    public interface IDenService {}

    /* Contract for displaying user-facing notifications (Toasts, Snackbars). */
    public interface IDenNotificationService : IDenService
    {
        // Displays a success message to the user.
        void ShowSuccess(string message, string? title = null);
        // Displays an informational message to the user.
        void ShowInfo(string message, string? title = null);
        // Displays a warning message to the user.
        void ShowWarning(string message, string? title = null);
        // Displays an error message to the user.
        void ShowError(string message, string? title = null);
        // Displays a debug message if ClientFeatureOptions.EnableDebugNotifications is true.
        void ShowDebug(string message, string? title = null);
    }

    /* Abstract contract for persisting simple state (local storage / session storage). */
    public interface IDenStorageService : IDenService
    {
        /* Retrieves a value from the storage provider asynchronously.
         * Returns default(T) if the key is not found or cannot be decrypted.
         */
        Task<T?> GetItemAsync<T>(string key);
        // Persists a value to the storage provider asynchronously.
        Task SetItemAsync<T>(string key, T value);
        // Removes a value from the storage provider asynchronously.
        Task RemoveItemAsync(string key);
    }

    // Server-side implementation of IDenStorageService using Protected Browser Storage.
    public sealed class DenLocalStorageService : IDenStorageService, IDenService;
}
```

### 4.5. State Management (The Core)

```csharp
namespace DebugDen.NET.Blazor.State
{
    /* Represents the lifecycle status of a state container. */
    public enum DenStateStatus { Uninitialized, Loading, Ready, Busy, Error }

    /* Defines the available theme modes for the application. */
    public enum DenThemeMode { System, Light, Dark }

    /* Contract for a managed state container. */
    public interface IDenState : IDisposable, IAsyncDisposable
    {
        // True if initialization has completed successfully.
        bool IsInitialized { get; }
        // True if there are active background tasks tracked by this state.
        bool IsBusy { get; }
        /* True if the state is usable (Ready or Busy), false if Loading or Error. */
        bool IsReady { get; }
        // Contains the exception if initialization failed.
        Exception? LoadError { get; }
        // Gets the current lifecycle status.
        DenStateStatus Status { get; }

        // Starts the initialization process for the state container.
        Task InitializeAsync();

        // Triggered when the data within the state changes.
        event Action? OnStateChanged;
        // Triggered when the Status (Busy/Ready/Error) changes.
        event Action? OnStatusChanged;
    }

    /* Base class for state containers implementing the "Tracked Task" pattern. */
    public abstract class DenStateBase<TTrackedTask> : IDenState, IDisposable, IAsyncDisposable
        where TTrackedTask : DebugDen.NET.Blazor.Models.IDenTrackedTask, DebugDen.NET.Blazor.Models.IDenTrackedTaskBuilder<TTrackedTask>
    {
        // Lifecycle Methods
        // OVERRIDE THIS for core data loading/fetching.
        protected virtual Task InitializeCoreAsync();
        // Configures the "Loading..." message used during initialization.
        protected virtual TTrackedTask InitializeConfig(TTrackedTask task);

        // Actions (Wraps logic in Busy/Ready state, handles errors, triggers events)
        protected Task ExecuteTrackedAsync(Func<Task> action, string? title = null, string? message = null, bool propagateException = true);
        // Shortcut to call ExecuteTrackedAsync with a configure function
        protected Task ExecuteTrackedAsync(Func<TTrackedTask, TTrackedTask>? configure, Func<Task> action, bool propagateException = true);
        protected Task<T?> ExecuteTrackedAsync<T>(Func<Task<T>> action, string? title = null, string? message = null, bool propagateException = true);
        // Shortcut to call ExecuteTrackedAsync<T> with a configure function
        protected Task<T?> ExecuteTrackedAsync<T>(Func<TTrackedTask, TTrackedTask>? configure, Func<Task<T>> action, bool propagateException = true);
        // Overloads exist for operations reporting progress via IProgress<Func<TTrackedTask, TTrackedTask>>

        // Updates an existing task (e.g. progress update)
        protected void UpdateTrackedTask(Guid id, Func<TTrackedTask, TTrackedTask> logic);
    }

    /* Non-generic contract for Layout State. */
    public interface IDenLayoutSession : IDenState
    {
        // Gets whether the main navigation sidebar is currently open.
        bool IsNavOpen { get; }
        // Toggles the state of the navigation sidebar.
        void ToggleNav();
        // Explicitly sets the state of the navigation sidebar.
        void SetNavState(bool isOpen);
        // Event raised when layout properties (like Sidebar Open) change.
        event Action? OnLayoutChanged;
    }

    /* The full Application Session contract including Theme Management. */
    public interface IDenAppSession<out TTheme> : IDenLayoutSession, IDenState
    {
        // Gets the current theme object.
        TTheme CurrentTheme { get; }
        // Gets the current theme mode setting.
        DenThemeMode ThemeMode { get; }
        // Gets whether the application is actually rendered in Dark Mode.
        bool IsDarkMode { get; }
        // Gets the detected system color scheme preference.
        bool SystemDarkMode { get; }

        // Cycles to the next available theme mode (System -> Light -> Dark).
        Task CycleThemeModeAsync();
        // Sets a specific theme mode and persists it.
        Task SetThemeModeAsync(DenThemeMode mode);
        // Updates the internal knowledge of the client's system preference (called via JS).
        void SetSystemDarkMode(bool isSystemDark);
    }

    // Base class for application session state.
    public abstract class DenAppSessionBase<TTheme, TTrackedTask> : DenStateBase<TTrackedTask>, IDenAppSession<TTheme>
    { /* ... */ }

    /* Contract for the User Session state container. */
    public interface IDenUserSession<TKey> : IDenState, DebugDen.NET.Blazor.Models.IDenUser<TKey>
        where TKey : notnull, IEquatable<TKey>
    { /* ... */ }

    /* Base class for User Session management. */
    public abstract class DenUserSessionBase<TKey, TUser, TTrackedTask> : DenStateBase<TTrackedTask>, IDenUserSession<TKey>
        where TKey : notnull, IEquatable<TKey>
        where TUser : class, DebugDen.NET.Blazor.Models.IDenUser<TKey>, DebugDen.NET.Blazor.Models.IDenUserBuilder<TKey, TUser>
        where TTrackedTask : DebugDen.NET.Blazor.Models.IDenTrackedTask, DebugDen.NET.Blazor.Models.IDenTrackedTaskBuilder<TTrackedTask>
    {
        // The current user state object.
        protected TUser CurrentUser { get; }

        // Required hook to fetch the current user state from the provider.
        protected abstract Task<TUser?> LoadUserAsync();

        // Checks if the current user possesses a specific role (case-insensitive).
        public bool IsInRole(string role);

        // Transactions (Login/Logout wrappers)
        // Executes a Login Transaction. Action returns the user on success, or null on failure.
        protected Task<DebugDen.NET.Blazor.Models.DenResult> ExecuteLoginCommandAsync(Func<Task<TUser?>> action, string? attemptIdentifier = null);
        // Executes a Logout Transaction. Action performs the provider-specific logout logic.
        protected Task<DebugDen.NET.Blazor.Models.DenResult> ExecuteLogoutCommandAsync(Func<Task> action);
        // Executes a Session Refresh Transaction. Action returns the refreshed user.
        protected Task<DebugDen.NET.Blazor.Models.DenResult> ExecuteRefreshCommandAsync(Func<Task<TUser?>> action);
        // Executes an Update Transaction. Action returns the updated user.
        protected Task<DebugDen.NET.Blazor.Models.DenResult> ExecuteUpdateCommandAsync(Func<Task<TUser?>> action);
    }
}
```

### 4.6. UI Templates (Blazor Components)

```csharp
namespace DebugDen.NET.Blazor.Templates
{
    /* The abstract base class for all UI components. */
    public abstract class DenComponentBase : ComponentBase, IDisposable, IAsyncDisposable
    {
        // Custom CSS classes applied to the root element.
        [Parameter] public string? Class { get; set; }
        // Custom CSS styles applied to the root element.
        [Parameter] public string? Style { get; set; }
        /* Captures any unmatched HTML attributes (e.g., aria-label)
         * to pass to the root element.
         */
        [Parameter(CaptureUnmatchedValues = true)] public Dictionary<string, object>? AdditionalAttributes { get; set; }

        // Register cleanup logic (synchronous action)
        protected void Register(Action dispose);
        /* Executes the subscription immediately, and registers the unsubscription for disposal.
         * Usage: Register(() => State.OnChange += Handle, () => State.OnChange -= Handle);
         */
        protected void Register(Action subscribe, Action unsubscribe);
    }

    /* Defines descriptive metadata for a routable Blazor component or page. */
    [AttributeUsage(AttributeTargets.Class)]
    public sealed class DenPageAttribute(string name) : Attribute;

    /* The base class for all Routable Pages in the application. */
    public abstract class DenPageBase : DenComponentBase
    {
        // The human-readable name of the current page (Reads DenPageAttribute).
        public virtual string PageName { get; }
        // The canonical route pattern for this page.
        public virtual string PageRoute { get; }

        /* Generates the full page title using the current PageName and configured SiteTitle.
         * Example: "Dashboard" -> "Dashboard | My App"
         */
        protected string Title();
        protected string Title(string pageName);
        protected string Title(string pageName, string appNameOverride);
    }

    /* The base class for all Layouts. Adds the required Body parameter. */
    public abstract class DenLayoutComponentBase : DenComponentBase
    {
        [Parameter] public RenderFragment? Body { get; set; }
    }

    /* A state-aware base class for "Frame" layouts (Sidebar + AppBar + Content). */
    public abstract class DenFrameLayoutComponentBase : DenLayoutComponentBase
    {
        // Indicates if the Sidebar/Navigation drawer is currently visible.
        protected bool IsNavOpen { get; }
        // Toggles the Sidebar state.
        protected void ToggleNav();
        // Explicitly sets the Sidebar state.
        protected void SetNavState(bool isOpen);
    }
}
```

### 4.7. Utilities (Logging)

```csharp
namespace DebugDen.NET.Blazor.Utilities
{
    /* Source-generated logging extensions for high-performance structured logging. */
    public static class DenLoggerExtensions
    {
        // Logs successful component initialization.
        public static void LogComponentInitialized(this ILogger logger);
        // Logs internal component state changes for debugging.
        public static void LogComponentDebug(this ILogger logger, string componentType, string message);
        // Logs a success notification shown to the client.
        public static void LogNotificationSuccess(this ILogger logger, string title, string message);
        // Logs a debug notification shown to the client.
        public static void LogDebugVisual(this ILogger logger, string title, string message);
        // Logs an info notification shown to the client.
        public static void LogNotificationInfo(this ILogger logger, string title, string message);
        // Logs an error that is being displayed to the client.
        public static void LogErrorClient(this ILogger logger, string title, string message);
        // Logs a warning that is being displayed to the client.
        public static void LogWarningClient(this ILogger logger, string title, string message);
        // Logs security-relevant access attempts.
        public static void LogSecurityAudit(this ILogger logger, string identifier);
    }
}
```
