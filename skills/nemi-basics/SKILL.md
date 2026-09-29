---
name: nemi-basics
description: "How Nemi is put together and how to move around it safely. Use whenever the user mentions Nemi, their Nemi workspace, or asks to find, open, change or share anything that lives in Nemi, and before the first Nemi tool call of a conversation."
---

# Working in Nemi

Nemi is the user's workspace: files and folders, Nemi Docs, Sheets, Forms and Canvas boards, share and upload links, Rooms, Beam, Meet and calendars made in Nemi. Every tool acts as the signed-in user, with their permissions and nothing more.

## Where things live

Nemi keeps different kinds of things in different places, and the tools follow that split. Looking in the wrong place is the most common reason a search "finds nothing".

| The user says | Where it is | How to find it |
| --- | --- | --- |
| a file, a PDF, a photo, a folder | the file tree of a workspace | `file_list` (with `query` to search by name across every workspace) |
| a document, a doc, notes written in Nemi | Nemi Docs | `doc_list`, then `doc_get` |
| a spreadsheet, a sheet, a tracker | Nemi Sheets | `sheet_list`, then `sheet_get` |
| a form, a survey, a sign-up | Nemi Forms | `form_list`, then `form_get` |
| a board, a whiteboard, a diagram | Nemi Canvas | `canvas_list`, then `canvas_get` |
| something someone beamed to them | Beam | `beam_list` (never in `file_list`) |
| a client space, a project page for guests | Rooms | `room_list`, then `room_get` |
| a meeting link | Meet | `meet_list` |
| their agenda | Nemi calendars | `calendar_events` |
| a link they sent out | shares and upload links | `share_list`, `upload_link_list` |
| how Nemi itself works | the help centre | `help_search`, then `help_article` |

If a name search in `file_list` returns nothing, try the Docs, Sheets, Forms and Canvas lists before telling the user it does not exist. A "report" is as likely to be a Nemi Doc as a PDF.

## Ids

- Start with `workspace_list` when a tool needs a `workspace_id` and you do not have one. If the user has one workspace, use it without asking. If they have several and the request does not say which, ask, naming the workspaces.
- Ids are never interchangeable. A file id is not a document id, a document id is not a sheet id, a share id is not a file id. Use the id the tool asks for, taken from the tool that returned that kind of thing.
- Folder ids accept `"root"` for the top of a workspace.
- Never invent an id, and never guess one from a name. List first.

## Getting files into Nemi

- Text you write (notes, Markdown, CSV, JSON, code): `file_write`. For something people will keep editing, a Nemi Doc or Sheet is usually better.
- A small binary you already hold, under 1 MB (an icon, a screenshot): `file_upload` with the bytes base64-encoded.
- Anything larger, up to 50 MB, or a file on the user's computer: `file_upload_url`, then PUT the bytes to the returned address (or give the user the curl line), then `file_confirm_upload` with the `upload_ticket`. Never base64 a large file into a tool call.
- Files from other people: an upload link (see the share-and-collect skill).

## Before you act

Reads are free: list, search and read as much as you need to be sure.

Ask before anything that cannot be taken back or that puts something on the open internet, and say exactly what will happen:

- **Permanent:** `folder_delete` (always permanent), `doc_delete`, `sheet_delete`, `form_delete`, `canvas_delete`, `share_revoke`, `meet_delete`, and `file_delete` with `permanent` or on a free plan (free plans have no trash).
- **Public:** `share_create`, `upload_link_create`, `doc_publish`, `form_publish`, `room_create`, `room_add_file`, `beam_send`, `meet_create`. A link from these is a real, working address that anyone holding it can open.

When the user has clearly asked for the exact action ("delete the draft called X", "share this with the client"), that request is the confirmation; do not ask twice. When you are about to do more than they said (deleting five files when they named one, publishing when they asked for a draft), stop and check.

## Reporting back

- Links that tools return (shares, upload links, published docs and forms, Rooms, meetings, downloads) go in your reply in full, exactly as returned. Never shorten, rebuild or guess a Nemi address, and never expose a storage address.
- Say what changed in plain words: "Moved 14 files into 4 folders", not the raw tool output.
- If a tool refuses (a plan limit, a permission, a feature that is not available on the account), tell the user what it said and what they can do about it. Do not retry the same call hoping for a different answer.
- An undo button exists only inside Nemi's own assistant. Changes made through this connection are not recorded for undo, which is one more reason to confirm before destructive steps.

## What is out of reach

- Vault files never open through this connection, by design.
- Calendars connected from Google, Outlook or an ICS feed cannot be read or written; only calendars made in Nemi can. An agenda read here may be incomplete; say so.
- Only the first tab of a spreadsheet is reachable, and cell formatting is set in the app.
- Joining a meeting, recording and receiving a Beam happen in a browser.
- Gallery photo libraries and account settings are not part of this connection.

When the user asks how to do something in the Nemi app itself (a setting, a button, a plan feature), search the help centre with `help_search` and answer from the article rather than from memory.
