---
description: Cancel one or more Linear issues by setting their status to Cancelled
argument-hint: One or more issue IDs (e.g. EO-42 EO-43 EO-44)
---

# Cancel Linear Issues

**Input:** $ARGUMENTS

## Step 1: Parse Issue IDs

Extract all issue IDs from the input. Issue IDs match the pattern `[A-Z]+-\d+` (e.g., `EO-42`).

If no valid issue IDs are found, tell the user and stop.

## Step 2: Confirm with User

Display the list of issue IDs that will be cancelled.

Ask the user using `AskUserQuestion`: "Cancel these issues?" with options Yes / No.

If No, stop.

## Step 3: Cancel Issues

For each issue ID, use `mcp__claude_ai_Linear__update_issue` with `state: "Cancelled"`.

If any update fails, report the error for that issue and continue with the remaining issues.

## Step 4: Report Results

Present a summary of what was cancelled:

| Issue | Status |
|-------|--------|
| EO-42 | Cancelled |
| EO-43 | Cancelled |
| EO-44 | Error: issue not found |
