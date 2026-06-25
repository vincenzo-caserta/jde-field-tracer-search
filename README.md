# JDE FieldTracer — Search JD Edwards Event Rules by Field Alias

A free online tool that indexes JD Edwards EnterpriseOne **Event Rules** and lets you search them by the one thing that actually matters when you're tracing a field: the **field alias**.

🔍 **[Search JDE FieldTracer](https://vincenzocaserta.it/en/jde-field-tracer-online)**

This is a companion tool to [JDE Object Librarian Search](../README.md). Object Librarian Search tells you which objects exist and what they do. FieldTracer tells you what happens *inside* those objects, at the level of a single Event Rule line.

---

## The problem it solves

JD Edwards EnterpriseOne's native Cross Reference Facility (the F9860 / OMW cross-reference) can tell you which objects reference a given field. It stops there — at the object level. It does not tell you which Event Rule, which section, or which line.

In practice, that means a consultant tracing a field like `AN8` or `EV01` through a customization still has to open every object the Cross Reference returns and read Event Rules manually, line by line, to find the actual occurrence.

FieldTracer fills that specific gap: alias in, exact line-level occurrences out.

## What's in the index

The index is built from a real local export of Event Rules covering four object types:

| Type | Description |
|------|--------------|
| APPL | Interactive Application |
| UBE | Universal Batch Engine |
| TBLE | Table |
| BSFN | Business Function |

It currently holds **over 6.2 million indexed field occurrences**. Each occurrence record stores:

- **Object** — the object name (e.g. `P4310`, `R09801`)
- **Object Type** — `APPL`, `UBE`, `TBLE`, or `BSFN`
- **Section** — the Form name (APPL) or Section name (UBE/TBLE) where the alias was found
- **Event** — the Event Rule event, e.g. *Control is Exited* or *Dialog is Initialized*
- **Line Number** — the line inside the original Event Rules export
- **Direction** — `READ`, `WRITE`, or `BOTH`

## How direction classification works

`READ` means the alias is used as a value at that line. `WRITE` means a value is being assigned to it. `BOTH` applies to two-way interconnections, typically seen in Data Structure mappings where a field is simultaneously an input and an output.

This classification is **syntactic**, based on the Event Rule statement at that specific line — not a full data-flow simulation of the program. It tells you exactly what the line of code does, not what the variable's value will eventually be at runtime.

## Why alias matching is exact, not fuzzy

Field aliases are short, structured codes (3-8 characters, typically), not free-form text. A fuzzy or partial match on something like `AN8` would pull in `AN801`, `ANT8`, and any other alias sharing a few letters — noise that defeats the purpose when you're trying to trace one specific field. So the match is exact and case-insensitive: type `AN8`, get occurrences of `AN8`, nothing else.

## Search and filtering model

There are two distinct layers of filtering, and the distinction matters for how you use the tool:

1. **Server-side filters** (sent with the query): `Object Type`, `Object` (exact object name), `Section contains`, `Event contains`. These narrow the search itself before results are returned, and are useful when you already know roughly where to look — e.g. only inside `P4310`, or only in events whose name contains "Exited".

2. **Client-side quick filter**: a text box above the results table that filters whatever is already loaded on the current page, with no new request. Useful once you have a result set in front of you and want to scan it for something specific without re-querying.

Results are paginated (50 per page) and every column header is sortable — click once for ascending, again for descending.

## Using the Line column

The Line column refers to the line number inside the original Event Rules export the index was built from. If you have the same object open in Object Management Workbench, or in your own Event Rules export/listing, you can jump straight to that line instead of scrolling through the whole event.

## What this is not

This is not a replacement for the official Oracle Cross Reference Facility, and it's not an Oracle product. It's an independently built, focused lookup tool for the specific gap described above: section- and event-level detail that the native cross-reference doesn't expose. No login, no install, no OMW connection required — paste an alias, get occurrences.

## Related tool

If you're starting from an object name or description rather than a field alias, see [JDE Object Librarian Search](https://vincenzocaserta.it/en/jd-edwards-en/free-search-jde-object-librarian-online) — a searchable index of 68,908 Object Librarian entries (APPL, UBE, BSFN, BL, BSVW, DSTR, TBLE, GT) with System Reporting Code.

## About

Built by [Vincenzo Caserta](https://vincenzocaserta.it) — JD Edwards EnterpriseOne Technical Consultant with 11+ years of experience in custom development, BSFN, NER, APPL, UBE and upgrade retrofit.

> This is an independently prepared search tool based on a local Event Rules export. Not affiliated with Oracle Corporation. JD Edwards is a registered trademark of Oracle Corporation.
