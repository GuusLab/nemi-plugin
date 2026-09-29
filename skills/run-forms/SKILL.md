---
name: run-forms
description: "Design, theme, publish and analyse Nemi Forms from a brief. Use when the user wants a form, survey, questionnaire, sign-up, registration, RSVP, poll, quiz, feedback or intake form, wants to change or close one, or asks what people answered or how responses are going."
---

# Nemi Forms

A Nemi Form has a title, a description, questions (fields), optional sections, a presentation, a theme and a public response link. Every response becomes one row in the form's response sheet.

## Limits

Up to 30 fields, labels up to 120 characters, answers up to 2000 characters, up to 20 options per choice question. If a tool says otherwise, trust the tool.

## Field types

`text`, `textarea`, `email`, `phone`, `url`, `number`, `date`, `time`, `select` (one choice, shown as buttons), `dropdown` (one choice, compact), `multiselect` (several choices), `checkbox` (one yes/no tick), `rating` (stars, `ratingMax`), `scale` (numbers with `min`, `max`, `scaleMinLabel`, `scaleMaxLabel`).

Each field is `{ label, type, required?, options?, description?, placeholder?, min?, max?, ratingMax?, scaleMinLabel?, scaleMaxLabel?, imageUrl? }`. `select`, `dropdown` and `multiselect` need `options`. A choice field without options, or any field without a label, is silently dropped, so always read the form back.

Pick the type that makes the answers countable. "How did you hear about us" is a `select` with an "Other" option, not a `text`. Satisfaction is a `scale` 1 to 5 or 1 to 10 with labelled ends, not free text. Use `email` and `phone` so answers are validated.

## From a brief to a live form

1. Draft the questions from what the user said. Keep it short: every extra question costs responses. Mark only what is truly needed as `required`.
2. Decide the presentation: `list` (all on one page, good for short forms) or `focus` (one question at a time, good for surveys and on phones).
3. `collect_name` defaults to true. Turn it off for anonymous surveys and say so in the description.
4. Pick a theme that fits the occasion (below).
5. `form_create` with everything, including `confirmation_message`. Use `publish` true only when the user wants it live now.
6. `form_get` and check that every question survived. Fix anything dropped with `form_update`.
7. Reply with the question list in a line or two and, when published, the `respond_url` in full.

## Themes

`theme` is merged onto the current one. Useful keys: `preset` (nemi, midnight, sunset, editorial, aurora, playful, mono, forest, pastel), `bgType` (solid, gradient, mesh, dots, grid, diagonal, image), `bgColor`, `gradientFrom`, `gradientTo`, `accent`, `text`, `muted`, `cardStyle` (solid, glass, outline, flat), `radius`, `headingFont`, `bodyFont`, `fieldStyle`, `buttonStyle`, `buttonShape`, `buttonLabel`, `align`, `progressBar`. Colours are `#rgb` or `#rrggbb`.

Start from a preset and change one or two keys: a party gets `playful` or `sunset`, a professional intake `editorial` or `mono`, a nature event `forest`. Keep text readable on the background.

## Sections and branching

`sections` is `[{ id?, title, description?, fieldIds? }]`. A section with no `fieldIds` is an intro screen. Where branching is available, a section can carry `routes: [{ fieldId, option, nextSectionId }]` and `defaultNextSectionId`, keyed on a choice or checkbox question inside that section; `nextSectionId` "end" finishes the form. Field ids are assigned on create, so for branching: create the fields first, `form_get` to learn their ids, then `form_update` with the sections.

## Changing a form

- Add questions without touching the rest: `form_add_fields` (optionally into a `section_id`).
- Change settings, the theme or the whole question list: `form_update`. `fields` and `sections` **replace the full lists**: `form_get` first and keep the existing field ids, or earlier answers lose their link to their question.
- Reshaping questions after responses exist can misalign the response sheet's columns. Warn the user before changing or reordering questions on a form that already has responses; adding at the end is safe.
- Stop new responses but keep the page up: `form_update` with `accepting` false. Take the link down: `form_publish` with `published` false. Both keep the address and the responses.

## Reading results

`form_responses` returns counts for every choice, rating and scale question across **all** responses, plus a page of individual responses (`limit` up to 200, `offset` to page, `order` newest or oldest).

- Quote the counts the tool gives; do not count rows yourself, since you may only have one page.
- Lead with the headline: how many responses, then the two or three things that stand out, then anything that needs action (a low score, a repeated complaint, a sold-out option).
- For open text answers, group them into themes and quote one or two short, representative answers.
- The response sheet id is returned; the user can open it in Sheets, and `sheet_get` can read it.

## Delete

`form_delete` removes the form and its link permanently, and keeps the response sheet. Confirm first.
