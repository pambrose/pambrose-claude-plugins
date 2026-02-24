---
description: Open IntelliJ IDEA with the current directory as the project root. Responds to "open ij" or "open in intellij".
---

# Open IntelliJ

Open IntelliJ IDEA using the current working directory as the project root.

Run the following command:

```bash
open -a "IntelliJ IDEA" .
```

If that fails (e.g., the app name differs), try:

```bash
idea .
```

After running, confirm to the user: "Opened IntelliJ IDEA in [current directory]."

If both commands fail, tell the user that IntelliJ IDEA could not be found and suggest they verify it is installed
or that the `idea` command-line launcher is configured.
