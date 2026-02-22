---
description: Display all outstanding Linear issues sorted by priority
---

# Show Issues

Display all outstanding Linear issues for a selected project, sorted by priority.

## Step 1: Get Project and Status Info

Fetch the following in parallel:
- Available projects using `mcp__claude_ai_Linear__list_projects`
- Available teams using `mcp__claude_ai_Linear__list_teams`

## Step 2: Ask Which Project

Use `AskUserQuestion` to ask the user: "Which project?" with the list of project names as options.

## Step 3: Look Up the "Todo" State

Use `mcp__claude_ai_Linear__list_issue_statuses` with the relevant team to get all available statuses.
Find the status whose name is "Todo" and note its exact name and ID.

## Step 4: Fetch Issues

Fetch issues in small batches to avoid context overflow. Use `limit: 10`:

```json
{
  "project": "<selected project name>",
  "state": "<exact Todo state name or ID from step 3>",
  "includeArchived": false,
  "limit": 10
}
```

From each batch, immediately extract ONLY these fields per issue and discard everything else (especially
the `description` field, which is very large):
- `identifier` (e.g., "EO-42")
- `title`
- `priority.name` (e.g., "High")
- `assignee` (e.g., "Alice" or null)

Store the extracted data, then fetch the next page using the `cursor` if `hasNextPage` is true. Repeat until
all pages are fetched.

## Step 5: Display Results

Sort the collected issues by priority (1 = Urgent first, then 2 = High, 3 = Medium, 4 = Low, 0 = No priority last).

Present the results as a markdown table:

| ID | Priority | Title | Assignee |
|----|----------|-------|----------|
| EO-42 | Urgent | Fix login timeout | Alice |
| EO-43 | High | Add retry logic | Bob |
| EO-44 | Medium | Refactor auth module | Unassigned |

Use the priority labels:
- 1 = Urgent
- 2 = High
- 3 = Medium
- 4 = Low
- 0 = No priority

If there are no Todo issues, tell the user: "No outstanding issues for [project name]."
