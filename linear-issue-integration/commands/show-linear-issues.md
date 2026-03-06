---
description: Display all Linear issues in "Todo" status sorted by priority
---

# Show Issues

Display all Linear issues in "Todo" status for a selected project, sorted by priority.

## Step 1: Get Teams and Projects

Fetch the following in parallel:
- Available teams using `mcp__claude_ai_Linear__list_teams`
- Available projects using `mcp__claude_ai_Linear__list_projects`

## Step 2: Ask Which Team and Project

Use a single `AskUserQuestion` call to ask both questions:

- **Team**: "Which team?" with the list of team names as options. If only one team exists, auto-select it and skip
  this question.
- **Project**: "Which project?" with the list of project names as options. If only one project exists, auto-select it
  and skip this question.

**Important**: `AskUserQuestion` requires at least 2 options per question. If a question would have only 1 option,
auto-select that option and omit it from the call. If all questions can be auto-selected, skip the call entirely and
inform the user of the auto-selected values.

## Step 3: Look Up the "Todo" State

Use `mcp__claude_ai_Linear__list_issue_statuses` with the selected team to get all available statuses.
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
- `labels` (array of label names, but exclude the "AI" label from the list)

Store the extracted data, then fetch the next page using the `cursor` if `hasNextPage` is true. When fetching
multiple pages, inform the user of progress (e.g., "Fetched 20 issues so far..."). Repeat until all pages
are fetched.

## Step 5: Display Results

Sort the collected issues by priority (1 = Urgent first, then 2 = High, 3 = Medium, 4 = Low, 0 = No priority last).

Present the results as a markdown table:

| ID | Priority | Title | Labels | Assignee |
|----|----------|-------|--------|----------|
| EO-42 | Urgent | Fix login timeout | Bug | Alice |
| EO-43 | High | Add retry logic | Bug, Design | Bob |
| EO-44 | Medium | Refactor auth module | Non-Idiom | Unassigned |

Use the priority labels:
- 1 = Urgent
- 2 = High
- 3 = Medium
- 4 = Low
- 0 = No priority

If there are no Todo issues, tell the user: "No Todo issues for [project name]."
