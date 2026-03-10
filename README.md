# pambrose-claude-plugins

Basic [Claude Code](https://docs.anthropic.com/en/docs/claude-code) plugin marketplace.

## Installation

1. Add the marketplace (one-time setup):

    ```bash
    /plugin marketplace add pambrose/pambrose-claude-plugins
    ```

2. Install the plugin you want:

    ```bash
    /plugin install linear-integration@pambrose-plugins
    /plugin install ide-integration@pambrose-plugins
    /plugin install osx-integration@pambrose-plugins
    ```

3. To update plugins after new changes are pushed:

    ```bash
    /plugin marketplace update pambrose-plugins
    ```

## Plugins

| Plugin | Description |
|--------|-------------|
| [linear-integration](linear-integration/README.md) | Linear integration: discover codebase issues, triage by priority, review with Codex CLI, fix autonomously in worktrees, create PRs, and cancel issues |
| [ide-integration](ide-integration/README.md) | Open IDEs, editors, terminals, and tools from Claude Code |
| [osx-integration](osx-integration/README.md) | macOS system integration — clipboard, notifications, text-to-speech, screenshots, and phone dialing |
