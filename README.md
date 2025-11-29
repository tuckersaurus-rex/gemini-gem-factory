# Gemini Gem Configuration Library

## 📂 Repository Structure

This repository utilizes the **Two-Slot Architecture** to separate static instructions from dynamic project data.

| Directory | Purpose | Protocol |
| :--- | :--- | :--- |
| **`/brains`** | **Slot 1: The OS/Kernel.** Contains core logic, roles, standards, and permanent library code. | **Strict.** Files must be `000` to `899` (Century Protocol). |
| **`/context`** | **Slot 2: The Data Index.** Contains the library inventory (`999` manifest). | **Strict.** Contains the data indexes and metadata. |
| **`/releases`** | Archived Zip files of built Brains. | N/A |
| **`.github/`** | CI/CD Workflows. | N/A |
| **Root** | Contains the `pack-context.py` script and documentation. | N/A |

---

## 🚀 Workflow: Creating a New Project Gem (Optimized)

This optimized workflow leverages the **GitHub App Integration** to provide live code context (Data) while retaining the Gem's static instruction structure (Rules).

| Step | Location | Action | Purpose |
| :--- | :--- | :--- | :--- |
| **1. Design & Plan** | Gemini Chat | Ask the **Gem Architect** to design the required instruction files using the template below. | Generates the file list and any new module content needed. |
| **2. Repo Development** | Local Repo | Create a feature branch and add any new `.md` files the Architect generated (e.g., `350-tech-blazor.md`). | Adds new capabilities to the shared instruction library (`/brains`). |
| **3. Catalog Update** | Local Repo | Add the newly created files to the `context/999-architect-library-manifest.md`. | Informs the Architect that this new module is now reusable. |
| **4. Deployment Trigger** | GitHub UI | Commit changes and merge to `main` using **Squash and Merge**. | Triggers CI/CD to push the updated Brains/Manifest to Google Drive. |
| **5. Final Assembly** | Local Repo | **Zip the necessary Brain files** from the `/brains` folder (including `000`, `100`, `200`, and any `800` library files). | Creates the portable instruction package (`gem-brain-latest.zip`). |
| **6. Gem Creation** | New Gem Config | **Create the new Project Gem** by uploading the **Brain Zip** (`gem-brain-latest.zip`). | Provides the static **Rules and Core Libraries** for the session. |
| **7. Context Injection** | Gemini Chat | **Start a chat** with the new Gem, click **Add file** → **Import code** → **Enter the GitHub branch URL** of your project code. | Provides the dynamic **Data** (live codebase context) for the session. |

---

## 🛠️ Tooling & Architecture

### 1. Architect Prompt Template

When requesting a new Gem configuration from the Architect, use this structured input:

> I require a new Gem configuration for the following scenario:
>
> 1.  **Role (The "Who"):** [e.g., Senior C# Backend Developer]
> 2.  **Project Context (The "Goal"):** [Describe the specific business goal or application being built.]
> 3.  **Tech Stack & Skills (The "What"):** [List all required languages, frameworks, and tools. e.g., .NET 8, EF Core, Docker.]
> 4.  **Constraints & Standards:** [List any specific rules. e.g., "Must use Kebab-Case," "Use clean architecture."]

### 2. pack-context.py (Static Library Injection)

This script is used **only** for capturing the source code of **stable, internal libraries** (files in the 800-band).

* **Purpose:** To transform stable library code into a single `800-lib-*.md` file that is then included in the Brain Zip as permanent knowledge.
* **Usage:** `python pack-context.py <path_to_library_root> <output_filename.md>`
* **Placement:** The resulting `800-lib-*.md` file must be placed in the `/brains` directory.

---

## 🏗️ Contribution Workflow

1.  **Strict Naming:** All files in `/brains` MUST follow `[000-899]-[category]-[name].md` (Kebab-Case).
2.  **Deployment Trigger:** The `deploy-to-drive.yml` workflow runs ONLY after a Pull Request is merged into `main`.
3.  **CI Enforcement:** The `quality-check.yml` GitHub Action will block any PR that violates the naming, formatting, or structure rules defined in the system.
4.  **Merging:** Always select **Squash and Merge** to maintain a clean, linear history on the `main` branch.
