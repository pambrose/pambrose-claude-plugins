---
description: Send a macOS notification. Responds to "notify", "notification", "alert".
argument-hint: "<message> [--title <title>] [--subtitle <subtitle>] [--sound <sound>]"
allowed-tools:
  - Bash
---

# Notify

Send a macOS desktop notification using `osascript`.

## Behavior

Send a notification with the user's message. Parse optional flags for title, subtitle, and sound.

Default title is "Claude Code" if none is provided.

Run the following command:

```bash
osascript -e 'display notification "MESSAGE" with title "TITLE" subtitle "SUBTITLE" sound name "SOUND"'
```

- Replace MESSAGE with the user's message text
- Replace TITLE with the provided title, or "Claude Code" if not specified
- Replace SUBTITLE with the provided subtitle, or omit `subtitle "SUBTITLE"` entirely if not specified
- Replace SOUND with the provided sound name, or omit `sound name "SOUND"` entirely if not specified

After running, confirm to the user: "Notification sent."

If the command fails, tell the user that the notification could not be sent.
