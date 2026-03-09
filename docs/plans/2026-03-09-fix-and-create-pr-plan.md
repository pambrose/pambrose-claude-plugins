# /fix-and-create-pr-linear-issue Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Add a command that autonomously fixes a Linear issue, creates tests, simplifies code, commits, creates a GitHub PR, and updates Linear.

**Architecture:** New command (`fix-and-create-pr-linear-issue.md`) orchestrates issue lookup, confirmation, and worktree cleanup. New agent (`issue-pr-creator.md`) runs in an isolated worktree to implement the fix, tests, simplify, commit, push, and PR creation.

**Tech Stack:** Claude Code plugin system (markdown commands/agents), Linear MCP, GitHub CLI (`gh`), Git worktrees

---

### Task 1: Create the agent — `issue-pr-creator.md`

**Files:**
- Create: `linear-issue-integration/agents/issue-pr-creator.md`

**Reference:** Model after `linear-issue-integration/agents/issue-fixer.md` but with these key differences:
- Creates branch named `fix/<issue-id-lowercase>-<slugified-title>` at start
- Explicitly creates/updates tests (agent judgment on placement)
- Invokes `/simplify` skill after verification
- Re-verifies after simplify
- Posts detailed summary to Linear issue comments before committing
- Creates GitHub PR via `gh pr create`
- Posts PR link to Linear issue comments
- No user confirmation for commit/push (fully autonomous)

**Step 1:** Write the agent markdown file with all sections.

**Step 2:** Verify the file is valid markdown with proper frontmatter.

**Commit:** `feat: add issue-pr-creator agent for autonomous PR workflow`

---

### Task 2: Create the command — `fix-and-create-pr-linear-issue.md`

**Files:**
- Create: `linear-issue-integration/commands/fix-and-create-pr-linear-issue.md`

**Reference:** Model after `linear-issue-integration/commands/fix-linear-issue.md` but with these key differences:
- Skip user confirmation if issue status is already "In Progress"
- Spawn `issue-pr-creator` agent instead of `issue-fixer`
- On success: update Linear status to "Done", clean up worktree
- On failure: revert Linear status to "Todo", post error comment to issue, clean up worktree

**Step 1:** Write the command markdown file with all steps.

**Step 2:** Verify the file is valid markdown with proper frontmatter.

**Commit:** `feat: add /fix-and-create-pr-linear-issue command`

---

### Task 3: Update plugin metadata

**Files:**
- Modify: `linear-issue-integration/.claude-plugin/plugin.json` — bump version to `1.8.0`
- Modify: `.claude-plugin/marketplace.json` — bump linear-issue-integration version to `1.8.0`

**Step 1:** Update both version numbers.

**Commit:** `chore: bump linear-issue-integration to v1.8.0`

---

### Task 4: Update README and llms.txt

**Files:**
- Modify: `linear-issue-integration/README.md` — add command, agent, and usage example
- Modify: `linear-issue-integration/llms.txt` — add command and agent descriptions

**Step 1:** Add `/fix-and-create-pr-linear-issue` to the commands table in README.
**Step 2:** Add `issue-pr-creator` to the agents table in README.
**Step 3:** Add usage example.
**Step 4:** Update llms.txt with new command and agent.

**Commit:** `docs: add fix-and-create-pr-linear-issue to README and llms.txt`
