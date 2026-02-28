# osx-integration

macOS system integration commands for Claude Code.

## Commands

| Command | Description |
|---------|-------------|
| `/clipboard` | Read from or write to the macOS clipboard |
| `/dial` | Look up a contact in macOS Contacts and initiate a phone call |
| `/notify` | Send a macOS desktop notification |
| `/say` | Speak text aloud using macOS text-to-speech |
| `/screenshot` | Take a screenshot |

## Usage Examples

```
/clipboard Hello, world!       # Copy text to clipboard
/clipboard                     # Read clipboard contents
/notify Build complete         # Send a notification
/notify Task done --title CI   # Notification with custom title
/say Hello there               # Speak text aloud
/say Hello --voice Samantha    # Speak with a specific voice
/screenshot                    # Full screen capture
/screenshot --window           # Interactive window capture
/dial Dr. Smith                # Call a contact by name
call mom                       # Natural language also works
```

## Requirements

- macOS (uses built-in `pbcopy`, `pbpaste`, `osascript`, `say`, `screencapture`, and Contacts app)
