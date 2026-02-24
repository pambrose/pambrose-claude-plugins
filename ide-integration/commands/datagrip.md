---
description: Open DataGrip with the current directory as the project root. Responds to "open datagrip", "open dg".
---

# Open DataGrip

Open DataGrip using the current working directory as the project root.

Run the following command:

```bash
open -a "DataGrip" .
```

If that fails (e.g., the app name differs), try:

```bash
datagrip .
```

After running, confirm to the user: "Opened DataGrip in [current directory]."

If both commands fail, tell the user that DataGrip could not be found and suggest they verify it is installed
or that the `datagrip` command-line launcher is configured (DataGrip > Tools > "Create Command-line Launcher").
