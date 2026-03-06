---
category: Lifestyle
id: treatment-plans
name: Treatment Plans
description: Generate concise (3-4 page), focused medical treatment plans in LaTeX/PDF format for all clinical specialties. Supports general medical treatment, rehabilitation therapy, mental health care, chronic disease management, perioperative care, and pain management. Includes SMART goal frameworks, evidence-based interventions with minimal text citations, regulatory compliance (HIPAA), and professional formatting. Prioritizes brevity and clinical actionability.
allowed-tools: [Read, Write, Edit, Bash]
---

# Treatment Plans

Create concise, clinician-ready treatment plans that focus on actionable care decisions. Default to short, scannable output with SMART goals, clear interventions, timelines, and monitoring.

## When to Use

- Individualized treatment planning
- Chronic disease management
- Rehabilitation programs (PT/OT/cardiac rehab)
- Mental health care plans
- Perioperative pathways
- Pain management protocols

## Output Requirements

- **Default length**: 3–4 pages; use **1-page format** when possible.
- **First page**: Executive summary only (no TOC or long narrative).
- **Style**: Bullets, tables, and short sections; avoid long prose.
- **Citations**: Minimal (0–3 short in-text citations if necessary).
- **Compliance**: De-identify patient data and follow HIPAA guidelines.

## Required Sections

1. **Title + Summary** (first page)
2. **Patient Info** (de-identified)
3. **Assessment / Problem List**
4. **Goals** (SMART, short- and long-term)
5. **Interventions** (medication, non-pharm, procedures)
6. **Timeline & Follow-Up**
7. **Monitoring Parameters**
8. **Risks & Safety**
9. **Expected Outcomes**
10. **Patient Education**

## Optional Sections (use as needed)

- Rehabilitation plan details
- Mental health care specifics
- Perioperative checklist
- Pain management strategy

## Visuals (Recommended)

Include **one simple schematic** when it improves clarity (flowchart, timeline, care coordination). If available, use the `scientific-schematics` skill; otherwise omit.

## LaTeX Skeleton (Concise)

```latex
\\documentclass[11pt]{article}
\\usepackage[margin=1in]{geometry}
\\begin{document}
\\title{Treatment Plan}
\\date{\\today}
\\maketitle

\\section*{Executive Summary}
% 1-page summary with key goals, interventions, and timeline

\\section*{Patient Info}
% De-identified demographics and diagnosis

\\section*{Goals (SMART)}
% Short-term and long-term goals

\\section*{Interventions}
% Medications, therapies, procedures

\\section*{Monitoring \\& Follow-up}
% What to track and when

\\section*{Risks \\& Safety}
% Safety considerations and red flags

\\section*{Expected Outcomes}
% Benchmarks and timelines
\\end{document}
```
