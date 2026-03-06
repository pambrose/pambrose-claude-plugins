---
name: create-linear-bug-issues
description: This skill should be used when the user asks to "find bugs", "review for bugs", "find logic errors", "check for race conditions", "find resource leaks", "audit error handling", "find null safety issues", or wants a systematic codebase review focused on functional bugs that files findings as Linear issues.
---

# Review Bugs

Review the codebase for functional bugs, then file each finding as a Linear issue.

## Overview

This skill dispatches a deep codebase analysis focused exclusively on bugs — logic errors, race conditions, resource
leaks, null safety issues, error handling gaps, and incorrect state management. Each finding is documented with code
snippets and filed as a Linear issue.

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
3. **Check existing labels** from the fetched label data for the chosen team. If a label named `bug` does not already
   exist, create it using `mcp__claude_ai_Linear__create_issue_label`:
    - `bug` (color: `#e74c3c`) — Functional bugs or logic errors

### Phase 2: Codebase Bug Review

Dispatch a subagent to review the codebase. Use the `feature-dev:code-explorer` subagent type for deep analysis.

**Bug Finder Agent:**
Review all source files for functional bugs, logic errors, race conditions, resource leaks, null safety issues, error
handling gaps, and incorrect state management. For each finding, produce:

- Title (concise summary)
- Description (what the bug is and when it manifests)
- Impact (what goes wrong if unfixed)
- Importance (Critical / High / Medium / Low)
- Code snippet showing the problem (with file path and line numbers)
- Code snippet showing the suggested fix

### Phase 3: Present Findings to User

After the agent completes:

1. **Compile all findings** into a summary table.
2. **Present the summary** to the user showing: title, importance, and file location for each finding.
3. **Ask for confirmation** before creating Linear issues: "I found N bugs. Create all as Linear issues?"
4. Allow the user to exclude specific findings before filing.

### Phase 4: File Linear Issues

For each approved finding, create a Linear issue using `mcp__claude_ai_Linear__save_issue` with:

```
title: <concise finding title>
team: <user-selected team>
project: <user-selected project>
assignee: <user-selected assignee, or omit if "No assignee" was chosen>
labels: ["bug"]
priority: <mapped from importance: Critical=1, High=2, Medium=3, Low=4>
state: Todo
description: |
  ## Description
  <what the issue is and context>

  ## Impact
  <what goes wrong if left unfixed>

  ## Importance
  <Critical | High | Medium | Low> - <brief justification>

  ## Problem Code
  **File:** `<file_path>`
  ```

  <code snippet showing the problem>
  ```

## Suggested Fix

  ```
  <code snippet showing the corrected code>
  ```

```

After all issues are created, present a final summary with the Linear issue identifiers and links.

## Finding Quality Standards

Each finding must meet these criteria before filing:

- **Specific**: Point to exact code, not vague observations.
- **Actionable**: Include a concrete fix, not just a complaint.
- **Impactful**: Explain real consequences, not theoretical purity concerns.
- **Non-trivial**: Skip style-only nits. Focus on findings that affect correctness or reliability.

## Priority Mapping

| Importance | Linear Priority | Criteria |
|------------|----------------|----------|
| Critical | 1 (Urgent) | Data loss, security flaw, crash in production path |
| High | 2 (High) | Incorrect behavior under common conditions, race condition |
| Medium | 3 (Normal) | Edge-case bugs, minor resource leaks |
| Low | 4 (Low) | Defensive coding gaps unlikely to trigger in practice |
