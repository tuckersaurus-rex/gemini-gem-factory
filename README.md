# Gemini Gem Configuration

## Overview

This repository manages the source code for Gemini Gem "Knowledge Files." It utilizes a modular architecture known as the **Century Protocol**, which enforces a strict 3-digit numeric hierarchy to manage instruction priority.

## The Century Protocol Structure

- **000-099 (Core):** Kernel instructions and safety protocols.
- **100-199 (Role):** Persona definitions (e.g., Architect, Developer).
- **200-299 (Standards):** Operational standards (e.g., Module writing rules).
- **300-899 (Tech):** Hard skills and frameworks (C#, Blazor, Python).
- **900-999 (Project):** Business logic and specific project constraints.

## Usage

All files are maintained in **Kebab-Case** and intended to be uploaded directly to the "Knowledge" section of a Gemini Gem.

## 🏗️ How to Prompt the Architect

When requesting a new Gem configuration from the **Gem Architect**, use the following template to ensure it correctly maps requirements to the 000-999 file structure:

> **I require a new Gem configuration for the following scenario:**
>
> **1. Role (The "Who"):** > [e.g., Senior C# Backend Developer, Technical Writer, SEO Specialist]
>
> **2. Project Context (The "Goal"):** > [Describe the specific project, business goal, or application being built. e.g., "A Blazor Web App for inventory management."]
>
> **3. Tech Stack & Skills (The "What"):** > [List all languages, frameworks, and tools. e.g., .NET 8, EF Core, Docker, Azure DevOps.]
>
> **4. Constraints & Standards:** > [List any specific rules. e.g., "Must use Kebab-Case for files," "Use clean architecture," "No external libraries."]

**Note:** The Architect will automatically check the `999-architect-library-manifest` and reuse existing modules where possible.

## 🔄 Contribution Workflow

This repository follows a strict **Feature Branch** workflow. Direct commits to `main` are discouraged.

1.  **Sync:** Ensure your local main is up to date (`git checkout main && git pull`).
2.  **Branch:** Create a feature branch (`git checkout -b feature/new-module-name`).
3.  **Work:** Add your new `.md` knowledge files.
4.  **Commit:** Stage and commit your changes.
5.  **Push:** Push the branch to GitHub (`git push -u origin feature/new-module-name`).
6.  **PR:** Open a Pull Request on GitHub to merge into `main`.

## 📂 Repository Structure

/
├── /Example Project # Put any project related knowledge files (non-century protocol names)
├── .gitignore # Ignores Google Drive system files
├── README.md # Documentation
├── 000-core-boilerplate.md
├── 100-role-gem-architect.md
├── 200-tech-module-standards.md
├── ... # Other knowledge files
├── 999-architect-library-manifest.md
└── sytem-kernal.me # What to put in the Gem instructions
