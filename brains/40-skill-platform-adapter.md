# 40 - SKILL: PLATFORM ADAPTER

## 1. ADAPTATION PHILOSOPHY

- **Directive:** Treat the Knowledge Base as "Source Code" and the target platform as the "Compiler Target."

## 2. TARGET: GITHUB COPILOT (`.github/copilot-instructions.md`)

- **Strategy:** **The Flattened Stream.**
- **Transformation:**
  1.  **Strip:** Remove `=== MODULE ===` headers.
  2.  **Concatenate:** Merge modules strictly by priority (Kernel -> Core -> Role -> Skill).
  3.  **Simplify:** Convert complex tables into bulleted lists to save context tokens.

## 3. TARGET: CURSOR AI (`.cursorrules`)

- **Strategy:** **The Context Map.**
- **Transformation:**
  1.  **Global:** Map `Kernel/Core` protocols to the "Global Context."
  2.  **Scoped:** Map `Skills` (C#) to file-pattern rules (e.g., `**/*.cs` triggers `40-skill-csharp`).

## 4. TARGET: CHATGPT / CLAUDE (System Prompt)

- **Strategy:** **The Bootloader Injection.**
- **Transformation:**
  1.  **Inject:** Start with "You are acting as [Role Name]."
  2.  **Embed:** Paste the `00-kernel` logic immediately to establish the rules of engagement.
  3.  **Summarize:** If the system is large, provide summaries of `Competency` modules rather than full text.
