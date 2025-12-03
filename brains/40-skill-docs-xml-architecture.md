# 40 - SKILL: CODE DOCUMENTATION

## 1. THE "WHY OVER HOW" PROTOCOL

* **Core Principle:** "Code explains **How**; Comments explain **Why**."
* **The Blind Consumer Rule:** You must write documentation assuming the user **cannot** see the source code. They rely entirely on the IntelliSense pop-up to know how to use the member safely.
* **Directive:** You are forbidden from writing summaries that echo the method name (e.g., "Saves the user" for `SaveUser()`). You must explain the *implication* (e.g., "Persists the user state to the SQL database and invalidates the local cache").

## 2. API SURFACE STANDARDS (IntelliSense)

*(Applies to all Public/Protected members, Classes, and Interfaces)*

* **Block 1: The Summary (The What):** A concise, one-sentence description of the operation.
* **Block 2: The Contract (The Rules):** You MUST explicitly define the boundary conditions.
  * **Parameters:** Do not just list types. Define valid ranges (e.g., "Must be > 0"), formats (e.g., "UTC only"), and nullability.
  * **Returns:** Define what the output represents. If `null` is returned, explain what that *means* (e.g., "Returns null if user is not found").
  * **Exceptions:** You must enumerate **ALL** possible exceptions the code explicitly throws, so the caller can wrap it in a `try/catch`.
* **Block 3: The Context (The Why):** (Mandatory for non-trivial logic). Explain the architectural decision behind the implementation. Why this algorithm? Why this constraint?

## 3. INLINE IMPLEMENTATION STANDARDS

*(Applies to private logic inside the method body)*

* **Rule 1: The "Why" Mandate.** Inline comments must primarily focus on *Intent* (Why this approach?) and *Consequences* (What happens if this is changed?).
* **Rule 2: Self-Documenting Priority.** You must prioritize descriptive variable and method names to explain the *How*. If a comment is needed to explain *what* a line does, your first instinct must be to refactor it into a named method.
* **Rule 3: The Complexity Exception.** You may ONLY use "How" comments (explaining mechanics) if the logic is strictly optimized, mathematical, or uses complex syntax (e.g., Regex, Bitwise), and the comment explicitly aids the code reviewer.
* **Rule 4: The "Section Header" Pattern.** For long methods, use comments to delimit logical phases of execution (e.g., `// 1. Validation`, `// 2. Transformation`).

## 4. TECHNICAL STANDARDS (AGNOSTIC)

*(Map these semantic requirements to the specific language syntax)*

* **C# / .NET:** Use **XML Documentation** (`///`).
  * Summary -> `<summary>`
  * Contract -> `<param>`, `<returns>`, `<exception>`
  * Context -> `<remarks>`
* **TypeScript / JavaScript:** Use **JSDoc** (`/**`).
  * Summary -> Main description.
  * Contract -> `@param`, `@returns`, `@throws`
  * Context -> `@remarks`
* **Python:** Use **Docstrings** (`"""`).
  * Summary -> First line.
  * Contract -> `:param`, `:return`, `:raises`
