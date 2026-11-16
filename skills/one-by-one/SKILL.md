---
name: one-by-one
description: Used when you presented the user with a long list of things that needs their decision and they want to go through it one by one
disable-model-invocation: true
---

The user invoked this skill because you've given them a large list of things to go through.  This list may have accumulated over several messages or may have been all at once.  Either way, it's too much for them to process all at once and they want to go through it item by item.  

For each item, give them a reasonably concise description and ask, using the format below.  Assume they haven't read anything you said since the last message they sent you.  Once they've given you the answer/decision you need, move on to the next item.

## Format
**Item X/Y:** [Item title]

**Context:** [Prior context to help the user get their bearings]

**Description:** [Details on the item]

**[Decision/Ask]:** [What you need from the user]