---
description: Open Warp terminal. Responds to "open warp", "open w".
---

# Open Warp

Open Warp terminal in the current working directory.

Run the following command:

```bash
open -a Warp "$(pwd)"
```

After running, confirm to the user: "Opened Warp in [current directory]."

If the command fails, tell the user that Warp could not be found and suggest they verify it is installed.
