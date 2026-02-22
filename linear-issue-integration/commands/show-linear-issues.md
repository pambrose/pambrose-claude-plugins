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

**CRITICAL:** You MUST pass these exact parameters to `mcp__claude_ai_Linear__list_issues`:

```json
{
  "project": "<selected project name>",
  "state": "Todo",
  "includeArchived": false,
  "limit": 50
}
```

Do NOT omit the `state` parameter. Do NOT set `limit` higher than 50. Omitting `state` will fetch all issues
including completed ones and cause a token overflow error.

If the response includes a cursor, fetch additional pages until all Todo issues are retrieved.

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
