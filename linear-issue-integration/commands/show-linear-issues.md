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

Use `mcp__claude_ai_Linear__list_issues` with these parameters:

```json
{
  "project": "<selected project name>",
  "state": "<exact Todo state name or ID from step 3>",
  "includeArchived": false,
  "limit": 50
}
```

Do NOT set `limit` higher than 50 to avoid token overflow errors.

If the response includes a cursor, fetch additional pages until all Todo issues are retrieved.

**After fetching:** As a safety check, discard any issues whose status is not "Todo". Only keep issues where the
state name exactly matches "Todo".

## Step 5: Display Results

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

If there are no Todo issues, tell the user: "No outstanding issues for [project name]."
