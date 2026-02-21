# pambrose-claude-plugins

Personal [Claude Code](https://docs.anthropic.com/en/docs/claude-code) plugin marketplace.

## Plugins

| Plugin                                                | Description                                                                                  |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------|
| [linear-issue-integration](linear-issue-integration/) | Linear integration: fix issues autonomously in worktrees and run systematic codebase reviews |

### linear-issue-integration

**Command:**

- `/fix-linear-issue` — Find a Linear issue by ID or search text and fix it autonomously in an isolated worktree

**Skills:**

- `create-linear-bug-issues` — Review the codebase for bugs and file findings as Linear issues
- `create-linear-design-issues` — Review the codebase for design/architecture problems and file findings as Linear
  issues
- `create-linear-idiom-issues` — Review the codebase for non-idiomatic language usage and file findings as Linear issues

**Agent:**

- `issue-fixer` — Autonomous agent that implements fixes, runs verification, commits, and updates Linear

## Installation

Add the marketplace:

```bash
/plugin marketplace add pambrose/pambrose-claude-plugins
```

Install a plugin:

```bash
/plugin install linear-issue-integration@pambrose-plugins
```

## Adding a New Plugin

1. Create a directory at the repo root:

    ```
    my-plugin/
    ├── .claude-plugin/
    │   └── plugin.json
    ├── commands/       # Slash commands (.md files)
    ├── agents/         # Subagent definitions (.md files)
    ├── skills/         # Auto-activating skills (subdirs with SKILL.md)
    └── hooks/          # Event handlers (hooks.json)
    ```

2. At minimum, `plugin.json` needs:

    ```json
    {
      "name": "my-plugin"
    }
    ```

3. Add an entry to `.claude-plugin/marketplace.json`:

    ```json
    {
      "name": "my-plugin",
      "source": "./my-plugin",
      "description": "What the plugin does"
    }
    ```

4. Push to GitHub and update the marketplace:

    ```bash
    /plugin marketplace update pambrose-plugins
    ```
