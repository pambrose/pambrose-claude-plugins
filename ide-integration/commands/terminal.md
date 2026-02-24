---
description: Open macOS Terminal. Responds to "open terminal".
---

# Open Terminal

Open macOS Terminal in the current working directory.

Run the following command:

```bash
open -a Terminal "$(pwd)"
```

After running, confirm to the user: "Opened Terminal in [current directory]."

If the command fails, tell the user that Terminal could not be found.
