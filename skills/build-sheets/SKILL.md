---
name: build-sheets
description: "Build, fill, fix and analyse Nemi Sheets spreadsheets with formulas that the Nemi engine can calculate. Use when the user wants a spreadsheet, budget, tracker, planning grid, invoice list or table in Nemi, wants to add or change rows, fix a formula, or turn a CSV or Excel file into a live sheet."
---

# Nemi Sheets

A Nemi Sheet is a live spreadsheet in the Sheets app. Through this connection you read and write the **first tab only**, cell by cell, keyed by A1 references. Formatting (bold, colours, widths, number formats) is done in the app.

## Cells

- Keys are A1 references: columns A to Z, then AA, AB and on; rows from 1.
- Values are text, numbers or formulas starting with `=`. Use `$` for absolute references (`$B$1`).
- Write numbers as plain numbers (`1250.5`), never with currency signs or thousands separators, or they become text and formulas skip them. Put the unit in the header instead: "Amount (EUR)".
- Dates in formulas are serial numbers from 1899-12-30. Use `DATE(2026,10,1)` in a formula, or write an ISO date as text for display.

## The formula engine is not full Excel

These functions exist, and only these:

SUM, AVERAGE, MIN, MAX, COUNT, COUNTA, COUNTBLANK, PRODUCT, MEDIAN, STDEV, ROUND, ROUNDUP, ROUNDDOWN, ABS, SQRT, POWER, MOD, INT, CEILING, FLOOR, EXP, LN, LOG, SIGN, IF, IFS, IFERROR, AND, OR, NOT, TRUE, FALSE, ISBLANK, ISNUMBER, ISTEXT, CONCAT, CONCATENATE, TEXTJOIN, LEN, UPPER, LOWER, TRIM, LEFT, RIGHT, MID, REPT, SUBSTITUTE, TEXT, COUNTIF, SUMIF, AVERAGEIF, COUNTIFS, SUMIFS, TODAY, NOW, DATE, YEAR, MONTH, DAY, HOUR, MINUTE, EOMONTH, WEEKDAY, WEEKNUM, DATEDIF, VLOOKUP, HLOOKUP, XLOOKUP, INDEX, MATCH

Also available: arithmetic, comparisons, `&` for joining text, ranges (`B2:B20`) and TRUE/FALSE.

Not available, with what to use instead:

| Not there | Use |
| --- | --- |
| FILTER, SORT, UNIQUE, QUERY | SUMIFS / COUNTIFS summaries, or sort the rows yourself before writing |
| ARRAYFORMULA, spilling ranges | one formula per row |
| SUMPRODUCT | a helper column with the product, then SUM |
| NETWORKDAYS, WORKDAY | WEEKDAY-based helper columns |
| VALUE, NUMBERVALUE | write the number as a number in the first place |
| other tabs (`Sheet2!A1`) | keep everything on the first tab |

Errors you may see: `#NAME?` (unknown function), `#REF!`, `#VALUE!`, `#DIV/0!`, `#N/A`, `#CYCLE`. Wrap lookups and divisions in IFERROR where a blank is expected.

## Create

1. Plan the layout first: a header row in row 1, one record per row from row 2, one kind of value per column, totals in a clearly labelled row below the data (or in a summary block to the right, leaving a blank column between).
2. `sheet_create` with `title` and all the `cells` at once, formulas included.
3. `sheet_get` and check the `display` of every formula cell. Fix any error before replying.
4. Reply with what the sheet contains and where the totals are, plus one or two numbers from the `display` values.

## Change

`sheet_set_cells` patches only the cells you send; everything else is left alone. An empty string clears a cell. The grid grows to fit.

1. `sheet_get` first, to see where the data ends and which columns hold what. Never assume the layout.
2. To add rows, write them below the last filled row. Then extend any totals or ranges that should include them (`=SUM(B2:B20)` becomes `=SUM(B2:B23)`).
3. `sheet_get` again and check the `display` values of the formulas you touched.

## Read and analyse

`sheet_get` returns each filled cell with `value` (the stored value or formula) and `display` (the calculated result). Quote `display` values when answering; they are what the user sees. When the user asks "what is the total" and there is no total cell, compute it yourself from the values and say that you did.

## From a file

To turn a CSV or Excel file into a live sheet: `file_read` it (Excel and ODS come back as CSV), map the columns to A1 keys, convert numbers to plain numbers, and `sheet_create`. For a very large file, create the sheet with the first chunk and add the rest with `sheet_set_cells` in batches of a few hundred rows.

For a static export nobody will edit, `file_write` a CSV into the file tree instead.

## Delete

`sheet_delete` is permanent and creator-only. Confirm first. A form's response sheet feeds that form; deleting it breaks the form's record.
