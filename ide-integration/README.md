# ide-integration

Open IDEs, editors, terminals, and tools directly from Claude Code on macOS.

## Commands

### Editors & IDEs

| Command | Aliases | Description |
|---------|---------|-------------|
| `/intellij` | "open ij", "open in intellij" | Open IntelliJ IDEA |
| `/pycharm` | "open pycharm", "open pc" | Open PyCharm |
| `/clion` | "open clion" | Open CLion |
| `/rustrover` | "open rustrover", "open rr" | Open RustRover |
| `/datagrip` | "open datagrip", "open dg" | Open DataGrip |
| `/vscode` | "open vscode", "open vs code", "open vsc" | Open Visual Studio Code |
| `/cursor` | "open cursor", "open cur" | Open Cursor |
| `/zed` | "open zed", "open z" | Open Zed |
| `/codex` | "open codex", "open cx" | Open Codex |

### Terminals

| Command | Aliases | Description |
|---------|---------|-------------|
| `/ghostty` | "open ghostty", "open tty" | Open Ghostty terminal |
| `/warp` | "open warp", "open w" | Open Warp terminal |
| `/terminal` | "open terminal", "open term" | Open macOS Terminal |

### Other

| Command | Aliases | Description |
|---------|---------|-------------|
| `/claude` | "open claude", "open cc" | Open Claude desktop app |
| `/finder` | "open finder" | Open macOS Finder |
| `/open-github` | "open github", "open gh" | Open current repo on GitHub |
| `/open-gitlab` | "open gitlab", "open gl" | Open current repo on GitLab |

## Usage Examples

```
/vscode                  # Open VS Code in current directory
/intellij                # Open IntelliJ IDEA in current directory
/ghostty                 # Open Ghostty terminal in current directory
/finder                  # Open Finder in current directory
/open-github             # Open current repo on GitHub in browser
```

## Requirements

- macOS (uses `open -a` for app launching)
- For `/open-github`: GitHub CLI (`brew install gh`)
- For `/open-gitlab`: GitLab CLI (`brew install glab`)
