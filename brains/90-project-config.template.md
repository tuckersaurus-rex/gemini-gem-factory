# 90 - PROJECT CONFIGURATION

## 1. INSTRUCTION

* **Filename:** Rename this file to `90-project-[your-project-name].md`.

## 2. PROJECT METADATA

* **Name:** [Insert Project Name]
* **Type:** [e.g., Blazor Server, Python Script, React App]
* **Goal:** [Brief description of the objective]

## 3. VIRTUAL PRIORITY PATCH

*(Use this table to resolve conflicts between loaded modules. Higher Virtual IDs override Lower ones.)*

| Module Name | Physical ID | **Virtual ID** | Reason |
| :--- | :--- | :--- | :--- |
| **[Module A]** | [Physical Sector] | **[New ID]** | [Why this module takes precedence] |
| **[Module B]** | [Physical Sector] | **[New ID]** | [Why this module yields] |

## 4. BUSINESS CONTEXT

*(Add specific rules that override general best practices)*
* **Rule 1:** [e.g., "All dates must be UTC."]
* **Rule 2:** [e.g., "Users cannot delete records, only archive them."]
* **Environment:** [e.g., "Using Azure AD for auth."]
