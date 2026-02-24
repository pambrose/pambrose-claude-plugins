---
description: Open Zed editor with the current directory as the project root. Responds to "open zed".
---

# Open Zed

Open Zed editor using the current working directory as the project root.

Run the following command:

```bash
zed .
```

After running, confirm to the user: "Opened Zed in [current directory]."

If the command fails, tell the user that Zed could not be found and suggest they verify it is installed
or that the `zed` command-line launcher is configured (Zed > Command Palette > "Install CLI").
