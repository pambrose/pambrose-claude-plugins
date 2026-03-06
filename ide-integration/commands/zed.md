---
description: Open Zed editor with the current directory as the project root. Responds to "open zed", "open z".
---

# Open Zed

Open Zed editor using the current working directory as the project root.

Run the following command:

```bash
open -a "Zed" .
```

If that fails (e.g., the app name differs), try:

```bash
zed .
```

After running, confirm to the user: "Opened Zed in [current directory]."

If both commands fail, tell the user that Zed could not be found and suggest they verify it is installed
or that the `zed` command-line launcher is configured (Zed > Command Palette > "Install CLI").
