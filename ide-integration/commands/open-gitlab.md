---
description: Open the current repo in GitLab in the default browser. Responds to "open gitlab", "open gl".
---

# Open GitLab

Open the current repository on GitLab in the default browser.

Run the following command:

```bash
glab repo view --web
```

After running, confirm to the user: "Opened GitLab repo in browser."

If the command fails, tell the user that the `glab` CLI could not be found or the current directory is not a GitLab
repository. Suggest they verify `glab` is installed (`brew install glab`) and that the directory is a git repo with a
GitLab remote.
