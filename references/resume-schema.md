# Resume JSON Schema

`scripts/build_resume.py` takes one JSON file describing the whole resume and renders it with the
house visual style from `visual-spec.md`. Keep the candidate's actual resume content in a JSON file
like this (suggested location: alongside their resume files, e.g.
`~/Desktop/Resumes/resume_data.json`) and regenerate the `.docx` from it any time content changes,
rather than hand-editing a Word file.

## Top level

```json
{
  "name": "Full Name",
  "contact_line": "(555) 555-5555 • email@school.edu • linkedin.com/in/handle",
  "work_auth_line": "Eligible to work in the U.S. under F-1 OPT (Filing October 2026)",
  "sections": [ ... ]
}
```

- `name` — rendered centered, bold, all caps, 14pt.
- `contact_line` — centered, plain, 10pt. Build the " • " separators into the string yourself.
- `work_auth_line` — optional. Omit the key entirely (or leave it an empty string) if there's
  nothing to say here.
- `sections` — ordered list; render order follows list order. See `visual-spec.md` for the
  recommended default order (Education, Technical Skills, Work Experience, Project Experience,
  Additional Experience and Leadership, Distinctions) and the one case where you'd swap Work
  Experience and Project Experience.

## Section object

Every section has:

```json
{ "heading": "WORK EXPERIENCE", "type": "entries", ... }
```

`type` is one of `"entries"`, `"skills"`, or `"bullets"`, each with its own extra fields below.

### `type: "entries"` — Education, Work Experience, Project Experience

```json
{
  "heading": "WORK EXPERIENCE",
  "type": "entries",
  "entries": [
    {
      "header_left": "Company or School Name",
      "header_right": "City, ST, Country",
      "sub_blocks": [
        {
          "sub_left": "Job Title or Degree",
          "sub_right": "Month Year – Month Year",
          "bullets": [
            "Led cross-functional team of seven to redesign the onboarding flow.",
            "Reduced average support-ticket resolution time by 30% by introducing a triage bot."
          ]
        }
      ]
    }
  ]
}
```

- `header_left` / `header_right` render bold, on one line, left/right split. This is the
  organization + location line. **`header_left`/`header_right` are required for every entry,
  including Project Experience** — they're what renders bold. For a project with no separate
  organization, put the **project title itself** in `header_left` (bold) and its date in
  `header_right` (bold); leave `header_right` as `""` only if there's genuinely no date to show,
  never to work around a missing organization. Do not move a project's title down into a
  `sub_blocks` entry — that slot always renders italic, so a title placed there silently loses its
  bold formatting, which is the one thing that visually marks it as a title rather than a detail
  line.
- `sub_blocks` is a list so one employer with multiple titles becomes multiple `sub_blocks` under
  one shared `header_left`/`header_right`, instead of repeating the employer. Most entries will
  have exactly one `sub_blocks` item.
- `sub_left` / `sub_right` render italic, one line, left/right split. Leave `sub_right` as `""` for
  a project's course/context line that doesn't need a right-aligned date (e.g., a subtitle like
  "BUDT 705, Individual Project (Tableau, Excel)" under a title line that already carries the
  date).
A standalone project (no organization) looks like this — note the title+date sit in
`header_left`/`header_right` (bold), and the course/context line goes in `sub_blocks[0].sub_left`
(italic), not the other way around:

```json
{
  "header_left": "Store Performance and Market Analysis – North Peak Coffee",
  "header_right": "Fall 2026 (In Progress)",
  "sub_blocks": [
    {
      "sub_left": "BUDT 705, Individual Project (Tableau, Excel)",
      "sub_right": "",
      "bullets": [
        "Analyzed ten years of store performance, competitor landscape, and demographic data for a fictional U.S. coffee chain.",
        "Built a multi-dashboard Tableau Story identifying untapped market opportunity for new store locations."
      ]
    }
  ]
}
```

- `bullets` is a flat list of strings. Each becomes its own hyphen-bulleted, justified paragraph.
  Write these using `bullet-writing-framework.md` and `content-rules.md` — don't just take
  dictation.

### `type: "skills"` — Technical Skills

```json
{
  "heading": "TECHNICAL SKILLS",
  "type": "skills",
  "lines": [
    { "label": "Languages and Tools", "items": "Python (pandas, NumPy, scikit-learn), SQL, Power BI, Tableau" },
    { "label": "Certifications", "items": "AWS Academy – Machine Learning Foundation, Data Engineering" }
  ]
}
```

Each line renders as `**Label:** items`, comma-separated, no bullets.

### `type: "bullets"` — Additional Experience and Leadership, Distinctions

```json
{
  "heading": "ADDITIONAL EXPERIENCE AND LEADERSHIP",
  "type": "bullets",
  "bullets": [
    "Research Intern, AI/ML and Deep Learning — National Institute of Technology, Warangal (July – September 2024): developed and evaluated CNN-based disease prediction models across multiple architecture variants."
  ]
}
```

This is the compact format: each bullet is one self-contained line carrying the role, org, dates,
and a short description together — no separate header line. Use this for entries too brief to
justify a full `entries`-style block (a short research stint, a club role, a one-off volunteer
gig). `DISTINCTIONS` uses the same shape, typically without dates (awards, languages, interests).

## Certifications as their own section (optional)

If the candidate has certifications substantial enough to pull out of Technical Skills, add a
second `"skills"`-type section headed `CERTIFICATIONS` right after Technical Skills, with one
`lines` entry per certification (label = cert name, items = awarding institution, or vice versa).

## Regenerating

```bash
python3 scripts/build_resume.py path/to/resume_data.json path/to/output.docx
```

The script prints a one-page fit estimate after writing the file — read `visual-spec.md`'s
"One-page check" note on why that's a heuristic, not a guarantee.
