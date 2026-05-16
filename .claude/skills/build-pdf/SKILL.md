---
name: build-pdf
description: Compile resume.tex and/or cover-letter.tex in an applications/<slug>/ folder with latexmk and place the renamed PDFs in out/.
---

# build-pdf

Use when the user wants to compile an application's PDFs.

## Inputs

- **Application slug** — the folder under `applications/`. If not provided, ask.
- **Targets** — `resume`, `cover-letter`, or `both` (default `both`).

## Steps

1. `cd applications/<slug>`.
2. For each target file that exists:
   - Run `latexmk -pdf -interaction=nonstopmode <file>.tex`.
   - On success, move the produced PDF to `out/<YourName>_<Company>_<Target>.pdf` where `<YourName>` is your name with no spaces and `<Company>` is derived from the slug's first segment, title-cased (e.g. `earthgenome` → `EarthGenome`, `sales-engineer` → `SalesEngineer`).
   - If only one target was built, drop the `_<Target>` suffix (e.g. `<YourName>_EarthGenome.pdf`).
3. Clean intermediate aux files (`latexmk -c`).
4. Report the absolute paths of the produced PDFs.

## On failure

- Print the last ~30 lines of the LaTeX error.
- Do NOT auto-fix LaTeX errors unless the user asks — ask first; the source is hand-tuned.

## Do not

- Do not touch base/ during builds.
- Do not commit. The user controls git.
