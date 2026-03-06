---
name: create-linear-design-issues
description: This skill should be used when the user asks to "review design", "find design problems", "check architecture", "find tight coupling", "find god classes", "audit separation of concerns", "review code structure", or wants a systematic codebase review focused on design and architecture issues that files findings as Linear issues.
---

# Review Design

Review the codebase for design and architecture problems, then file each finding as a Linear issue.

## Overview

This skill dispatches a deep codebase analysis focused exclusively on design issues — poor separation of concerns, tight
coupling, missing abstractions, god classes, inappropriate responsibility assignment, poor error propagation patterns,
and concurrency model issues. Each finding is documented with code snippets and filed as a Linear issue.

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
3. **Check existing labels** from the fetched label data for the chosen team. If a label named `design` does not already
   exist, create it using `mcp__claude_ai_Linear__create_issue_label`:
    - `design` (color: `#f39c12`) — Architectural or design problems

### Phase 2: Codebase Design Review

Dispatch a subagent to review the codebase. Use the `feature-dev:code-explorer` subagent type for deep analysis.

**Design Reviewer Agent:**
Review all source files for design problems: poor separation of concerns, tight coupling, missing abstractions, god
classes, inappropriate responsibility assignment, poor error propagation patterns, and concurrency model issues. For
each finding, produce:

- Title (concise summary)
- Description (what the design problem is and how it manifests)
- Impact (what goes wrong if unfixed — maintainability, extensibility, testability)
- Importance (Critical / High / Medium / Low)
- Code snippet showing the problem (with file path and line numbers)
- Code snippet showing the suggested fix or restructuring

### Phase 3: Present Findings to User

After the agent completes:

1. **Compile all findings** into a summary table.
2. **Present the summary** to the user showing: title, importance, and file location for each finding.
3. **Ask for confirmation** before creating Linear issues: "I found N design issues. Create all as Linear issues?"
4. Allow the user to exclude specific findings before filing.

### Phase 4: File Linear Issues

For each approved finding, create a Linear issue using `mcp__claude_ai_Linear__save_issue` with:

```
title: <concise finding title>
team: <user-selected team>
project: <user-selected project>
assignee: <user-selected assignee, or omit if "No assignee" was chosen>
labels: ["design"]
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
  <code snippet showing the corrected code or restructuring>
  ```

```

After all issues are created, present a final summary with the Linear issue identifiers and links.

## Finding Quality Standards

Each finding must meet these criteria before filing:

- **Specific**: Point to exact code, not vague observations.
- **Actionable**: Include a concrete fix or restructuring, not just a complaint.
- **Impactful**: Explain real consequences for maintainability, extensibility, or testability.
- **Non-trivial**: Skip minor style preferences. Focus on findings that meaningfully affect the codebase's long-term health.

## Priority Mapping

| Importance | Linear Priority | Criteria |
|------------|----------------|----------|
| Critical | 1 (Urgent) | Architectural flaw blocking feature development or causing cascading failures |
| High | 2 (High) | Tight coupling or missing abstraction causing widespread code duplication |
| Medium | 3 (Normal) | Suboptimal design that hampers maintainability or extensibility |
| Low | 4 (Low) | Minor structural improvements, better responsibility assignment |
