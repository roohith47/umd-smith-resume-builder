# Visual Spec — House Style

This is the resume's visual template: a deliberate hybrid of the Smith School OCS official format
and the candidate's own established resume style. `scripts/build_resume.py` implements this spec
directly, so you shouldn't need to hand-build formatting — but read this if you're editing the
script, debugging output, or explaining to the candidate why the resume looks the way it does.

## Where this deviates from the official Smith OCS template, and why

The candidate reviewed the official guidelines and asked for three specific overrides, confirmed
directly:

1. **Font: Times New Roman everywhere**, not the official Calibri. Keep the official size scheme
   (name larger, headers mid-size, body smallest) — only the typeface changes.
2. **Margins: Word's built-in "Narrow" preset — 0.5 inch on all four sides.** The official
   guideline's asymmetric margins (top ≥0.6", sides ≥0.9", bottom ≥0.2") are not used here.
3. **Bullets and layout follow the candidate's own existing resume, not the official template.**
   The official template specifies right-justified circle bullets; the candidate's resume (and
   this house style) instead uses left-aligned hyphen bullets with justified paragraph text, and
   keeps the official template's left/right split for name-vs-location and title-vs-date lines.

Everything else — section content rules, grammar, SAR bullet structure — still follows the official
Smith OCS guidelines in `content-rules.md`. Only the *visual* template changed.

## Page and font

- Page size: 8.5 x 11 in (US Letter).
- Margins: 0.5 in top, bottom, left, right.
- Font family: Times New Roman, throughout — name, headers, body, dates, everything.
- Font sizes: name 14pt, section headers 11pt, all body text (education, skills, bullets,
  dates, locations) 10pt.
- No page numbers.

## Name and contact block

- Name: centered, bold, 14pt, all caps, first line of the document.
- Contact line: centered, 10pt, plain (not bold/italic), fields separated by " • " — typically
  phone, email, LinkedIn URL in that order.
- Optional third line, also centered, 10pt: work authorization or security clearance statement
  (e.g., "Eligible to work in the U.S. under F-1 OPT (Filing October 2026)").

## Section headers

- Bold, ALL CAPS, left-aligned, 11pt.
- A thin bottom border/rule under the heading text helps separate sections visually (matches the
  official template's "1-point border" convention) — the build script adds this automatically.
- Canonical default order: EDUCATION → TECHNICAL SKILLS → WORK EXPERIENCE → PROJECT EXPERIENCE →
  ADDITIONAL EXPERIENCE AND LEADERSHIP → DISTINCTIONS. This is a starting point, not a mandate —
  see "Adapting sections to the candidate" below for how and when to depart from it.

## Adapting sections to the candidate

The six-section default assumes a candidate with both solid work experience and project work to
show. Most candidates don't fit that cleanly — a first-year student may have neither; an
experienced career-changer may have strong work experience in a different field and nothing
relevant to show as a "project." Treat the section list as something you assemble per candidate
during the intake conversation (see SKILL.md's "Core workflow"), not a template you fill in
unconditionally. A few concrete rules:

- **Never render a section with nothing in it.** An empty "WORK EXPERIENCE" header looks worse
  than no header at all — it visually advertises the gap instead of just not drawing attention to
  it. If a candidate has no professional work experience yet, drop the section entirely rather than
  leaving it empty or writing filler.
- **Reorder Work Experience vs. Project Experience based on which one is the stronger evidence.**
  If the candidate has little or no relevant professional work experience relative to their project
  work (an early-career pivot, or a target role where projects are the strongest evidence), move
  Project Experience ahead of Work Experience, or drop Work Experience altogether. The Smith OCS
  rationale for Project Experience existing at all is specifically to compensate for thin work
  experience — let that same logic decide both the ordering and whether to include each section.
- **When someone has neither solid work experience nor a standout project**, lean harder on
  Education (relevant coursework, GPA if strong, case competitions) and Additional Experience and
  Leadership (clubs, volunteer work, campus involvement) — these become the primary evidence
  sections rather than the supporting ones. It's fine, and often better, for a candidate's first
  resume to be four sections long instead of six.
- **Suggest a non-standard section when it genuinely represents the candidate better than forcing
  their content into one of the six defaults.** Smith OCS's own Distinctions section already
  absorbs "whatever doesn't fit elsewhere" (languages, publications, hobbies) — if a candidate has
  enough of one kind of distinct content to warrant its own heading (a "RESEARCH EXPERIENCE"
  section for someone with multiple research stints, a "CERTIFICATIONS" section pulled out of
  Technical Skills, an "ACTIVITIES" section for a candidate whose main story is campus
  involvement), it's fine to add it. Use the same visual template (bold header with bottom border,
  then either the `entries` or `bullets` section type) — the schema isn't limited to the six named
  headings in the default order, those are just the common cases.
- **Say what you changed and why**, briefly — the candidate should understand that their resume
  doesn't have a Work Experience section because you made a deliberate call based on what they told
  you, not because something broke.

## Two-line entry blocks (education, work experience, project experience)

Each entry in Education, Work Experience, and Project Experience uses a two-line header made of
four fields split left/right:

```
<Bold, left>  Organization / School Name                 <Bold, right>  City, ST, Country
<Italic, left>  Degree / Job Title / Project Title         <Italic, right>  Month Year – Month Year
```

- Line 1 is a single paragraph with a right-aligned tab stop so the org/school name sits flush
  left and the location sits flush right on the same line, both bold.
- Line 2 is the same pattern, both italic: title (or degree, or project title) flush left, date
  range flush right.
- For Work Experience, if someone held multiple titles at one employer, repeat line 2 (title +
  dates + its own bullets) under a single shared line 1 rather than repeating the employer.
- Project Experience entries may omit the location field if there isn't a natural one (classroom
  projects usually don't have one) — just the title/date line is required.
- Date ranges use an en dash or em dash between start and end, e.g. "June 2026 – August 2026."
  Use "Present" as the end date for a current role.

## Bullets

- Plain hyphen character ("-"), not a bullet glyph.
- Left-aligned (the hyphen sits at the paragraph's left indent, not right-justified).
- The bullet text itself is **justified** alignment (both left and right edges align), matching
  the candidate's existing resume — this is a deliberate deviation from the official template's
  left-ragged body text.
- End every bullet with a period (content rule, not just visual).
- Keep each bullet to about 2–3 lines at most; longer than that, split or trim it.

## Compact sections (Additional Experience and Leadership, Distinctions)

- **Additional Experience and Leadership**: one line per entry, bold for the role/title and
  organization, with dates right-aligned same as the two-line entries above, optionally followed
  by one short indented description/bullet. This section is intentionally more compact than a full
  Work Experience block — don't give it three full bullets per entry.
- **Distinctions**: flat list of hyphen bullets, one per line, no dates — e.g., an award with its
  one-line description, or an "Interests: ..." line.

## Technical Skills

- No bullets. A short block of one or two lines, each formatted as `Bold label: plain-text items`,
  e.g. "Languages and Tools: Python (pandas, NumPy, scikit-learn), SQL, Power BI, ..." — comma
  separated, not a sub-bulleted list, to save vertical space for a one-page resume.

## One-page check

`scripts/build_resume.py` estimates whether the generated resume fits one page by estimating
wrapped line counts per paragraph against the content width and comparing total estimated height to
the page height inside the margins. This is a heuristic, not a substitute for actually opening the
file — always open the generated `.docx` (Word, or any viewer) and eyeball it, especially right
near the one-page boundary.
