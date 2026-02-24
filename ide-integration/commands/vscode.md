---
description: Open Visual Studio Code with the current directory as the project root. Responds to "open vscode", "open vs code".
---

# Open VS Code

Open Visual Studio Code using the current working directory as the project root.

Run the following command:

```bash
code .
```

After running, confirm to the user: "Opened VS Code in [current directory]."

If the command fails, tell the user that VS Code could not be found and suggest they verify it is installed
or that the `code` command-line launcher is configured (VS Code > Command Palette > "Shell Command: Install 'code' command in PATH").
