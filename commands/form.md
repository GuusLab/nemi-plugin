---
description: "Turn a short brief into a finished, themed Nemi Form"
argument-hint: "<what the form is for, who fills it in, when it closes>"
---

Build a Nemi Form for: $ARGUMENTS

Follow the run-forms skill. Draft the questions with the types that make answers countable, choose list or focus presentation, pick a fitting theme preset, and write a confirmation message. Show the user the question list and ask whether to publish now. Then `form_create` (with `publish` true if they said yes), read it back with `form_get` to confirm every question survived, and reply with the full `respond_url` when it is live.
