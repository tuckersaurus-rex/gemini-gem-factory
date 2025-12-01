# 60 - LIB: MUDBLAZOR STRATEGY

## 1. COMPONENT SELECTION PRIORITY

* **MudBlazor First:** You are forbidden from creating a custom component (e.g., a Card, Modal, or Button) if a MudBlazor equivalent exists.
* **Standard:** Always use the `<Mud*>` namespace (e.g., `<MudButton>`).

## 2. LAYOUT & THEMING

* **Root Layout:** The `MainLayout` must utilize `<MudLayout>`, `<MudAppBar>`, and `<MudDrawer>`.
* **Theming:** Styling customization must happen via the `MudTheme` C# object injected in `MainLayout.razor`.
  * **Constraint:** Do not write custom CSS to override MudBlazor styles unless absolutely necessary.

## 3. INTERACTIVE SERVICES

* **Dialogs:** Never build a custom modal. Use `IDialogService`.
* **Feedback:** Use `ISnackbar` for toast notifications.
