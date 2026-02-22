---
description: Display all outstanding Linear issues sorted by priority
---

# Show Issues

Display all outstanding Linear issues for a selected project, sorted by priority.

## Step 1: Get Project List

Fetch available projects using `mcp__claude_ai_Linear__list_projects`.

## Step 2: Ask Which Project

Use `AskUserQuestion` to ask the user: "Which project?" with the list of project names as options.

## Step 3: Fetch Issues

Use `mcp__claude_ai_Linear__list_issues` to fetch all open issues for the selected project. Filter to only issues that
are not completed or cancelled.

## Step 4: Display Results

Sort the issues by priority (1 = Urgent first, then 2 = High, 3 = Medium, 4 = Low, 0 = No priority last).

Present the results as a markdown table:

| Priority | Title | Assignee |
|----------|-------|----------|
| Urgent | Fix login timeout | Alice |
| High | Add retry logic | Bob |
| Medium | Refactor auth module | Unassigned |

Use the priority labels:
- 1 = Urgent
- 2 = High
- 3 = Medium
- 4 = Low
- 0 = No priority

If there are no open issues, tell the user: "No outstanding issues for [project name]."
