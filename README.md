# pambrose-claude-plugins

Personal [Claude Code](https://docs.anthropic.com/en/docs/claude-code) plugin marketplace.

## Installation

1. Add the marketplace (one-time setup):

    ```bash
    /plugin marketplace add pambrose/pambrose-claude-plugins
    ```

2. Install the plugin you want:

    ```bash
    /plugin install linear-issue-integration@pambrose-plugins
    ```

3. To update plugins after new changes are pushed:

    ```bash
    /plugin marketplace update pambrose-plugins
    ```

## Plugins

| Plugin                                                | Description                                                                                  |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------|
| [linear-issue-integration](linear-issue-integration/) | Linear integration: fix issues autonomously in worktrees, request Codex CLI reviews, and run systematic codebase reviews |
| [ide-integration](ide-integration/)                   | Open IDEs and apps from Claude Code                                                          |

---

### linear-issue-integration

Integrates Claude Code with [Linear](https://linear.app) for autonomous issue fixing, Codex CLI reviews, and systematic
codebase reviews that file findings directly as Linear issues.

**Requires:** Linear MCP integration configured in Claude Code.

#### Command: `/linear-plugin-version`

Show the installed version of the linear-issue-integration plugin.

```
/linear-plugin-version
```

#### Command: `/show-linear-issues`

Display all outstanding Linear issues for a selected project, sorted by priority.

```
/show-linear-issues
```

**What it does:**

1. Fetches the list of Linear projects
2. Asks which project to view
3. Displays all "Todo" issues in a table sorted by priority (Urgent, High, Medium, Low), showing title and assignee

#### Command: `/cancel-linear-issue`

Cancel one or more Linear issues by setting their status to Cancelled.

```
/cancel-linear-issue EO-42
/cancel-linear-issue EO-42 EO-43 EO-44
```

**What it does:**

1. Parses issue IDs from the input
2. Asks for confirmation before cancelling
3. Sets each issue's status to "Cancelled" and reports results

#### Command: `/fix-linear-issue`

Find a Linear issue and fix it autonomously in an isolated worktree.

```
/fix-linear-issue EO-42
/fix-linear-issue login timeout bug
```

**What it does:**

1. Looks up the issue by ID or searches Linear by text
2. Shows the issue details and asks for confirmation
3. Sets the Linear issue status to "In Progress"
4. Spawns the `issue-fixer` agent in an isolated git worktree
5. After the agent finishes, presents options to merge, create a PR, or clean up

#### Command: `/codex-review`

Ask Codex CLI (OpenAI's coding assistant) to independently review a Linear issue and post its feedback as a comment.

```
/codex-review EO-42
/codex-review login timeout bug
```

**Requires:** [Codex CLI](https://github.com/openai/codex) installed (`npm install -g @openai/codex`).

**What it does:**

1. Validates that Codex CLI is installed
2. Looks up the issue by ID or searches Linear by text
3. Shows the issue details and asks for confirmation
4. Runs Codex in read-only sandbox mode to analyze the codebase against the issue
5. Displays the review for your approval
6. Posts the review as a comment on the Linear issue

#### Agent: `issue-fixer`

Autonomous agent that runs in an isolated git worktree to fix a single Linear issue. Spawned by
the `/fix-linear-issue` command — not invoked directly.

**What it does:**

1. Records the worktree branch name
2. Reads and analyzes the issue
3. Explores the codebase (CLAUDE.md, relevant files)
4. Implements the fix with minimal, focused changes
5. Auto-detects the build system (Gradle, npm, Cargo, Go, Python, Make) and runs tests and lint
6. Asks whether to commit, commit and push, or skip
7. Posts a summary comment on the Linear issue (files changed, root cause, test results, branch name)

#### Skill: `create-linear-bug-issues`

Reviews the codebase for functional bugs and files each finding as a Linear issue.

**Trigger phrases:** "find bugs", "review for bugs", "find logic errors", "check for race conditions", "find resource
leaks", "audit error handling"

**What it looks for:** logic errors, race conditions, resource leaks, null safety issues, error handling gaps, incorrect
state management.

#### Skill: `create-linear-design-issues`

Reviews the codebase for design and architecture problems and files each finding as a Linear issue.

**Trigger phrases:** "review design", "find design problems", "check architecture", "find tight coupling", "find god
classes", "audit separation of concerns"

**What it looks for:** poor separation of concerns, tight coupling, missing abstractions, god classes, inappropriate
responsibility assignment, poor error propagation, concurrency model issues.

#### Skill: `create-linear-idiom-issues`

Reviews the codebase for non-idiomatic language usage and files each finding as a Linear issue. Automatically detects
the project language — not limited to any specific language.

**Trigger phrases:** "check for non-idiomatic code", "review idioms", "find non-idiomatic patterns", "find unidiomatic
code"

**What it looks for:** code written in the style of a different language, missing use of language-specific features,
imperative code that could be functional/declarative, verbose patterns where concise alternatives exist, underuse of
the type system, mutable state where immutable is idiomatic.

#### How the review skills work

All three review skills follow the same workflow:

1. **Gather Linear context** — fetch teams, projects, and users; ask which to use
2. **Ensure the label exists** — creates `bug`, `design`, or `non-idiomatic` if missing
3. **Dispatch a code explorer agent** for deep codebase analysis
4. **Present findings** in a summary table for review
5. **File approved findings** as Linear issues with structured descriptions, code snippets, and suggested fixes

---

### ide-integration

Open IDEs, terminals, and apps directly from Claude Code.

#### Editors

| Command    | Application         | Also responds to                 |
|------------|---------------------|----------------------------------|
| `/intellij`| IntelliJ IDEA       | "open ij", "open in intellij"   |
| `/pycharm` | PyCharm             | "open pycharm", "open pc"      |
| `/clion`   | CLion               | "open clion"                    |
| `/rustrover`| RustRover           | "open rustrover", "open rr"    |
| `/datagrip`| DataGrip            | "open datagrip", "open dg"     |
| `/cursor`  | Cursor              | "open cursor"                   |
| `/vscode`  | Visual Studio Code  | "open vscode", "open vsc"      |
| `/zed`     | Zed                 | "open zed"                      |
| `/codex`   | Codex               | "open codex"                    |
| `/claude`  | Claude              | "open claude", "open cc"        |

#### Terminals

| Command     | Application | Also responds to  |
|-------------|-------------|--------------------|
| `/ghostty`  | Ghostty     | "open ghostty", "open tty"  |
| `/warp`     | Warp        | "open warp"       |
| `/terminal` | Terminal    | "open terminal"   |

#### Utilities

| Command   | Application  | Also responds to  |
|-----------|--------------|--------------------|
| `/finder` | macOS Finder | "open finder"     |
