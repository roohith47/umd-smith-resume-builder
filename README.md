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

## Getting the best results

**Try it on the samples first, before touching your own resume.** `assets/samples/` has two
fully fictional, fully built resumes — one with solid internship experience, one early-career
with no professional experience yet. Ask Claude to render one (`python3 scripts/build_resume.py
assets/samples/sample_experienced.json /tmp/out.docx`) and open it, so you know what a finished
one looks like and can see the two different section layouts before deciding how your own should
look.

**Expect to be interviewed first, not handed a form.** The first time you ask it to build your
resume, it won't just dump you into the six-section template in `resume.template.json`. It'll ask
a handful of questions — career stage and target role, whether you have relevant work experience
(or none, which is completely fine), projects, leadership/volunteer work, certifications, and
whether you have a specific job posting in mind. Answering those up front, even briefly, gets you
a much better first draft than "build my resume" with nothing else — but you don't need to have
everything figured out; it's built to adapt around whatever you actually have.

**It won't invent experience you don't have.** If you don't have professional work experience
yet, it drops that section rather than leaving it empty or padding it — and it'll tell you why it
restructured things the way it did. If something in a draft looks off, say so; it should explain
its reasoning, not just silently redo it.

**Tailoring toward a specific job?** Paste the posting (or a link's text) when you ask, and it'll
reorder and reword your bullets to surface whatever you have that overlaps with what that posting
is actually asking for, instead of just listing everything you've ever done.

**It's meant to be revisited, not a one-shot.** Your actual content lives in a plain JSON file (see
`references/resume-schema.md`) — keep it somewhere stable like `~/Desktop/Resumes/resume_data.json`
and just ask Claude to update it whenever you have a new internship, project, or role to add, or
want to re-tailor it for a different posting. It regenerates the `.docx` from that file every time,
so formatting never drifts between edits.

**One-page is a heuristic, not a guarantee.** The script estimates whether a draft fits one page
and will warn you if it looks long, but it can't actually render the page the way Word does.
Always open the `.docx` yourself and eyeball it, especially if you're near the warning threshold.

## How it works

- `SKILL.md` — when Claude should use this skill, and the step-by-step workflow.
- `references/` — the Smith OCS content rules, the SAR bullet-writing framework, an action-verb
  bank, and the full visual spec / JSON schema.
- `scripts/build_resume.py` — renders a resume JSON file into the formatted `.docx`.
- `assets/resume.template.json` — field-level reference for a new resume's shape (not meant to be
  copied wholesale — most people won't need all six sections).
- `assets/samples/` — two fictional, fully populated example resumes at different career stages.

```bash
python3 scripts/build_resume.py path/to/resume_data.json path/to/output.docx
```

See `references/resume-schema.md` for the full JSON schema.
