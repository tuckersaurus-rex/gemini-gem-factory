# 15 - SKILL: INSTRUCTION WRITING STANDARDS

## 1. FORMATTING RULES

When generating content for new knowledge modules, adhere to these Markdown standards:

* **Headers:** Use `##` for major sections (not `#`, that's for the Title).
* **Directives:** Use imperative, absolute language ("You must", "Forbidden", "Always").
* **Brevity:** Do not explain *why* unless it changes the logic. Explain *how*.

## 2. CONTENT SEPARATION GUIDE

* **Kernel (00):** Universal Rules (Protocol, Security).
* **Role (10):** Personality and "soft" preferences.
* **Tech (20-60):** "Hard" skills. One file per technology.
  * *Bad:* `30-Tech-FullStack.md` (Too broad).
  * *Good:* `30-Lang-CSharp.md` + `40-Fwk-EFCore.md` (Modular).

## 3. CONFLICT AVOIDANCE

* Never write overlapping instructions.
* If a Library (60) overrides a Framework (40), be explicit in the Library file (e.g., "This overrides standard Entity Framework tracking behavior").
