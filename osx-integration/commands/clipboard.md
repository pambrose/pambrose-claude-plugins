---
description: Read from or write to the macOS clipboard. Responds to "clipboard", "copy", "paste", "pbcopy", "pbpaste".
argument-hint: "[text to copy] or leave empty to read clipboard"
allowed-tools:
  - Bash
---

# Clipboard

Interact with the macOS system clipboard using `pbcopy` and `pbpaste`.

## Behavior

### If the user provides text to copy:

Write the provided text to the clipboard using `pbcopy` with a heredoc to avoid shell injection:

```bash
pbcopy <<'EOF'
TEXT_HERE
EOF
```

**Important:** Use a heredoc with single-quoted delimiter (`<<'EOF'`) so that `$`, `` ` ``, `\`, and `"` in the
user's text are passed through literally without shell interpretation.

After running, confirm to the user: "Copied to clipboard."

### If the user wants to read/paste the clipboard:

Read the current clipboard contents using `pbpaste`:

```bash
pbpaste
```

Display the clipboard contents to the user.

### If no argument is provided:

Ask the user whether they want to read or write the clipboard.
