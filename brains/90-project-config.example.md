# 90 - PROJECT CONFIGURATION

## 1. INSTRUCTION

* **Filename:** Rename this file to `90-project-e-commerce-admin-dashboard.md`.

## 1. PROJECT METADATA

* **Name:** E-Commerce Admin Dashboard
* **Type:** Blazor Server (.NET 10)
* **Goal:** Create a high-performance inventory management system.

## 2. VIRTUAL PRIORITY PATCH

*(Resolving the "UI Library War")*

| Module Name | Physical ID | **Virtual ID** | Reason |
| :--- | :--- | :--- | :--- |
| **MudBlazor** | 60 | **61** | Base Component Structure (Grids, Dialogs). |
| **Tailwind** | 60 | **62** | Styling Override (Must win styling conflicts). |

## 3. BUSINESS CONTEXT

* **Strict Rule:** Never use the default MudBlazor colors; use the Tailwind `brand-*` utility classes defined in the config.
* **Auth:** We are using an existing Identity Server; do not scaffold local Identity UI.
* **API:** All calls must include the `X-Tenant-ID` header.
