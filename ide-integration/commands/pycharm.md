---
description: Open PyCharm with the current directory as the project root. Responds to "open pycharm".
---

# Open PyCharm

Open PyCharm using the current working directory as the project root.

Run the following command:

```bash
open -a "PyCharm" .
```

If that fails (e.g., the app name differs), try:

```bash
charm .
```

After running, confirm to the user: "Opened PyCharm in [current directory]."

If both commands fail, tell the user that PyCharm could not be found and suggest they verify it is installed
or that the `charm` command-line launcher is configured (PyCharm > Tools > "Create Command-line Launcher").
