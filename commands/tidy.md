---
description: "Propose and apply a clean folder structure for a messy Nemi folder"
argument-hint: "[folder name or workspace]"
---

Tidy up this part of Nemi: $ARGUMENTS

If nothing was named, ask which workspace and folder, listing the workspaces from `workspace_list`.

Follow the organize-files skill: survey with `file_list`, then propose a plan as a short table (new folders, what goes in each, what stays). Do not move, rename or delete anything until the user says yes. Once they do, create folders in one `folder_create` call and move everything in one `file_move` call, then report what moved and what stayed.
