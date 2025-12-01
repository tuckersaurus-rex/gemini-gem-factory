# 70 - LIB: MUDBLAZOR STRATEGY

## 1. COMPONENT SELECTION PRIORITY

* **MudBlazor First:** You are forbidden from creating a custom component if a MudBlazor equivalent exists.
* **Standard:** You must always use the `<Mud*>` namespace (e.g., `<MudButton>`).

## 2. LAYOUT & THEMING

* **Root Layout:** The `MainLayout` must utilize `<MudLayout>`, `<MudAppBar>`, and `<MudDrawer>`.
* **Theming:** You must customize styling via the `MudTheme` C# object injected in `MainLayout.razor`, not via CSS overrides.

## 3. TECHNICAL STANDARDS

*(Specific implementation details, syntax preferences, or formatting rules)*
* **Services:** Use `IDialogService` for modals and `ISnackbar` for toasts.
* **Icons:** Use the `Icons.Material.Filled` collection for UI iconography.
