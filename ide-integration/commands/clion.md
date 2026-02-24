---
description: Open CLion with the current directory as the project root. Responds to "open clion".
---

# Open CLion

Open CLion using the current working directory as the project root.

Run the following command:

```bash
open -a "CLion" .
```

If that fails (e.g., the app name differs), try:

```bash
clion .
```

After running, confirm to the user: "Opened CLion in [current directory]."

If both commands fail, tell the user that CLion could not be found and suggest they verify it is installed
or that the `clion` command-line launcher is configured (CLion > Tools > "Create Command-line Launcher").
