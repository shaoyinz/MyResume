---
name: cover-letter
description: Draft a cover-letter.tex in an applications/<slug>/ folder from the JD, the tailored resume, and shared/profile.md.
---

# cover-letter

Use when the user wants a cover letter for an application. Reads JD + resume + profile, writes `applications/<slug>/cover-letter.tex`.

## Inputs

- **Application slug** — the folder under `applications/`. If not provided, ask.

## Preconditions

- `applications/<slug>/jd.md` exists and is non-empty.
- `applications/<slug>/resume.tex` exists (preferably already tailored).
- `shared/profile.md` exists.

## Steps

1. Read `jd.md`, `resume.tex`, and `shared/profile.md`.
2. Delegate to `jd-analyzer` for keywords and what the company seems to care about (mission, product, stack). Under 200 words.
3. Draft `cover-letter.tex`:
   - 3–4 short paragraphs.
   - Paragraph 1: why this company/role specifically (use something from the JD or company context, not generic).
   - Paragraph 2–3: 1–2 concrete projects from the resume that map to the JD's top needs. Quantify when the resume does.
   - Paragraph 4: short close + availability.
   - Use the same LaTeX preamble approach as the resume (shared/preamble.tex if available, else minimal article class).
4. **Never invent.** Every claim must be traceable to profile.md or resume.tex.
5. Append a note to `applications/<slug>/notes.md` summarizing the angle taken.

## Style

- Plain professional tone. No buzzwords ("synergize", "leverage", "passionate"). No em-dashes if the user has expressed a preference against them.
- Address to "Hiring Team at <Company>" unless the JD names a recruiter.
- Keep to one page when compiled.

## Output

Confirm file path and offer `/build-pdf <slug>` next.
