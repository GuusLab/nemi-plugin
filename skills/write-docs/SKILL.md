---
name: write-docs
description: "Write, edit, restructure, illustrate and publish Nemi Docs documents. Use when the user wants to draft, write up, edit, rewrite, proofread, translate, extend or summarise a document in Nemi, add a picture to one, turn notes or a file into a doc, or publish a doc as a web page."
---

# Nemi Docs

A Nemi Doc is a collaborative document stored in the Docs app, not in the file tree. It is read and written as Markdown.

## Markdown that survives

Supported: headings, **bold**, *italic*, ~~strike~~, ==highlight==, inline code and fenced code blocks, links, images, quotes, nested lists, task lists (`- [ ]`), GFM tables, horizontal rules and hard line breaks. Anything else (HTML, footnotes, math, custom containers) becomes plain text, so do not use it.

## Titles

Until the user renames a doc by hand, its first h1 or h2 becomes its title. So either:

- give `title` and start the body with the first section (no heading repeating the title), or
- start the body with a heading that is exactly the title.

Never open the body with a heading that disagrees with the title.

## Create

1. Pick the workspace (`workspace_list` if the user has several and did not say).
2. `doc_create` with `title` and the whole `body` in one call. Write the real content, not a skeleton, unless the user asked for an outline.
3. Reply with what you wrote in a sentence or two and the document's title. Do not paste the whole document back.

## Edit

There is no partial edit. Every change is: read, change the text, write it all back.

1. `doc_list` (it shows previews) to find the doc, then `doc_get` for the full Markdown.
2. Make the change on that Markdown. Keep everything you were not asked to change exactly as it was, including headings, links, image lines and task list states. Do not "improve" untouched paragraphs.
3. `doc_update` with `mode` "replace" and the complete new body.
4. To add at the end only (a new meeting's notes, a log entry), use `mode` "append" with just the new part; no need to read first.

Others may be editing the same document live. Read immediately before you write, and keep the gap short. If the user says someone else is working in it, append instead of replacing when that does the job.

## Pictures

`doc_add_image` is the only way to put a picture in a doc. Give a `file_id` for an image in the file tree (find it with `file_list`) or a Gallery `photo_id`, an optional `caption`, and `position` "start" or "end". A share link or a download link pasted into the Markdown will not render as a picture. To put a picture in the middle, add it, then `doc_get`, move the image line to where it belongs, and `doc_update` replace.

## From a file to a doc

To turn a PDF, Word file, notes or a transcript into a Nemi Doc: `file_read` the file (follow `next_offset` for long ones), then `doc_create` with the result written as proper Markdown. Say in the reply which file it came from.

## Publish

`doc_publish` puts the doc on the open web as a read-only page anyone can open without an account, that search engines may find, and that follows every later save. Only the doc's creator can publish.

- Confirm before publishing unless the user asked for it in so many words.
- Reply with the address and say plainly that it is public.
- `published` false takes the page down and the address stops working.

## Delete

`doc_delete` is permanent and only the creator can do it. Confirm first, naming the document.

## Good documents

- Match the user's language. A Dutch request gets a Dutch document.
- Use headings for anything longer than a screen, tables for anything the reader will compare, task lists for action items with owners.
- Meeting notes: date, attendees, decisions, action items as a task list with names.
