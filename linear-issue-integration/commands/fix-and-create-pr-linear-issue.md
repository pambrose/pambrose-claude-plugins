---
description: Fix a Linear issue in a worktree, create tests, simplify, commit, and open a GitHub PR
argument-hint: Issue ID (e.g. EO-42) or search text
---

# Fix and Create PR for Linear Issue

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

## Step 2: Confirm with User (Conditional)

Check the issue's current status.

Display the issue details regardless of status:

- **ID:** [issue identifier]
- **Title:** [issue title]
- **Status:** [current status]

**If the issue is already "In Progress":** Skip confirmation and proceed directly to Step 3. The issue was pre-approved
for work (this supports autonomous `/loop` usage).

**If the issue is NOT "In Progress":** Also display:

- **ID:** [issue identifier]
- **Title:** [issue title]
- **Status:** [current status]
- **Description:** [issue description, truncated if very long]

Ask the user: "Fix this issue and create a PR?" with options Yes / No.

If No, stop.

## Step 3: Update Linear Status

Use `mcp__claude_ai_Linear__save_issue` to move the issue status to "In Progress".

If the issue is already "In Progress", this is a no-op — proceed without error.

If the status update fails for another reason (e.g., invalid state transition), warn the user but continue.

## Step 4: Spawn the Issue PR Creator Agent

Use the `Task` tool with `isolation: "worktree"` and `subagent_type: "linear-issue-integration:issue-pr-creator"` to
spawn the agent.

Pass the full issue context in the prompt:

```
Fix this Linear issue and create a PR:

- **Issue ID:** [id]
- **Title:** [title]
- **Description:** [full description]
- **Labels:** [labels if any]
- **Priority:** [priority if any]

Work autonomously: explore the codebase, implement the fix, create tests, run verification, run /simplify, re-verify,
post a detailed summary to the Linear issue comments, commit, push, create a GitHub PR, and post the PR link to the
Linear issue.
```

## Step 5: After Agent Completes

### On Success

1. Report the results to the user (PR URL, summary of changes).
2. Use `mcp__claude_ai_Linear__save_issue` to move the issue status to "Done".
3. Clean up the worktree: run `git worktree remove <worktree-path>` using the path returned by the agent. If cleanup
   fails, warn the user but do not treat it as an error.

### On Failure

1. Report the failure to the user.
2. Use `mcp__claude_ai_Linear__save_comment` to post an error comment on the issue:
   ```markdown
   ## Automated Fix Failed

   The autonomous fix attempt encountered an error:

   [error details]

   The branch may contain partial work. Manual intervention is needed.
   ```
3. Use `mcp__claude_ai_Linear__save_issue` to move the issue status back to "Todo".
4. Clean up the worktree: run `git worktree remove <worktree-path> --force`. If cleanup fails, warn the user.
