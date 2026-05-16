---
name: tailor-resume
description: Adapt the resume.tex in an applications/<slug>/ folder to match the job description in jd.md, using shared/profile.md as the source of truth for facts.
---

# tailor-resume

Use when the user wants to tailor a resume to a specific JD. Operates on one application folder.

## Inputs

- **Application slug** — the folder under `applications/`. If not provided, list folders and ask which.

## Preconditions

- `applications/<slug>/jd.md` exists and is non-empty (not just placeholder).
- `applications/<slug>/resume.tex` exists.
- `shared/profile.md` exists (warn if it's still a TODO stub — tailoring quality depends on it).

If any are missing, stop and tell the user what's needed.

## Steps

1. Read `applications/<slug>/jd.md`.
2. Delegate to the `jd-analyzer` agent to extract: required skills, preferred skills, keywords, role seniority, and any disqualifiers. Keep the agent's report under 200 words.
3. Read `shared/profile.md` for the user's full inventory of facts.
4. Read the current `applications/<slug>/resume.tex`.
5. Propose edits to `resume.tex`:
   - Reorder/rewrite bullets to lead with JD-relevant work.
   - Swap in keywords from the JD where they truthfully apply (per profile.md).
   - Cut bullets that don't serve this role.
   - **Never invent experience or metrics.** Only use what's in profile.md or the existing resume.
6. Append a tailoring log to `applications/<slug>/notes.md`:
   - Date
   - Keywords surfaced from JD
   - Bullets added/removed/reordered (one-line each)
   - Anything you cut for fit that the user should know about

## Style

- **One page, always.** The final PDF must fit on a single letter-size page. If tailoring pushes content past one page, cut: drop the least-JD-relevant project, then the least-JD-relevant bullets, then tighten the Summary. Do not shrink fonts, margins, or vertical spacing to fake it — those are tuned in the base. After `/build-pdf`, verify the page count and trim further if needed before reporting done.
- LaTeX edits should be surgical. Don't reformat unchanged sections.
- Preserve the resume's existing macros and structure.
- If the JD is in a language other than the resume's language, ask before translating.

## Output

After editing, tell the user:
- Which bullets changed (one-line summary)
- Whether to run `/build-pdf <slug>` next
