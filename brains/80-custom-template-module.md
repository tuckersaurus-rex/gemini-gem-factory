# 80 - CUSTOM: MODULE TEMPLATE

## 1. TEMPLATE USAGE

This file serves as the **Source of Truth** for generating Competency Modules (Sectors 30-80).

* **Naming Convention:** `[SectorID]-[Abbr]-[Name].md` (strictly-lowercase-kebab-case).
* **Directive Standard:** All instructional text MUST be in imperative, absolute language ("You must", "Forbidden", "Always").

### Sector Map (v3.0)

* **30** `env` (Runtime / Infrastructure)
* **40** `skill` (Languages / Syntax)
* **50** `fwk` (Backend Frameworks)
* **60** `ui` (Frontend Interface)
* **70** `lib` (External Libraries)
* **80** `custom` (Catalogs / Templates)

## 2. THE TEMPLATE CONTENT

*(Copy the block below exactly when generating output)*

```markdown
# [SectorID] - [ABBR]: [TOPIC NAME - Use Title Case]

## 1. [MAJOR SECTION TITLE - Use Title Case]

* **[Directive/Rule Name]:** [Instructional content using absolute language.]
* **[Directive/Rule Name]:** [Instructional content using absolute language.]

## 2. [MAJOR SECTION TITLE - Use Title Case]

* **[Directive/Rule Name]:** [Instructional content using absolute language.]
* **[Directive/Rule Name]:** [Instructional content using absolute language.]
* **[Constraint]:** [Specify a key limiting factor or non-negotiable rule.]

## 3. TECHNICAL STANDARDS

*(Specific implementation details, syntax preferences, formatting rules, or advanced configuration strategies)*
* **Syntax:** [e.g., "Use File-Scoped Namespaces."]
* **Formatting:** [e.g., "All JSON must be minified."]
```
