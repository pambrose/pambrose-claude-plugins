---
description: Open Ghostty terminal. Responds to "open ghostty", "open tty".
---

# Open Ghostty

Open Ghostty terminal in the current working directory.

Run the following command:

```bash
open -na Ghostty.app --args --working-directory="$(pwd)"
```

After running, confirm to the user: "Opened Ghostty in [current directory]."

If the command fails, tell the user that Ghostty could not be found and suggest they verify it is installed.
