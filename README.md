# resume-builder

A [Claude Code](https://claude.com/claude-code) skill that builds, updates, and tailors a one-page
resume as a Word `.docx` for University of Maryland Robert H. Smith School of Business MS students.

It follows the Smith School Office of Career Services (OCS) content and writing standards (SAR
bullet structure, grammar/number/abbreviation rules, section content guidance), but uses a visual
house style that deliberately differs from the stock OCS template:

| | Stock Smith OCS template | This skill |
|---|---|---|
| Font | Calibri | Times New Roman |
| Margins | 0.6in top, 0.9in sides, 0.2in bottom | 0.5in on all sides ("Narrow") |
| Bullets | Right-justified circle bullets | Left-aligned hyphen bullets, justified text |

This repo ships with **no real resume content** — just a blank placeholder template
(`assets/resume.template.json`) with bracketed fields like `[Past-tense action verb]`. Point Claude
Code at this folder (drop it in `~/.claude/skills/`) and it will interview you for your actual
experience before writing anything.

## Install

```bash
git clone https://github.com/roohith47/umd-smith-resume-builder.git ~/.claude/skills/resume-builder
```

Then, in Claude Code, just ask it to build or update your resume — it'll find this skill
automatically.

## How it works

- `SKILL.md` — when Claude should use this skill, and the step-by-step workflow.
- `references/` — the Smith OCS content rules, the SAR bullet-writing framework, an action-verb
  bank, and the full visual spec / JSON schema.
- `scripts/build_resume.py` — renders a resume JSON file into the formatted `.docx`.
- `assets/resume.template.json` — blank starting point for a new resume.

```bash
python3 scripts/build_resume.py path/to/resume_data.json path/to/output.docx
```

See `references/resume-schema.md` for the full JSON schema.
