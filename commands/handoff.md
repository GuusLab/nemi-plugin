---
description: "Open a Nemi Room for a client and put the right files in it"
argument-hint: "<client or project> [which files] [allow uploads]"
---

Set up a client handoff in Nemi for: $ARGUMENTS

Follow the share-and-collect skill. Find the files for this client or project with `file_list`. Show the user which files you will put in the Room and the settings (password, guest uploads, expiry) and wait for a yes. Then `room_create`, `room_add_file` for each file, and reply with the Room link, the file list, and a reminder that anyone holding the link can see and download everything in it, so it should be sent privately.
