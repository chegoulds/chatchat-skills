---
id: docx
category: Content & Media
name: Docx
description: "Use this skill whenever the user wants to create, read, edit, or manipulate Word documents (.docx files). Triggers include: any mention of \\"Word doc\\", \\"word document\\", \\".docx\\", or requests to produce professional documents with formatting like tables of contents, headings, page numbers, or letterheads. Also."
license: Proprietary. LICENSE.txt has complete terms
---

# DOCX creation, editing, and analysis

Use this skill when the user needs to create, read, edit, or manipulate .docx files.

## Read and inspect
- Extract text (with tracked changes): `pandoc --track-changes=all file.docx -o output.md`
- Inspect structure: `python scripts/office/unpack.py file.docx unpacked/`

## Convert legacy .doc
- Convert before editing: `python scripts/office/soffice.py --headless --convert-to docx file.doc`

## Create new document
- Generate with docx-js (`npm install -g docx`)
- Validate after creation: `python scripts/office/validate.py file.docx`

## Edit existing document
- Unpack, edit XML, validate, and repack using the provided office scripts

## Output expectations
- Summarize edits, files touched, and how to regenerate the final .docx
