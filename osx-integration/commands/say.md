---
description: Speak text aloud using macOS text-to-speech. Responds to "say", "speak", "tts", "text to speech".
argument-hint: "<text> [--voice <voice>] [--rate <rate>]"
allowed-tools:
  - Bash
---

# Say

Speak text aloud using the macOS `say` command.

## Behavior

Speak the user's provided text using macOS text-to-speech. Parse optional flags for voice and rate.

Run the following command:

```bash
say 'TEXT' -v VOICE -r RATE
```

- Replace TEXT with the user's text. Use single quotes to avoid shell interpretation of `$`, `` ` ``, and `\`.
  If the text contains single quotes, escape each `'` as `'\''`.
- Replace VOICE with the provided voice name, or omit `-v VOICE` entirely to use the system default
- Replace RATE with the provided speech rate (words per minute), or omit `-r RATE` entirely to use the default

**Note:** The `say` command blocks until speech is complete. For long text, this may take a while.

After running, confirm to the user: "Spoke: [text]"

If the user asks what voices are available, run:

```bash
say -v '?'
```

And display the list of available voices.

If the command fails, tell the user that text-to-speech could not be used.
