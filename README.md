![Nemi](assets/logo.png)

# Nemi for Claude and ChatGPT

Work in your [Nemi](https://nemilab.com) workspace from a conversation. This plugin connects Claude (claude.ai, the desktop and mobile apps, Cowork and Claude Code) and ChatGPT and Codex to your own Nemi account, and adds skills that teach the assistant how Nemi works, so it picks the right tool, keeps your data safe and gets things right the first time.

## What you can do

- **Files:** find anything by name across workspaces, read what is inside PDFs, Word, Excel, slides and photos, tidy folders in bulk, rename, colour-code, compress and extract archives, and check your storage.
- **Convert and edit:** turn files into other formats, shrink videos and PDFs, and edit pictures: exact sizes, square or round avatars, crops for social media, watermarks, backgrounds and target file sizes.
- **Docs:** write, edit, translate and illustrate Nemi Docs in Markdown, turn a file into a document, and publish one as a web page.
- **Sheets:** build budgets, trackers and tables with formulas that the Nemi engine can actually calculate, and turn a CSV or Excel file into a live sheet.
- **Forms:** go from a one-line brief to a themed, published form, change it safely, and get a clear summary of the responses.
- **Canvas:** draw flowcharts, mind maps, timelines, kanban boards and sticky-note walls.
- **Sharing:** make the right link every time: share links with passwords and expiry, upload links to collect files from people without an account, Rooms for clients, and Beam to get files onto your phone with a QR code.
- **Calendar and Meet:** read your agenda, find free time, create and move events (including repeating ones) and set up video calls with the link in the invite.

## Nemi in the conversation

In Claude, and in any other client that supports interactive views, answers from Nemi are not only text. They arrive as Nemi itself, with the same icons and colours as the app, and you can use them:

| You ask for | You get |
| --- | --- |
| a folder, or a search | a file browser: open folders, search every workspace, select files, then Share, Download or Summarise |
| a document | the page, set as a page: select a passage and ask about it |
| a spreadsheet | the grid with a formula bar: select cells for a sum, edit a cell in place, download a CSV |
| your calendar | a week you can page through: open an event, join its call, add one by pressing an empty hour |
| form responses | a chart per question and a table of responses |
| a canvas | the board, which you can drag and zoom |
| a share link, upload link, Room, meeting or published page | a card with Copy, a QR code, and whether it has a password and an end date |
| a Beam | the QR code and a countdown |
| "open Nemi" | home: storage, the coming week, recent documents and shortcuts |

The assistant knows what you are looking at and what you selected, so "share these", "why is this one wrong" and "move this to Friday" work without naming anything. Views open larger from the arrows in their corner, follow light and dark, and work on a phone. A view loads nothing from the network while you use it, and never makes a link or deletes anything by itself: those go through the conversation, where your client can ask you first. More: [Nemi inside the conversation](https://nemilab.com/help/developers/interactive-views).

Views are being switched on gradually. Without them, everything works as text.

## Getting started

1. Install the plugin:
   - **Claude and Cowork:** Customize, Plugins, Add, Add marketplace, then `https://github.com/GuusLab/nemi-plugin`, and install Nemi. Once Nemi is listed in the directory you can add it from there instead.
   - **Claude Code:** `/plugin marketplace add GuusLab/nemi-plugin`, then `/plugin install nemi@guuslab`.
   - **Codex:** `codex plugin marketplace add GuusLab/nemi-plugin`, then `codex plugin add nemi@guuslab`.
   - **ChatGPT:** add Nemi from the plugin directory once it is listed there.
2. Connect the Nemi connector when asked, and sign in with your Nemi account. Nemi asks you to approve the connection. In Claude the plugin can also connect on its own when you sign in to Claude, if your Claude email address is a verified address on your Nemi account; it then stays connected without signing in again. That connection never reaches Nemi Mail.
3. Ask in plain language, for example "What is on my Nemi calendar this week?" or "Make a sign-up form for Saturday's workshop and publish it".

You need a Nemi account. Everything works on every plan; what you can do is what your plan and your workspace role allow.

## Safety in Claude Code

The Nemi basics skill tells Claude to ask you before a Nemi tool deletes something, switches a link off, or makes a link or a page other people can open.

## Skills

| Skill | Helps with |
| --- | --- |
| `nemi-basics` | Where things live in Nemi, ids, interactive views, and when to ask before acting |
| `share-and-collect` | Share links, upload links, Rooms and Beam |
| `write-docs` | Creating, editing, illustrating and publishing Docs |
| `build-sheets` | Spreadsheets with the formulas Nemi supports |
| `run-forms` | Designing, theming, publishing and analysing Forms |
| `draw-on-canvas` | Diagrams and boards laid out on a grid |
| `plan-with-calendar` | Agenda, free time, events and Meet links |
| `organize-files` | Surveys, folder plans, bulk moves and clean-up |
| `convert-and-edit` | Format conversion, compression and picture edits |
| `read-and-extract` | Summaries, answers and data pulled from files |

## Commands

In Claude Code and Cowork: `/nemi:home`, `/nemi:agenda`, `/nemi:share`, `/nemi:collect`, `/nemi:tidy`, `/nemi:form`, `/nemi:results`, `/nemi:handoff` and `/nemi:catch-up`. In chat they load as skills and apply when the request fits. Claude Code and Cowork also get a `workspace-organizer` agent for large clean-ups.

The Nemi connector offers the same tasks as prompts of its own (Catch up, Agenda, Share files, Collect files, Tidy a folder, Make a form, Form results, Client hand-off), so a client that lists a connector's prompts shows them even without this plugin.

## Data and privacy

This plugin contains only instructions (Markdown) and configuration (JSON). It runs no code on your computer, installs no packages and has no hooks.

It connects to one server, Nemi's own MCP endpoint at `https://nemilab.com/api/mcp`, run by GuusLab. You sign in with OAuth 2.0; the plugin never sees or stores your password, and no API key is involved. Every request is made as you, under your own permissions in Nemi. What the assistant sends to Nemi is what a request needs: file and folder ids, search terms, the text of a document or spreadsheet it writes, form questions, event details and so on. What Nemi sends back is what you asked for: file lists, document and sheet contents, file contents when you ask the assistant to read a file, and the links it creates.

The plugin sends nothing anywhere else. Vault files never open through the connection, and photo libraries and account settings are out of reach. Links the assistant creates (shares, upload links, Rooms, published Docs and Forms, meetings) are real public addresses, and the skills tell the assistant to ask before making one unless you asked for it.

What happens to your data inside Nemi, and to the conversation in the assistant you use, is described in the [Nemi privacy policy](https://nemilab.com/privacy) (section 6.3 covers connected assistants) and in your assistant's own policy. Disconnect at any time from the connector settings in your assistant, or revoke the connection in Nemi.

## Support

Questions and problems: [support@nemilab.com](mailto:support@nemilab.com) or an issue in this repository. More about the connection: [Connecting Nemi to an AI assistant](https://nemilab.com/help/developers/ai-connection).

## License

MIT, see [LICENSE](LICENSE). Nemi and the Nemi logo are trademarks of GuusLab and are not covered by the license.
