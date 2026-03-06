---
category: Content & Media
id: pptx-composio
name: Pptx
description: "Presentation creation, editing, and analysis. When Claude needs to work with presentations (.pptx files) for: (1) Creating new presentations, (2) Modifying or editing content, (3) Working with layouts, (4) Adding comments or speaker notes, or any other presentation tasks."
license: Proprietary. LICENSE.txt has complete terms
---

# PPTX creation, editing, and analysis

Use this skill when the user needs to create, edit, or analyze .pptx files.

## Read and inspect
- Extract text quickly: `python -m markitdown file.pptx`
- Inspect structure: `python ooxml/scripts/unpack.py <pptx> <dir>` then read `ppt/` XML files

## Create new presentation
- Build HTML slides and convert with `scripts/html2pptx.js` (one HTML file per slide)
- Use clear hierarchy, consistent spacing, and readable fonts
- Validate by generating thumbnails and checking for cutoffs or overlaps

## Edit existing presentation
- Unpack, edit slide XML (`ppt/slides/slide{N}.xml` and related files), then validate
- Repack the presentation after changes

## Output expectations
- Summarize edits, files touched, and how to regenerate the final .pptx
