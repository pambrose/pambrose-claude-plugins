---
description: Take a screenshot on macOS. Responds to "screenshot", "screen capture", "screencapture".
argument-hint: "[filename] [--window] [--clipboard] [--delay <seconds>]"
allowed-tools:
  - Bash
---

# Screenshot

Take a screenshot using the macOS `screencapture` command.

## Behavior

Capture the screen and save it to a file. Parse optional flags for capture mode and delay.

### Default (full screen capture):

```bash
screencapture FILENAME
```

- Replace FILENAME with the user's provided filename, or default to `screenshot.png` in the current directory

### With options:

- `--window` or `-w`: Interactive window capture — add `-w` flag to screencapture
- `--selection` or `-s`: Interactive region selection — add `-s` flag to screencapture
- `--clipboard` or `-c`: Capture to clipboard instead of file — add `-c` flag and omit filename
- `--delay <seconds>` or `-d <seconds>`: Add delay before capture — add `-T SECONDS` flag

Example with all options:

```bash
screencapture -w -T 3 screenshot.png
```

After running, confirm to the user what was captured:
- If saved to file: "Screenshot saved to [filename]."
- If copied to clipboard: "Screenshot copied to clipboard."

If the command fails or produces a blank image, tell the user that the screenshot could not be taken and suggest
checking that Screen Recording permission is granted in System Settings > Privacy & Security > Screen Recording.
