---
name: convert-and-edit
description: "Convert files between formats, shrink them, and edit pictures in Nemi: resize, crop, square or round avatars, rotate, watermark, adjust, change background or hit a file size. Use when the user wants a file as another format (PDF, JPG, PNG, WebP, MP4, MP3 and more), smaller, compressed, or a picture changed in any way."
---

# Converting and editing in Nemi

Two tools, and picking the right one matters:

- **`image_edit`** for anything about how a picture looks, and its output format in the same step.
- **`file_convert`** for format changes of any other kind (documents, audio, video, HEIC, RAW, PSD) and for making non-image files smaller.

Results are saved beside the original (or in `folder_id` in the same workspace). The original is never changed.

## Converting

1. `file_convert_formats` with the `file_id`. It lists the formats this file can become and the settings each accepts, with allowed values and defaults. Always call it unless the request is a plain format change that obviously exists.
2. `file_convert` with `output_formats` (extensions, e.g. `["pdf"]`, or several at once: `["mp4", "webm"]`). Put settings in `options` (all formats) or `options_per_format` (one format, e.g. `{ "jpg": { "quality": "70", "width": "1600" } }`). Keys and values must come from step 1.
3. Reply with the names of the new files and their sizes.

Making a file smaller: convert it to its own format with a lower quality or size. A 40 MB video for WhatsApp becomes an `mp4` with a lower resolution; a heavy PDF may accept a quality setting.

HEIC, RAW and PSD photos: convert to jpg or png first, then `image_edit` if more is needed.

## Editing pictures

`image_edit` takes `file_ids` (up to 20; the same operations apply to each), an ordered list of `operations`, and `output`.

When the edit depends on the current size (making it exactly 1080 wide, cropping the right third), call `file_get` with `image_details` true first.

Operations, each `{ "op": ..., ... }`:

- `resize` `{ width, height, percent, fit, position, background, enlarge }`. Give both edges for an exact size; `fit` cover (default when both given) crops to fill, contain pads with `background`, stretch distorts, inside and outside keep the ratio.
- `crop` `{ aspect: "1:1" | "4:5" | "16:9", position }` or `{ left, top, width, height }`. `position` is centre, top, bottom, left, right, a corner, or smart to keep the subject.
- `rotate` `{ degrees, background }`, `flip`, `mirror`, `trim` `{ threshold }` (cuts a plain border), `pad` `{ size or top/right/bottom/left, color }`, `round` `{ radius }`, `circle`.
- `grayscale`, `negate`, `normalize`, `tint` `{ color }`, `sepia`, `blur` `{ amount }`, `sharpen` `{ amount }`, `brightness`, `saturation`, `contrast` `{ value, 1 is unchanged }`, `hue` `{ degrees }`, `gamma` `{ value }`, `flatten` `{ color }` (fills transparency).
- `text` `{ text, position, size, color, opacity }`, `watermark` `{ text, position, opacity }`.

Colours are hex (`#ff7a4d`) or white, black, transparent.

`output`: `{ format: jpg | png | webp | avif | gif | tiff, quality: 1-100, lossless, max_kb, keep_metadata }`. Leave `format` out to keep the source format. `max_kb` lowers the quality until the file fits (jpg, webp, avif). An edit that needs transparency becomes a PNG.

Do not call `file_convert` after `image_edit`; the format is set in the same step.

## Recipes

| Request | operations | output |
| --- | --- | --- |
| Profile picture | `crop` aspect 1:1 position smart, `resize` 800 x 800, `circle` | png |
| Instagram portrait | `crop` aspect 4:5 position smart, `resize` 1080 x 1350 | jpg, quality 85 |
| Website hero | `resize` width 1920 | webp, quality 80 |
| Under 500 KB for an email | `resize` width 2000 (if larger) | jpg, max_kb 500 |
| Watermark proofs | `watermark` text "PROOF" position centre opacity 0.3 | keep |
| Scan cleanup | `grayscale`, `normalize`, `sharpen` | png |
| Logo on transparent | `trim` | png |

Metadata (location, camera) is removed unless `keep_metadata` is true; mention it when the user shares photos publicly.
