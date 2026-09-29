---
name: draw-on-canvas
description: "Draw diagrams, flowcharts, mind maps, timelines, kanban boards and sticky-note walls on a Nemi Canvas board, and read or rearrange an existing board. Use when the user wants something drawn, sketched, mapped or laid out visually in Nemi, or asks what a board says."
---

# Nemi Canvas

A Canvas board is an infinite whiteboard. Through this connection a board is a flat list of elements drawn in order; later elements draw on top.

## Elements

Every element is `{ type, x, y, w, h, ... }` in world pixels, y growing downwards.

| type | use for | notes |
| --- | --- | --- |
| `rect` | boxes, steps, cards | `radius` rounds corners (default 6) |
| `ellipse` | start and end nodes, emphasis | |
| `text` | titles, labels | `fontSize` 8 to 96 (default 16), `bold`, `align` |
| `sticky` | notes, ideas, kanban cards | `fill` is the note colour |
| `arrow` | flow, cause and effect | x, y is the start; w, h is the delta to the end (may be negative); `bend` curves it |
| `line` | connectors without direction, dividers | same geometry as arrow |
| `draw` | freehand | `points` as [dx, dy] relative to x, y; avoid unless asked |

Common fields: `stroke` (hex, default `#1e1e1e`), `fill` (hex, or `transparent`, the default), `strokeWidth` (1 to 12, default 2), `text` (a shape's label, up to 4000 characters), `dash` (solid, dashed, dotted), `opacity` (0.05 to 1), `angle` (degrees). Leave `id` out on new elements; ids are assigned. At most 2000 elements per board.

## Every update replaces the whole board

`canvas_update` with `elements` **replaces everything**. To change a board:

1. `canvas_get` (leave `include_strokes` false unless you must keep freehand drawings; if the board has `draw` elements you must set it true, or they are lost).
2. Edit the list: keep every existing element, with its id, exactly as it was unless you are changing it.
3. `canvas_update` with the complete list.
4. Check the returned snapshot: elements that were invalid are dropped.

Cards that point at files, documents, links or sheet data (`file`, `docref`, `embed`, `sheetdata`) come back from `canvas_get`; send them back unchanged.

## Lay it out on a grid

Boards look deliberate when everything sits on a grid. Use these numbers unless the content needs more room:

- Box: 200 x 80, text inside via `text`, `fill` a light colour, `stroke` a darker one of the same hue.
- Gap: 80 between boxes horizontally, 60 vertically.
- Title: a `text` element at the top left, `fontSize` 32, `bold` true.
- Arrows: from the middle of one box's edge to the middle of the next. For a box at (x, y, w, h) flowing right to the next at (x2, y2): start (x + w, y + h/2), delta (x2 - (x + w), (y2 + h2/2) - (y + h/2)).

Patterns:

- **Flowchart:** steps left to right or top to bottom, decisions as `ellipse` or a rotated `rect` with two labelled outgoing arrows.
- **Mind map:** the central idea in the middle, branches on a circle around it, sub-ideas further out; `line` connectors.
- **Timeline:** a long horizontal `line`, events as small boxes alternating above and below, dates in `text`.
- **Kanban:** column headers as `text`, `sticky` cards stacked beneath each (200 x 120, gap 20).
- **Retro or brainstorm wall:** one `sticky` colour per category, with a `text` legend.

Soft colours read well together: yellow `#FFF3B0`, green `#D9F2C4`, blue `#D6E8FF`, pink `#FFD9E6`, purple `#E6DDFF`, orange `#FFE0C2`, with dark text.

## Create

1. `canvas_create` with a `title`.
2. Work out every coordinate first, then `canvas_update` with the whole element list in one call.
3. Reply with what you drew in a sentence. If the client shows the board, do not describe it element by element.

## Read

`canvas_get` and read the `text` and `sticky` elements to know what the board says. Group what you find by position (columns, clusters) when summarising a wall of notes.

## Delete

`canvas_delete` is permanent. Confirm first.
