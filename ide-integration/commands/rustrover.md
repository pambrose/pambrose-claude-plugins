---
description: Open RustRover with the current directory as the project root. Responds to "open rustrover", "open rr".
---

# Open RustRover

Open RustRover using the current working directory as the project root.

Run the following command:

```bash
open -a "RustRover" .
```

If that fails (e.g., the app name differs), try:

```bash
rustrover .
```

After running, confirm to the user: "Opened RustRover in [current directory]."

If both commands fail, tell the user that RustRover could not be found and suggest they verify it is installed
or that the `rustrover` command-line launcher is configured (RustRover > Tools > "Create Command-line Launcher").
