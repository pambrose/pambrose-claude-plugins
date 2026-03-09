---
description: Find a Linear issue and fix it in an isolated worktree
argument-hint: Issue ID (e.g. EO-42) or search text
---

# Fix Linear Issue

**Input:** $ARGUMENTS

## Step 1: Find the Issue

Determine if the input is an issue ID or search text:

- **Issue ID pattern** (matches `[A-Z]+-\d+`, e.g. `EO-42`): Use `mcp__claude_ai_Linear__get_issue` to fetch the issue
  directly by its identifier.
- **Search text** (anything else): First, ask the user which team and project to search within:
    1. Fetch teams and projects in parallel using `mcp__claude_ai_Linear__list_teams` and
       `mcp__claude_ai_Linear__list_projects`.
    2. Use a single `AskUserQuestion` call to ask:
        - **Team**: "Which team?" (list all teams). If only one team exists, auto-select it and skip this question.
        - **Project**: "Which project?" (list all projects). If only one project exists, auto-select it and skip this
          question.
    3. Search for issues matching the text, scoped to the selected team/project, with `limit: 10`. If multiple
       results, present them to the user via `AskUserQuestion` and ask which one to fix. If no results, tell the
       user and stop.

  **Important**: `AskUserQuestion` requires at least 2 options per question. If a question would have only 1 option,
  auto-select that option and omit it from the call.

## Step 2: Confirm with User

Display the issue details:

- **ID:** [issue identifier]
- **Title:** [issue title]
- **Status:** [current status]
- **Description:** [issue description, truncated if very long]

Ask the user: "Fix this issue?" with options Yes / No.

If No, stop.

## Step 3: Update Linear Status

Use `mcp__claude_ai_Linear__save_issue` to move the issue status to "In Progress".

If the status update fails (e.g., invalid state transition), warn the user but continue with the fix.

## Step 4: Spawn the Issue Fixer Agent

Use the `Task` tool with `isolation: "worktree"` and `subagent_type: "linear-integration:issue-fixer"` to spawn
the agent.
Pass the full issue context in the prompt:

```
Fix this Linear issue:

- **Issue ID:** [id]
- **Title:** [title]
- **Description:** [full description]
- **Labels:** [labels if any]
- **Priority:** [priority if any]

Work autonomously: explore the codebase, implement the fix, run tests and lint, commit the changes, and update the Linear issue with a comment summarizing what you did.
```

## Step 5: After Agent Completes

If the agent failed or encountered errors, report the failure to the user and use `mcp__claude_ai_Linear__save_issue`
to move the issue status back to "Todo". Then stop.

If the agent succeeded, report the results to the user. Then invoke the `finishing-a-development-branch` skill so the
user can choose how to integrate the work (merge, PR, or cleanup).
