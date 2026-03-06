---
name: create-linear-idiom-issues
description: This skill should be used when the user asks to "check for non-idiomatic code", "review idioms", "find non-idiomatic patterns", "check idiomatic usage", "review code style", "find Java-style Kotlin", "find unidiomatic code", or wants a systematic codebase review focused on non-idiomatic language usage that files findings as Linear issues.
---

# Review Idioms

Review the codebase for non-idiomatic language usage, then file each finding as a Linear issue.

## Overview

This skill dispatches a deep codebase analysis focused exclusively on non-idiomatic code — patterns that work but don't
follow the conventions and idioms of the language being used. Each finding is documented with code snippets showing both
the non-idiomatic pattern and the idiomatic alternative, then filed as a Linear issue.

## Workflow

### Phase 1: Gather Linear Context

Before reviewing code, collect the Linear team, project, and user information.

1. **Fetch all data in parallel** using `mcp__claude_ai_Linear__list_teams`, `mcp__claude_ai_Linear__list_projects`,
   `mcp__claude_ai_Linear__list_users`, and `mcp__claude_ai_Linear__list_issue_labels`.
2. **Ask the user all three questions in a single `AskUserQuestion` call** with these questions:
    - **Team**: Which team should the issues be created under? (list all teams from step 1). If only one team exists,
      auto-select it and skip this question.
    - **Project**: Which project should the issues be assigned to? (list all projects; if results can be filtered by
      team, do so after team is known).
    - **Assignee**: Who should the issues be assigned to? (list all users for the team, plus a "No assignee" option).

   **Important**: `AskUserQuestion` requires at least 2 options per question. If a question would have only 1 option,
   auto-select that option and omit it from the `AskUserQuestion` call. If all questions can be auto-selected, skip the
   `AskUserQuestion` call entirely and inform the user of the auto-selected values.
3. **Check existing labels** from the fetched label data for the chosen team. If a label named `non-idiomatic` does not
   already exist, create it using `mcp__claude_ai_Linear__create_issue_label`:
    - `non-idiomatic` (color: `#3498db`) — Non-idiomatic language usage

### Phase 2: Detect the Project Language

Before dispatching the review agent, identify the primary language(s) used in the codebase by examining file extensions,
build files, and project configuration (e.g., `build.gradle.kts`, `package.json`, `Cargo.toml`, `go.mod`,
`pyproject.toml`). This determines what idiomatic patterns to check for.

### Phase 3: Codebase Idiom Review

Dispatch a subagent to review the codebase. Use the `feature-dev:code-explorer` subagent type for deep analysis.

**Idiomatic Code Reviewer Agent:**
Review all source files for non-idiomatic usage of the detected language(s). Look for patterns such as:

- Code written in the style of a different language (e.g., Java patterns in Kotlin, C patterns in Go, JavaScript
  patterns in TypeScript)
- Missing use of language-specific features, constructs, or standard library functions
- Imperative code that could leverage the language's functional or declarative capabilities
- Verbose patterns where the language provides concise alternatives
- Not leveraging the type system effectively (e.g., missing sealed types, union types, enums where appropriate)
- Mutable state where immutable alternatives are idiomatic
- Missing use of the language's concurrency primitives or async patterns
- Verbose error handling where the language provides cleaner idioms

For each finding, produce:

- Title (concise summary)
- Language (which language the finding applies to)
- Description (what the non-idiomatic pattern is)
- Impact (readability, maintainability, or performance consequences)
- Importance (Critical / High / Medium / Low)
- Code snippet showing the non-idiomatic pattern (with file path and line numbers)
- Code snippet showing the idiomatic alternative

### Phase 4: Present Findings to User

After the agent completes:

1. **Compile all findings** into a summary table.
2. **Present the summary** to the user showing: title, language, importance, and file location for each finding.
3. **Ask for confirmation** before creating Linear issues: "I found N non-idiomatic patterns. Create all as Linear
   issues?"
4. Allow the user to exclude specific findings before filing.

### Phase 5: File Linear Issues

For each approved finding, create a Linear issue using `mcp__claude_ai_Linear__save_issue` with:

```
title: <concise finding title>
team: <user-selected team>
project: <user-selected project>
assignee: <user-selected assignee, or omit if "No assignee" was chosen>
labels: ["non-idiomatic"]
priority: <mapped from importance: Critical=1, High=2, Medium=3, Low=4>
state: Todo
description: |
  ## Description
  <what the non-idiomatic pattern is and context>

  ## Language
  <language name>

  ## Impact
  <readability, maintainability, or performance consequences>

  ## Importance
  <Critical | High | Medium | Low> - <brief justification>

  ## Non-Idiomatic Code
  **File:** `<file_path>`
  ```

  <code snippet showing the non-idiomatic pattern>
  ```

## Idiomatic Alternative

  ```
  <code snippet showing the idiomatic version>
  ```

```

After all issues are created, present a final summary with the Linear issue identifiers and links.

## Finding Quality Standards

Each finding must meet these criteria before filing:

- **Specific**: Point to exact code, not vague observations.
- **Actionable**: Include the idiomatic alternative, not just a complaint.
- **Impactful**: Explain real consequences for readability or maintainability.
- **Non-trivial**: Skip pure style preferences (formatting, naming conventions with no clarity impact). Focus on patterns where the idiomatic version is meaningfully clearer, safer, or more maintainable.

## Priority Mapping

| Importance | Linear Priority | Criteria |
|------------|----------------|----------|
| Critical | 1 (Urgent) | Non-idiomatic pattern that introduces subtle bugs or safety issues |
| High | 2 (High) | Pattern that significantly harms readability or misses key language safety features |
| Medium | 3 (Normal) | Verbose or unclear code where idiomatic alternative is substantially better |
| Low | 4 (Low) | Minor idiom improvements, stylistic modernization |
