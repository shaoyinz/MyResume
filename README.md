# Resume Tailoring Workbench

A [Claude Code](https://claude.com/claude-code)–driven system for tailoring a LaTeX
resume to each job you apply to. You keep one source-of-truth profile and a few base
resumes; Claude Code skills scaffold a per-application folder, rewrite the resume to
match the job description, draft cover letters, answer screening questions, and build
the PDFs.

Every claim a skill makes is traceable to your `shared/profile.md` — the skills are
instructed to **never invent** experience, metrics, or skills.

## Prerequisites

- **Claude Code** — installed and able to open this folder.
- **A TeX distribution with `latexmk`** — [MacTeX](https://tug.org/mactex/) (macOS),
  [TeX Live](https://tug.org/texlive/) (Linux/Windows), or MiKTeX. Check with
  `latexmk --version`.
- *(Optional)* **`pdftotext`** (from [poppler](https://poppler.freedesktop.org/)) —
  a fallback for reading a PDF resume in `/extract-profile`. Claude Code reads PDFs
  natively, so this is rarely needed. `brew install poppler` on macOS.

## Quick start

1. **Clone the repo and open it in Claude Code.**

2. **Bootstrap your profile.** Run `/extract-profile` and point it at your existing
   resume (PDF, `.tex`, `.docx` exported to text, or pasted text). It produces:
   - `shared/profile.md` — your single source of truth.
   - `base/<role>/main.tex` — a starter resume in this repo's one-page layout.

   Then open `shared/profile.md` and fill in any `TODO`s the skill left (work
   authorization, GPA, languages, etc. — anything not on your old resume).

3. **Start an application.** Run `/new-application`. It asks for a company slug and a
   role base, then scaffolds `applications/<company>-<role>/` with `jd.md`,
   `notes.md`, `questions.md`, `resume.tex`, and `out/`.

4. **Paste the job description** into `applications/<slug>/jd.md`.

5. **Tailor and build.**
   - `/tailor-resume <slug>` — rewrites `resume.tex` to match the JD.
   - `/build-pdf <slug>` — compiles the PDF into `applications/<slug>/out/`.

6. **Optional extras.**
   - `/cover-letter <slug>` — drafts `cover-letter.tex`; build it with `/build-pdf`.
   - Paste screening / application-form questions into `questions.md`, then
     `/answer-questions <slug>` to draft answers into `notes.md`.

## Skills

| Skill | What it does |
|-------|--------------|
| `/extract-profile` | Reads your existing resume → populates `shared/profile.md` and a starter `base/<role>/main.tex`. Run once during setup. |
| `/new-application` | Scaffolds `applications/<slug>/` with `jd.md`, `notes.md`, `questions.md`, `resume.tex`, and `out/`. |
| `/tailor-resume` | Rewrites an application's `resume.tex` to match its `jd.md`, drawing facts only from `profile.md`. |
| `/cover-letter` | Drafts a `cover-letter.tex` for an application from the JD, resume, and profile. |
| `/answer-questions` | Drafts answers to the questions in `questions.md`, grounded in the resume and profile. |
| `/build-pdf` | Compiles `resume.tex` / `cover-letter.tex` with `latexmk` and places renamed PDFs in `out/`. |

The `jd-analyzer` agent (in `.claude/agents/`) is used internally by the tailoring
and cover-letter skills to summarize a job description.

## Repo layout

```
base/                 Base resumes, one directory per role type (e.g. data-analyst/).
  _template/main.tex  Generic one-page layout — the structural template.
shared/
  profile.template.md Blank profile template (ships publicly).
  profile.md          YOUR real profile — gitignored, never published.
  preamble.tex        Shared LaTeX preamble.
applications/
  _template/          Generic scaffolding stubs (tracked).
  <slug>/             One folder per job application (gitignored).
.claude/
  skills/             The skills listed above.
  agents/             The jd-analyzer agent.
```

## What stays private

`.gitignore` keeps your personal content out of the public repo:

- `shared/profile.md` — your real name, contact, and full work history.
- `base/*/main.tex` — base resumes hard-code your contact info (the generic
  `base/_template/main.tex` is the one exception that ships).
- `applications/*` — every real application, except `applications/_template/`.
- Build output (`out/`, `*.pdf`), local Claude Code settings, and OS metadata.

Only the templates, skills, agent, and `.gitignore` are tracked — so the public repo
is a clean, reusable workbench with no one's personal data in it.
