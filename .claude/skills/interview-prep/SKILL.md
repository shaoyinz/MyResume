---
name: interview-prep
description: Prepare for an interview for an applications/<slug>/ folder. Analyzes the outreach email in outreach.md, researches the company's interview process on the web, and drafts likely questions with answers grounded in resume.tex and shared/profile.md. Writes interview-prep.md.
---

# interview-prep

Use when the user has an interview scheduled and wants to prepare. Analyzes the
recruiter/interviewer outreach email, researches the company's interview process
online, and drafts likely questions with answers traceable to the user's materials.
Writes everything to `applications/<slug>/interview-prep.md`.

## Inputs

- **Application slug** — the folder under `applications/`. If not provided, list
  folders and ask which.

## Preconditions

- `applications/<slug>/outreach.md` exists and contains a real pasted email (not just
  the stub comment). If it still only contains the hint comment, stop and tell the
  user to paste the outreach email into it first. If `outreach.md` does not exist at
  all (older folder predating the stub), create it from `applications/_template/`
  and ask the user to paste the email.
- `applications/<slug>/jd.md` exists and is non-empty.
- `applications/<slug>/resume.tex` exists (ideally already tailored).
- `shared/profile.md` exists.

If any are missing, stop and tell the user what's needed.

## Steps

1. Read `applications/<slug>/{outreach.md, jd.md, resume.tex, notes.md}` and
   `shared/profile.md`.

2. **Analyze the outreach email.** From `outreach.md`, extract:
   - **Round** — recruiter screen, hiring-manager call, technical, take-home, panel,
     onsite, final, etc.
   - **Format** — phone / video (which platform) / in-person; live coding vs.
     conversational.
   - **Interviewer(s)** — name(s) and title(s) of anyone the email names as
     conducting or attending the interview.
   - **Logistics** — date, time, time zone, duration, location/link, what to bring.
   - **Prep instructions** — anything the email explicitly asks the candidate to
     prepare, review, or complete beforehand.
   If the email is vague on any of these, note it as unknown rather than guessing.

3. **Research the company.** Delegate to the `interview-researcher` agent, passing
   the company name, the role title (from `jd.md`), and the interviewer name(s)
   extracted in step 2. It returns interview-process notes, reported questions, and
   per-interviewer background. If it reports `[no reliable sources found]`, proceed
   using the JD and email alone and say so in the output.

4. **Draft likely questions.** Build a question set appropriate to the round
   identified in step 2, drawing on the JD's required skills, the researcher's
   reported questions, and the role type. Group them:
   - **Behavioral** — "tell us about a time...", teamwork, conflict, failure.
   - **Technical / role-specific** — questions on the skills and tools the JD names.
   - **Company / role fit** — why this company, why this role, what you know about
     the product.
   Aim for a focused set (roughly 6–12), not an exhaustive dump. A recruiter screen
   skews toward fit and logistics; a technical round skews toward role-specific.

5. **Answer each question** from `resume.tex` and `profile.md` only:
   - Behavioral answers use a compact STAR shape (situation, task, action, result)
     written as a coherent paragraph — don't label the parts.
   - Technical answers point to the specific project or skill on the resume that
     demonstrates the competency.
   - Fit answers draw on the company angle plus one concrete resume project that
     maps to it.
   - **Never invent.** Every claim must be traceable to `profile.md` or `resume.tex`.
     If a question can't be answered from those sources, write
     `[NEEDS USER INPUT: <what's missing>]` instead of guessing.

6. **Draft questions for the candidate to ask** — 3–5 thoughtful questions the user
   could ask the interviewer(s), informed by the JD, the company research, and the
   named interviewers' backgrounds.

7. Write `applications/<slug>/interview-prep.md` with this structure:

   ```
   # Interview Prep — <company> / <role>
   _Prepared <YYYY-MM-DD>_

   ## Outreach summary
   - Round: ...
   - Format: ...
   - Interviewer(s): ...
   - Logistics: ...
   - Prep instructions: ...

   ## Logistics checklist
   <actionable items the user must handle before the interview>

   ## Interviewer notes
   <per-interviewer background and common ground; omit if none named>

   ## Company interview process
   <what the research surfaced, with staleness noted>

   ## Likely questions & drafted answers

   ### Behavioral
   **Q: <question>**
   <drafted answer>
   _Sources: profile.md (<fact>), resume.tex (<bullet>)_

   ### Technical / role-specific
   ...

   ### Company / role fit
   ...

   ## Questions to ask them
   <3–5 questions>

   ## Sources
   <URLs from the researcher>
   ```

   Overwrite `interview-prep.md` if it already exists (re-running refreshes it), but
   tell the user you did.

8. Print to chat a short summary: the round/format/date, how many questions drafted,
   and any `[NEEDS USER INPUT]` flags the user must resolve before the interview.

## Style

- First person for the answers, plain professional tone. No buzzwords ("synergize",
  "leverage", "passionate", "deeply"). No em-dashes if the user has expressed a
  preference against them.
- Match the resume's voice. If the resume quantifies impact, so should the answers.
- Answers are drafts to rehearse from, not scripts to memorize — keep them natural.

## Do not

- Do not invent metrics, employers, dates, technologies, interview rounds, or
  interviewer details. If a source lacks it, flag it or say it's unknown.
- Do not edit `resume.tex`, `jd.md`, `questions.md`, `outreach.md`, or
  `shared/profile.md`. This skill is write-only against `interview-prep.md`.
- Do not commit. The user controls git.

## Output

Confirm the `interview-prep.md` path and print the summary from step 8.
