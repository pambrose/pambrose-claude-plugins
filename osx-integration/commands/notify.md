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

**Important:** Before interpolating any user text into the AppleScript string, escape all `"` characters as `\"` and
all `\` characters as `\\` to prevent AppleScript injection.

### Available sounds

If the user asks what sounds are available, list these built-in macOS sounds or run `ls /System/Library/Sounds/`:

Basso, Blow, Bottle, Frog, Funk, Glass, Hero, Morse, Ping, Pop, Purr, Sosumi, Submarine, Tink

After running, confirm to the user: "Notification sent."

If the command fails, tell the user that the notification could not be sent.
