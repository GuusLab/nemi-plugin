---
description: "Make a Nemi upload link so someone without an account can send me files"
argument-hint: "<what to collect> [from whom] [deadline]"
---

Set up a Nemi upload link to collect: $ARGUMENTS

Follow the share-and-collect skill. Create or pick a dedicated folder for the arrivals, then `upload_link_create` with a clear page title, a short message telling the sender what to upload, `notify` true, and an expiry if there is a deadline. Reply with the full address, the folder the files will land in, and a one-line message the user can paste when sending the link.
