---
name: share-and-collect
description: "Pick and make the right kind of Nemi link to send files to someone, collect files from someone, or give a client a space of their own. Use when the user wants to share, send, hand over, deliver, request, collect or receive files, set up a client portal or project page, or change, find or switch off a link they already made."
---

# Sharing and collecting with Nemi

Nemi has five ways to move files between people. Most mistakes come from picking the wrong one, so decide first, then act.

## Choose the kind of link

| The user wants to | Use | Lasts |
| --- | --- | --- |
| send files to somebody | `share_create` | until it expires or is revoked |
| receive files from somebody without a Nemi account | `upload_link_create` | until it expires or is switched off |
| give a client or group one place to see, download and maybe upload files for a project | `room_create`, then `room_add_file` | until archived or it expires |
| get files onto their own phone or a device next to them, right now | `beam_send` (returns a QR) | 15 minutes, at most 25 files |
| put a written Nemi Doc on the web as a page | `doc_publish` (see the write-docs skill) | until unpublished |
| download a file themselves | `file_download_url` | one hour, for the user only |

Rules of thumb:

- "Send it to Jan", "share the contract", "deliver the photos": a share link.
- "Ask the client for their logo", "let people send me receipts", "collect submissions": an upload link on a folder.
- "A page for this client", "somewhere the agency can find everything", ongoing back-and-forth: a Room.
- "Get this on my phone", "I'm standing next to the printer": Beam. More than 25 files, or needed later: a share link instead.
- Never use `file_download_url` to give files to someone else. It is for the user and dies in an hour.

## Sensible defaults

Offer these rather than asking a list of questions:

- **Contracts, IDs, invoices, anything personal:** a password and an expiry of 7 to 14 days. Say the password in your reply, once, and suggest sending it through a different channel than the link.
- **Photos or deliverables for a client:** no password, 30 days, a friendly `name` for the page and a short `message`.
- **Upload links:** a `name` that says what to send ("Send your receipts for March"), `notify` true so the owner hears when something arrives, and an expiry when the collection has a deadline.
- **Rooms:** `allow_guest_uploads` only when the guests need to send something back.

If an organisation policy caps expiry or forbids something, the tool says so; report it plainly.

## Making a share link

1. Find the files: `file_list` with `query`, or list the folder. Confirm you have the right ones when names are ambiguous ("There are two files called contract.pdf, one from May and one from August. Which one?").
2. `share_create` with `file_ids`, and `name`, `message`, `expires_in_days` and `password` as fitting.
3. Reply with the full address exactly as returned, what is in it, when it expires and whether it has a password.

## Collecting files

1. Pick or create the destination folder (`folder_create`). A dedicated folder per collection keeps arrivals separate from the user's own files.
2. `upload_link_create` with `folder_id`, `name`, `message`, and `notify`, `max_files` and `expires_in_days` as fitting.
3. Tell the user that uploads count against the workspace owner's storage while the link is on.
4. Later, `upload_link_list` shows how many files arrived; `file_list` on the folder shows them.

## A Room for a client

1. `workspace_list` if needed, then `room_create` with `workspace_id`, a `name` the guests will recognise, an optional `note`, and `password`, `allow_guest_uploads` and `expires_in_days` as fitting.
2. `room_add_file` for each file. Files must come from the Room's own workspace.
3. Reply with the Room link and say plainly that anyone holding it can see and download everything inside, so it should be sent privately, never posted publicly.
4. `room_get` later shows what guests uploaded. `room_archive` closes it and keeps everything.

## Beam

1. `beam_send` with the `file_ids` (at most 25).
2. If the client shows the QR, tell the user to scan it; otherwise give the link. Mention it stops working in 15 minutes.
3. Files others beamed to the user are in `beam_list`, not in the file tree.

## Changing or ending a link

- Change title, message, expiry or password but keep the address: `share_update`. Send only what changes; an empty password removes it, `expires_in_days` 0 means never.
- Stop a share for good: `share_revoke`. The same address can never be switched back on, so confirm first unless the user said to revoke it.
- Stop an upload link: `upload_link_disable`. Files already received stay.
- Find the id with `share_list` or `upload_link_list` first. Match on the name, the address or the date the user mentions.

## Checking what is out there

When the user asks "what have I shared" or wants a clean-up, list with `share_list`, `upload_link_list` and `room_list`, and present a short table: name, what it holds, expiry, password yes or no. Offer to switch off the ones that look stale; do not switch anything off without a yes.
