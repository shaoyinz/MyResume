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
3. **Verify the resume is exactly one page** (skip if `resume` was not a target). After the PDF is in `out/`:
   ```bash
   PAGES=$(mdls -name kMDItemNumberOfPages -raw out/<the-resume-pdf>.pdf)
   if [ "$PAGES" != "1" ]; then
     echo "❌ Resume is $PAGES pages, must be 1"
     exit 1
   fi
   echo "✅ resume is 1 page"
   ```
   This is a hard rule — resumes must fit on one page. Do not paper over a failure here.
4. Clean up. The canonical PDFs are already in `out/`, so do a strong clean of the source folder and the `/tmp` preview byproducts:
   - `latexmk -C` (removes aux files **and** the leftover `.pdf` in the source folder).
   - `rm -f /tmp/${slug}_*.png /tmp/${slug}_pg*` for any `sips`/`pdftoppm` previews generated during page-fit inspection.
5. Report the absolute paths of the produced PDFs.

## On failure

- **LaTeX error** — print the last ~30 lines of the LaTeX error. Do NOT auto-fix unless the user asks — the source is hand-tuned.
- **Page-fit failure** (resume > 1 page) — surface the failure to the user with the page count and stop. Do not silently re-tailor or shrink margins; trimming is the user's call.

## Do not

- Do not touch base/ during builds.
- Do not commit. The user controls git.
