---
description: Open the current repo in GitHub in the default browser. Responds to "open github", "open gh".
---

# Open GitHub

Open the current repository on GitHub in the default browser.

Run the following command:

```bash
gh browse
```

After running, confirm to the user: "Opened GitHub repo in browser."

If the command fails, tell the user that the `gh` CLI could not be found or the current directory is not a GitHub
repository. Suggest they verify `gh` is installed (`brew install gh`) and that the directory is a git repo with a
GitHub remote.
