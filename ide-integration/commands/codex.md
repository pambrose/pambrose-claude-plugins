---
description: Open Codex editor with the current directory as the project root. Responds to "open codex".
---

# Open Codex

Open Codex editor using the current working directory as the project root.

Run the following command:

```bash
open -a Codex "$(pwd)"
```

After running, confirm to the user: "Opened Codex in [current directory]."

If the command fails, tell the user that Codex could not be found and suggest they verify it is installed.
