---
name: answer-questions
description: Draft answers to application-form / screening questions for an applications/<slug>/ folder, grounded in the tailored resume.tex, jd.md, and shared/profile.md. Appends drafts to notes.md.
---

# answer-questions

Use when the user has finished tailoring a resume and now needs to write answers to job-application questions (screening forms, "tell us about a time...", "why this role", etc.). Reads JD + tailored resume + profile + questions, appends drafted answers to `applications/<slug>/notes.md`.

Avoid using dash - in the middle of a sentence.

## Inputs

- **Application slug** — the folder under `applications/`. If not provided, list folders and ask which.

## Preconditions

- `applications/<slug>/questions.md` exists with real questions in it. `new-application` scaffolds this file as a stub — if it still only contains the hint comment, stop and tell the user to paste their screening questions into it first.
- `applications/<slug>/resume.tex` exists (ideally already tailored — warn if it looks untouched vs. the base).
- `applications/<slug>/jd.md` exists and is non-empty.
- `shared/profile.md` exists.

If any are missing, stop and tell the user what's needed. If `questions.md` does not exist at all (older folder predating the stub), create the stub from `applications/_template/questions.md` and ask the user to paste their questions.

## Question format

`questions.md` is plain markdown. Each question is its own block. The user may annotate length limits inline:

```
Q1 (300 chars): Why are you interested in this role?
Q2 (150 words): Describe a project where you used GIS to drive a decision.
Q3: What is your salary expectation?
```

Parse `(N chars)` or `(N words)` as a hard limit for that answer. If no annotation, draft without a limit (full answer).

## Steps

1. Read `applications/<slug>/{questions.md, jd.md, resume.tex, notes.md}` and `shared/profile.md`.
2. Delegate to `jd-analyzer` for keywords + company angle. Under 200 words. (Used to keep answers JD-aligned.)
3. For each question:
   - Identify which resume bullets and profile facts most directly answer it.
   - Draft an answer in plain professional tone, first person, no buzzwords.
   - Respect the per-question length limit if specified. Count characters/words and stay within the cap.
   - **Never invent.** Every claim must be traceable to `profile.md` or `resume.tex`. If the question cannot be answered from those sources (e.g. salary expectation, visa status not in profile), flag it as `[NEEDS USER INPUT: <what's missing>]` instead of guessing.
4. Append a new section to `applications/<slug>/notes.md`:

   ```
   ## Application questions — <YYYY-MM-DD>

   ### Q1: <question text>
   <drafted answer>
   _Sources: profile.md (<which fact>), resume.tex (<which bullet>)_

   ### Q2: ...
   ```

   Sources line is one short line per answer — which profile facts or resume bullets it draws from. Helps the user verify nothing was fabricated.
5. After appending, print to chat a short index of the questions answered + any `[NEEDS USER INPUT]` flags, so the user knows what still requires their input before submitting.

## Style

- First person, plain professional tone. No buzzwords ("synergize", "leverage", "passionate", "deeply"). No em-dashes if the user has expressed a preference against them.
- Match the resume's voice. If the resume quantifies impact, so should the answers.
- For behavioral questions ("tell us about a time..."), use a compact STAR shape (situation, task, action, result) but don't label the parts — just write it as a coherent paragraph.
- For "why this company / role" questions, draw on the JD-analyzer's company angle plus one concrete project from the resume that maps to it.
- If a question asks something the resume already covers (e.g. "describe your most relevant experience"), don't just copy the bullet — expand it into a narrative.

## Do not

- Do not invent metrics, employers, dates, technologies, or outcomes. If profile.md lacks the detail, flag it.
- Do not edit `resume.tex`, `jd.md`, `questions.md`, or `shared/profile.md`. This skill is write-only against `notes.md`.
- Do not commit. The user controls git.
- Do not exceed a per-question length cap. If the answer doesn't fit, cut content, don't shrink the cap.

## Output

Confirm `notes.md` path and print the index of answered questions + any `[NEEDS USER INPUT]` flags.
