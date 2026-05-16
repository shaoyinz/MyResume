---
name: extract-profile
description: Bootstrap a newcomer by reading their existing resume and producing a populated shared/profile.md plus a starter base/<role>/main.tex. Run this once before the first application.
---

# extract-profile

Use this the first time someone sets up the repo. It turns an existing resume into the
two files the other skills depend on:

- `shared/profile.md` — the single source of truth for all facts about the user.
- `base/<role>/main.tex` — a starter resume in the repo's LaTeX layout.

## Inputs

Ask the user (only if not provided):

1. **Resume source** — a path to their existing resume (`.pdf`, `.tex`, `.md`, `.txt`,
   or an image) or the resume text pasted directly into chat.
2. **Role slug** — a short lowercase name for the base resume, e.g. `data-analyst`,
   `gis-analyst`. Used as the `base/<role>/` directory name. Confirm before creating.

## Preconditions

- `shared/profile.template.md` exists (structure to follow).
- `base/_template/main.tex` exists (layout to copy).
- If `shared/profile.md` already exists, **stop and warn** — do not overwrite without
  explicit confirmation. The user may already have a populated profile.

## Steps

1. **Read the resume.**
   - `.pdf` → read it with the Read tool (it reads PDFs natively via the `pages`
     parameter). If that fails and `pdftotext` is installed, fall back to
     `pdftotext -layout <file> -`.
   - `.tex` / `.md` / `.txt` → read the file directly.
   - image → read it with the Read tool.
   - `.docx` → ask the user to paste the text or export to PDF (no reliable reader).
   - pasted text → use it directly.
2. **Extract** contact info, summary, education, experience (reverse-chronological,
   with bullets and tech stacks), projects, and skills.
3. **Write `shared/profile.md`** following the section structure of
   `shared/profile.template.md`. Fill every field you can support from the resume.
   Where the resume is silent (e.g. work authorization, GPA, languages), leave the
   `TODO` placeholder in place so the user knows to complete it.
   - **Never invent.** No metrics, employers, dates, or skills that aren't in the
     source resume.
   - For the Summary section, draft at least one variant matching the user's apparent
     target role; add more variants only if the resume clearly spans role types.
4. **Create the base resume.** Confirm `base/<role>/` does not already exist, then
   copy `base/_template/main.tex` to `base/<role>/main.tex` and populate the header
   (name, role title, email, phone, links), Summary, Professional Experience,
   Education, Projects, and Technical Skills from the extracted content.
   - Preserve the LaTeX macros and environments from the template exactly.
   - Keep it to **one letter-size page**. If content overflows, cut the least
     important bullets/projects — do not shrink fonts, margins, or spacing.
5. **Report** the two written paths and tell the user to:
   - Review `shared/profile.md` and fill any remaining `TODO`s.
   - Run `/new-application` to start their first tailored application.

## Do not

- Do not overwrite an existing `shared/profile.md` or `base/<role>/main.tex` without
  asking first.
- Do not invent facts, metrics, dates, or skills not present in the source resume.
- Do not edit `base/_template/main.tex` or `shared/profile.template.md` — they are
  read-only templates.
- Do not commit. The user controls git.

## Output

Confirm the paths of `shared/profile.md` and `base/<role>/main.tex`, list which
`TODO`s still need the user's input, and point them to `/new-application`.
