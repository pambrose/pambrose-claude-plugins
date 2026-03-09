# Design: /fix-and-create-pr-linear-issue

## Overview

New command + agent for the linear-issue-integration plugin that finds a Linear issue, fixes it in an isolated worktree, creates tests, runs /simplify, commits, creates a GitHub PR, updates Linear, and cleans up.

## Architecture

Two new files:
- `commands/fix-and-create-pr-linear-issue.md` — orchestration command
- `agents/issue-pr-creator.md` — autonomous agent

## Command Flow

1. **Find issue** — ID or text search (same pattern as fix-linear-issue)
2. **Confirm with user** — show issue details, ask Yes/No. **Skip confirmation if issue is already "In Progress"** (supports /loop usage)
3. **Update Linear status** — move to "In Progress" (idempotent if already there)
4. **Spawn agent** — `issue-pr-creator` with `isolation: "worktree"`, pass full issue context
5. **On success:** clean up worktree via `git worktree remove`, update Linear status to "Done"
6. **On failure:** revert Linear status to "Todo", post error comment on the issue, clean up worktree

## Agent Flow

1. **Create branch** — `fix/<issue-id-lowercase>-<slugified-title>` (e.g., `fix/eo-42-fix-login-timeout`)
2. **Explore codebase** — Glob, Grep, Read, check CLAUDE.md
3. **Implement fix** — minimal, focused changes
4. **Create tests** — agent's judgment based on codebase conventions
5. **Run verification** — detect build system, run tests + lint (max 3 retries)
6. **Run /simplify** — invoke simplify skill to review and clean up changes
7. **Re-verify** — run tests again after simplify
8. **Post detailed summary** — write to Linear issue comments (files changed, root cause, test results)
9. **Commit** — `fix: <description> (<ISSUE-ID>)` conventional commit
10. **Push** — `git push -u origin <branch>`
11. **Create PR** — via `gh pr create` with structured body referencing Linear issue
12. **Update Linear** — post comment with PR link

## Branch Naming

Format: `fix/<issue-id-lowercase>-<slugified-title>`
Example: `fix/eo-42-fix-login-timeout`

## Error Handling

- On failure: keep the branch (preserve work), revert Linear to "Todo", post error to issue comments, clean up worktree
- Status update to "In Progress" is idempotent for /loop compatibility

## Loop Compatibility

When used via `/loop`, issues already marked "In Progress" skip the user confirmation step, enabling fully autonomous batch processing.
