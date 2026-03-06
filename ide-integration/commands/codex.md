---
description: Open Codex editor with the current directory as the project root. Responds to "open codex", "open cx".
---

# Open Codex

Open Codex editor using the current working directory as the project root.

Run the following command:

```bash
open -a "Codex" .
```

If that fails (e.g., the app name differs), try:

```bash
codex .
```

After running, confirm to the user: "Opened Codex in [current directory]."

If both commands fail, tell the user that Codex could not be found and suggest they verify it is installed.
