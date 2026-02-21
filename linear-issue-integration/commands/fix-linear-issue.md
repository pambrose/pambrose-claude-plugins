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
- **Search text** (anything else): Use `mcp__claude_ai_Linear__search_issues` to find issues matching the text. If
  multiple results, present them to the user via `AskUserQuestion` and ask which one to fix. If no results, tell the
  user and stop.

## Step 2: Confirm with User

Display the issue details:

- **ID:** [issue identifier]
- **Title:** [issue title]
- **Status:** [current status]
- **Description:** [issue description, truncated if very long]

Ask the user: "Fix this issue?" with options Yes / No.

If No, stop.

## Step 3: Update Linear Status

Use `mcp__claude_ai_Linear__update_issue` to move the issue status to "In Progress".

## Step 4: Spawn the Issue Fixer Agent

Use the `Task` tool with `isolation: "worktree"` and `subagent_type: "linear-issue-integration:issue-fixer"` to spawn
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

Report the agent's results to the user. Then invoke the `finishing-a-development-branch` skill so the user can choose
how to integrate the work (merge, PR, or cleanup).
