---
description: Open Cursor editor with the current directory as the project root. Responds to "open cursor", "open cur".
---

# Open Cursor

Open Cursor editor using the current working directory as the project root.

Run the following command:

```bash
open -a "Cursor" .
```

If that fails (e.g., the app name differs), try:

```bash
cursor .
```

After running, confirm to the user: "Opened Cursor in [current directory]."

If both commands fail, tell the user that Cursor could not be found and suggest they verify it is installed
or that the `cursor` command-line launcher is configured (Cursor > Command Palette > "Install 'cursor' command").
