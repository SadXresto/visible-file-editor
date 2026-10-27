---
name: visible-file-editing
description: Use when a user asks ChatGPT or Codex to edit, revise, repair, convert, or compare a DOCX, PDF, spreadsheet, presentation, code file, or other file and wants changes to be visible and reviewable by a human.
---

# Visible File Editing

## Goal
Edit the user's actual file, preserve everything outside the requested scope, and make the result auditable by a person. Do not merely provide instructions when the file itself is available and tools permit editing.

## Workflow
1. **Inspect before editing.** Identify the file type, structure, page/sheet/slide count where possible, and relevant formatting or dependencies. For code projects, inspect related files and tests before changing code.
2. **Confirm scope only when necessary.** If the request is clear, proceed. If a change could destroy content or meaning, ask one focused question or make a separate copy and explain the assumption.
3. **Protect the original.** Never overwrite the sole original by default. Save a clearly named edited copy. Keep originals and backups separate from generated deliverables.
4. **Make minimal, targeted edits.** Preserve styles, headers/footers, tables, page setup, formulas, slide layouts, metadata, links, images, and code conventions unless asked to change them. Do not invent missing facts or silently rewrite unrelated content.
5. **Make changes visible.** Choose the strongest honest review format the file type supports:
   - DOCX: produce the edited DOCX and a change log. If native Track Changes cannot be written reliably, do not claim it is enabled; instead provide a separate redline/marked-up copy or a before/after table. Use comments only when the available tools genuinely support them.
   - PDF: preserve the original PDF when possible; for text edits, provide an edited PDF plus a change summary. If direct editing risks layout or text integrity, create an annotated/redline copy or explain the limitation and provide an editable source format when available. Never silently rasterize a PDF if selectable text/accessibility matters.
   - XLSX/ODS/CSV: preserve formulas, sheet names, data types, formatting, and links where possible. Highlight changed cells in a review copy and provide a cell-level change log (sheet, cell, old value, new value). Never replace formulas with their displayed values without permission.
   - PPTX/ODP: preserve slide order, master/layout, speaker notes, and media where possible. Provide an edited deck and slide-by-slide change log; visibly mark changed elements in a separate review copy when feasible.
   - Code/config/text/Markdown/HTML: use a unified diff or clearly marked patch, explain behavior changes, and run available tests or syntax checks. Keep secrets and credentials out of output.
   - Images and other binary/proprietary formats: only edit if the tools support that exact format. Otherwise explain the limitation and offer a safe conversion or an inspectable alternative; never pretend an unsupported format was edited.
6. **Human-readable deliverables.** When possible, provide: (a) edited file, (b) review copy with visible marks, (c) concise change report, and (d) validation results. Use clear filenames such as `document_edited.docx`, `document_review.docx`, and `CHANGELOG.md`.
7. **Verify.** Reopen or parse the output, compare it against the original, and check page/sheet/slide counts, formatting, formulas, links, and relevant content. For code, run tests/lint/build checks that are available. State which checks actually ran and which did not.
8. **Report honestly.** Summarize the meaningful changes, mention any unverified areas or format limitations, and link to the actual generated files. Never claim a file is downloadable unless the exact output exists. Never claim perfect layout preservation without checking.

## ChatGPT-specific behavior
- Use attached files and available file-generation tools directly when possible.
- Return actual edited artifacts, not just pasted replacement text, when the user requests a downloadable edited file.
- If tools cannot preserve a format's structure, say so before presenting a lossy conversion as equivalent.

## Codex-specific behavior
- Work in the user's project/worktree, inspect repository guidance and `git status` first, and avoid overwriting unrelated changes.
- For code edits, prefer a small patch; show a diff, run relevant tests, and report the exact commands and outcomes.
- Do not commit, push, delete user files, or run destructive commands unless explicitly authorized.

## Common tooling guidance (use only when available)
- DOCX: `python-docx` can edit content and formatting but does not provide complete native Word Track Changes authoring.
- XLSX: `openpyxl` can edit workbook cells/styles and create highlighted review copies; verify macro-enabled files separately because saving with the wrong library can strip macros.
- PPTX: `python-pptx` supports many slide edits but not every PowerPoint feature.
- PDF: use a PDF-aware library for text/annotations; check whether the file is scanned and whether fonts/layout can be preserved. OCR is an interpretation step and may contain errors.
- Code/text: use version-control diffs or unified diffs.
- ODT/ODS/ODP and proprietary formats: check actual tool support first; do not assume Office libraries support them.

## Final response format
1. Link the edited file(s).
2. List the most important changes in plain language.
3. Link or include the change log/diff/review copy if generated.
4. State validation performed and any limitations.
