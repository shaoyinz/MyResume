---
name: new-application
description: Scaffold a new job application folder under applications/ with jd.md, notes.md, and a resume.tex copied from a chosen base/ role.
---

# new-application

Use when the user is starting a new job application. Creates `applications/<slug>/` with:

- `jd.md` — empty stub for pasting the job description
- `notes.md` — empty stub for tailoring decisions
- `resume.tex` — copied from the chosen base
- `out/` — empty directory for built PDFs

## Inputs

Ask the user (one at a time, only if not provided):

1. **Company** — short slug, lowercase, e.g. `earthgenome`, `wsp`.
2. **Role base** — one of: `data-analyst`, `gis-analyst`, `gis-analyst-ng`, `chinese`. Look in `base/` for the canonical list.
3. **Folder slug** — default `<company>-<role-base>` (e.g. `earthgenome-data-analyst`). Confirm before creating.

## Steps

1. Verify `base/<role>/` exists; list available bases if not.
2. Verify `applications/<slug>/` does NOT already exist. If it does, ask before overwriting.
3. Create `applications/<slug>/{jd.md,notes.md,out/}`.
4. Copy `base/<role>/main.tex` to `applications/<slug>/resume.tex` (note: for `chinese`, the source files are `resume-en.tex`/`resume-zh.tex` and `resume.cls` — copy all three).
5. Print the folder path and tell the user to paste the JD into `jd.md`, then invoke `/tailor-resume <slug>`.

## Do not

- Do not pre-fill jd.md with placeholder content beyond a single comment line.
- Do not modify base/ files. Bases are read-only templates.
