---
name: organize-files
description: "Tidy, sort, rename, colour-code and clean up files and folders in a Nemi workspace, and report on storage. Use when the user wants a folder or workspace organised, files sorted into folders, a naming scheme applied, duplicates or old files found, folders created or coloured, space freed, or asks how much storage they use."
---

# Organising files in Nemi

Reorganising is where an assistant can save the most time and do the most damage. Always survey, propose, get a yes, then act in as few calls as possible.

## 1. Survey

- `workspace_list`, then `file_list` on the folder the user means (`folder_id`, or leave it out for the top; `limit` up to 200). List subfolders that matter too.
- For a whole workspace, walk the folders one level at a time. Stop and summarise if it is very large rather than listing thousands of files.
- `storage_info` when space is part of the question.

## 2. Propose

Group the files and show the plan as a short table before moving anything:

| Folder | Files going there |
| --- | --- |
| Invoices 2026 | 23 PDFs named invoice-*, factuur-* |
| Photos | 41 images |
| Contracts | 6 files |
| (stays where it is) | 4 files that do not fit a group |

Good groupings, in order of preference: what the user asked for; by project or client when names show one; by type of document (invoices, contracts, receipts, photos, exports); by year when files span several. Keep existing folders the user made; add to them rather than rebuilding. Never propose deleting anything as part of a tidy-up; offer it separately.

## 3. Act, in bulk

- `folder_create` with `names` makes all new folders in one call (up to 50) and returns their ids. Add `color` to colour-code (red, orange, amber, yellow, lime, green, teal, cyan, sky, blue, indigo, violet, purple, magenta, pink, rose).
- `file_move` with `moves` sorts everything in one call: `[{ "file_ids": [...], "target_folder_id": "..." }, ...]`. Files that are refused are reported and the rest still move.
- `folder_color` recolours a set of folders in one call (a colour name, a hue 0 to 360, or "default").
- `file_rename` one file at a time; keep the extension. A naming scheme like `2026-03-14 Invoice Acme.pdf` (date first) sorts well.
- `folder_rename` for folders.

Then report what happened in one or two lines: "Made 4 folders and moved 70 files. 4 files stayed at the top because they did not fit a group: ..."

## Finding things to clean up

- **Duplicates:** files with the same name and size in the same workspace, or names like `report (1).pdf`, `report copy.pdf`. `file_get` shows size and date when you need to compare.
- **Big files:** list and sort by size; suggest compressing (see the convert-and-edit skill) or archiving.
- **Old files:** by date, when the user gives a cut-off.
- **Stale links:** `share_list`, `upload_link_list` and `room_list` for links nobody needs any more.

Present what you found and let the user choose. Only then delete, one explicit list at a time.

## Deleting

- `file_delete` sends a file to the trash, where it can be restored, unless `permanent` is true. Free plans have no trash: their deletes are always permanent. The result says which happened; pass that on.
- `folder_delete` removes the folder and every folder inside it **permanently**, without the trash. The files inside are kept and move to the top of the workspace. A vault, or a folder holding one, is refused.
- Confirm deletes by name and count ("Delete these 12 files? They go to the trash.") unless the user already named exactly what to delete.

## Archiving

- `archive_compress` packs files into a zip (or tar, tar.gz and others) beside them or in a `folder_id` in the same workspace. Useful before sending many files, or to bundle an old project. The originals stay; offer to delete them only if the user wants the space back.
- `archive_extract` unpacks a zip or tar next to itself or into a folder.

## Storage

`storage_info` reports the plan, space used and space left in GB. When the user is near the limit, find the largest files and the biggest folders, and suggest compressing media, deleting duplicates or emptying old upload-link folders. Beam files and trash may count too; say what the tool reports.
