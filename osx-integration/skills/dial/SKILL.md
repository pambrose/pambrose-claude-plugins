---
name: dial
description: >
  This skill should be used when the user asks to "dial", "call", "phone",
  "ring", or "contact" someone by name. Looks up a contact in macOS Contacts
  and initiates a phone call via the Phone/FaceTime app.
---

# Dial Contact

Look up a contact by name in the macOS Contacts app and initiate a phone call.

## Invocation

Triggered by `/dial <name>` or natural language like "call Dr. Smith" or "dial mom".

## Workflow

### Step 1: Extract the Contact Name

Parse the contact name from the user's request. The name may be a first name,
last name, full name, or nickname.

### Step 2: Search macOS Contacts

Run this osascript to search for matching contacts:

```bash
osascript -e '
tell application "Contacts"
  set matchingPeople to every person whose name contains "<SEARCH_NAME>"
  set output to ""
  repeat with p in matchingPeople
    set pName to name of p
    set output to output & "NAME:" & pName & linefeed
    repeat with ph in phones of p
      set phLabel to label of ph
      set phValue to value of ph
      set output to output & "PHONE:" & phLabel & "|" & phValue & linefeed
    end repeat
  end repeat
  return output
end tell
'
```

Replace `<SEARCH_NAME>` with the contact name. The search uses `contains` for
fuzzy matching (e.g., "smith" matches "John Smith").

### Step 3: Parse Results

The output format is:

```
NAME:John Smith
PHONE:_$!<Main>!$_|+1 (555) 123-4567
PHONE:_$!<Mobile>!$_|+1 (555) 987-6543
```

Phone label mappings from macOS Contacts:

| Label in output | Meaning |
|---|---|
| `_$!<Main>!$_` | Phone (main) |
| `_$!<Mobile>!$_` | Mobile |
| `_$!<iPhone>!$_` | iPhone |
| `_$!<Home>!$_` | Home |
| `_$!<Work>!$_` | Work |
| `_$!<WorkFAX>!$_` | Work fax |
| `_$!<HomeFAX>!$_` | Home fax |
| `_$!<Pager>!$_` | Pager |

### Step 4: Select the Phone Number

Apply this priority order when choosing which number to dial:

1. **Main** (`_$!<Main>!$_`) — preferred
2. **Mobile** (`_$!<Mobile>!$_`)
3. **iPhone** (`_$!<iPhone>!$_`)
4. **Home** (`_$!<Home>!$_`)
5. First available number

If only one number exists, use it regardless of label.

### Step 5: Handle Ambiguity

- **No matches:** Inform the user and suggest checking the spelling or trying a
  different name.
- **Multiple contacts match:** Present the list of matching contacts with their
  phone numbers and ask the user to choose.
- **Multiple numbers, no Main label:** Present the available numbers with their
  labels and ask the user to choose.

### Step 6: Dial

Strip all non-numeric characters except `+` from the phone number, then open
the Phone app:

```bash
open "tel:<CLEANED_NUMBER>"
```

This uses macOS Continuity to hand off to the user's iPhone, or opens
FaceTime for audio calls.

Confirm to the user which contact and number is being dialed.
