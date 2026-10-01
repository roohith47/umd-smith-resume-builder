---
name: resume-builder
description: Build, update, or tailor a one-page UMD Smith School of Business resume as a Word .docx, in a house style (Times New Roman, 0.5in "Narrow" margins, hyphen bullets, justified body text) that deliberately differs from the stock Smith OCS template (Calibri, wide margins, circle bullets). This is a GENERIC skill for any UMD Smith MS student -- it ships with only a blank placeholder template, never real resume content. Use this whenever the user asks to create, update, revise, or tailor their resume, add or rewrite a resume bullet point, turn a new internship/job/project/leadership role into resume content, tailor their resume to a specific job posting, or asks about resume formatting, structure, one-page length, or wording standards for a UMD/Smith MS job search -- even if they don't say "resume builder" or name this skill directly. Also trigger for requests like "help me word this bullet point" or "does this bullet sound strong enough" in a resume context.
---

# Resume Builder (UMD Smith MS, house style)

This skill produces a one-page resume `.docx` for a University of Maryland Robert H. Smith School
of Business MS student. It follows the Smith School Office of Career Services (OCS) content and
writing standards, but with a visual template that deliberately differs from the stock OCS
template: Times New Roman instead of Calibri, 0.5in margins on all sides instead of the OCS's wider
asymmetric margins, and a hyphen-bullet/justified-text layout instead of the OCS's right-justified
circle bullets. Don't drift back toward the stock OCS visual template even if source material (like
an official template file) suggests otherwise — these visual deviations are an intentional house
style this skill was built around, not an oversight.

**This is a generic, shareable skill — it has no one's personal resume content baked into it.**
`assets/resume.template.json` is a blank placeholder structure (bracketed placeholders like
`[Past-tense action verb]`, `Full Company Name`), not a real example. Every time this skill runs
for a new person, build their actual resume data from scratch through conversation — never invent
plausible-sounding fake experience, and never reuse another person's resume content (from a
previous run, another file on disk, etc.) as a starting point for someone else's resume.

Read `references/visual-spec.md` once at the start of a session that touches this skill — it has
the full visual rationale and the generic entry schema the build script expects. You generally
won't need to re-read it on every single bullet edit within the same session.

## Core workflow

1. **Find or create the content source file.** This skill is driven by a JSON file (see
   `references/resume-schema.md` for the schema) rather than a Word file you hand-edit directly —
   regenerating from structured data is what keeps the formatting correct every time.
   - If the user already has a resume data JSON for this skill (ask, or check common places like
     `~/Desktop/Resumes/` or wherever they keep resume files), use that.
   - If this is their first time, copy `assets/resume.template.json` as the starting point and
     interview the user to replace every placeholder with their real information — don't leave
     bracketed placeholders in the final file, and don't fabricate specifics (companies, dates,
     numbers) they haven't given you.

2. **Gather or update content.**
   - If the user is adding a new role/project/award: ask what's missing to run the SAR method from
     `references/bullet-writing-framework.md` (situation, action, result), unless they've already
     given you enough to work with.
   - If they're tailoring toward a specific job posting: ask for the posting (or a pasted
     description) if you don't have it, extract the repeated required skills, and prioritize/word
     bullets to surface the user's matching experience first. This is the forward-looking approach
     from the bullet-writing framework — don't just reorder for its own sake, reorder because a
     specific target role says to.
   - If they just want a general refresh: review the existing JSON for weak bullets (responsibility
     only, no result, no verb) and propose stronger versions using the three-stage pattern.

3. **Apply the writing standards** in `references/content-rules.md` to every piece of text you
   write or touch: action-verb-first bullets (pull from `references/action-verbs.md` when you need
   a fresh one), SAR structure, quantified results, grammar/number/abbreviation rules, and the
   one-page constraint. These are content rules, not visual ones — they apply regardless of
   template, so don't skip them just because the visual side is handled by the script.

4. **Edit the JSON file directly** (it's just a text file — use your normal file-editing tools).
   Keep the section order from `visual-spec.md` unless the user's situation calls for swapping
   Work Experience and Project Experience (that section explains exactly when).

5. **Render it:**
   ```bash
   python3 scripts/build_resume.py path/to/resume_data.json path/to/output.docx
   ```
   The script applies the whole visual spec automatically — don't hand-build formatting in Word or
   try to replicate the template yourself. It prints a one-page fit estimate; if it warns the draft
   runs long, cut the least-relevant bullet rather than shrinking fonts/margins below spec (shrinking
   is a last resort the user would need to explicitly ask for).

6. **Tell the user what changed and where the file landed**, and remind them to open the `.docx`
   and eyeball the one-page fit themselves — the script's estimate is a heuristic, not a substitute
   for looking at it (see the "One-page check" note in `visual-spec.md`).

## Reference files

- `references/visual-spec.md` — the full visual template: fonts, margins, section-header style,
  the two-column header/sub-header layout, bullet style, and the JSON structure the build script
  consumes. Read this before editing `scripts/build_resume.py` or explaining formatting choices.
- `references/resume-schema.md` — the JSON schema in full, with worked examples per section type.
- `references/content-rules.md` — grammar, numbers, abbreviations, section content guidance, and
  the name/contact block rules, straight from the official Smith OCS guidelines (these weren't
  changed by the house visual-style choices above — keep applying them).
- `references/bullet-writing-framework.md` — the SAR elicitation method and the three-stage
  bullet-strengthening pattern. Read this before drafting any new bullet from scratch.
- `references/action-verbs.md` — a bank of past-tense action verbs to open bullets with.
- `assets/resume.template.json` — a blank placeholder resume, structurally correct but with no
  real content. Copy this for a brand-new user; never substitute someone else's real resume data.

## Things to get right

- Every new or edited bullet ends with a period, starts with a past-tense action verb (present
  tense only for something still ongoing), and states a quantified result wherever one exists.
- No possessives, no contractions, no "&" (spell out "and"). Numbers 1–9 spelled out, 10+ as
  digits, except money and percentages which are always digits.
- Degree names spelled out ("Master of Science," not "MS"); GPA and GMAT are the only acronyms
  allowed unspelled; state/country abbreviations are the only abbreviations allowed at all.
- Don't silently add a Projects section, a Certifications section, or reorder sections without
  saying why — mention the reasoning (e.g., "I moved Project Experience above Work Experience since
  this is for an internship search and your fieldwork is thinner than your coursework projects").
- If the user asks for something that contradicts a content rule (e.g., "just put MS instead of
  spelling it out"), it's their resume — make the change, but mention you're deviating from the
  Smith OCS content standard so they're making an informed call, not quietly diverging from a
  standard they may still care about for other sections.
- Never populate a new user's resume with content from another person's resume, another skill
  invocation, or any file you happen to find that isn't theirs.
