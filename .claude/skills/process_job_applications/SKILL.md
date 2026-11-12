---
name: apply
description: Process job applications from an uploaded job JSON file. Parses the jobs, compares them with my CV, then lets me pick which jobs to review step by step and which to batch, and tailors my CV and cover letter PDFs from my .tex templates. Use when I upload a .json file of jobs and call /apply.
---

# /apply: process job applications

Usage: I call `/apply` and upload the job `.json` file in the chat (no path is given).

## 1. Read first (every time)
1. `CLAUDE.md` (who I am, hard rules, when to ask me).
2. `MEMORY.md` (lessons from my past feedback; apply them).
3. Everything in `rules/`: `job_json_parser.md` (parsing) and `cv_and_cover_letter.md` (CV and cover letter rules, file names), plus everything in `additional_information/` (my private personal rules, e.g. which name to use). If a rule file is empty or doesn't cover a case, ask me.
4. `additional_information/about_me.md` (private extra facts about me; you may edit it when I tell you new facts) and `reference_files/` (READ-ONLY: my CV `.tex`, cover letter `.tex` template, certificates, examples). Treat my CV and `additional_information/about_me.md` together as the source of truth.

## 2. Set up
1. Create `output/YYYYMMDD/` for today. If it exists, reuse it. Never duplicate or overwrite it.
2. Save the uploaded JSON inside it. Don't overwrite a same-named file there; ask me or add a numbered suffix.
3. Parse the jobs as described in `rules/job_json_parser.md`.

## 3. Workflow

**Commands:** `nx` = approve this job and go to the next one. `skip` = skip this job and go to the next one.

### 3a. Fit table
Compare each job with my skills and CV. Show one table of all jobs, sorted strongest to weakest fit, with emojis, concise:

| # | Job title (link) | Skill required | You | Fit |
|---|---|---|---|---|

- **One row per job.** Each job is its own row.
- **Numbering:** number the rows 1, 2, 3 ... in table order (strongest fit first). Never use the job's position or id in the JSON file.
- **Link:** the job title is a clickable link to the job's URL from the JSON.
- The number lets me refer to jobs.

Then **stop and wait**. Do not start any job or produce any file.

### 3b. I pick the jobs
I choose jobs by their table numbers, in one or both of these forms:
- `step by step for: <numbers>`: review these jobs one at a time (3c).
- `batch: <numbers>`: set these aside for the batch stage (3e).

The numbers are the table numbers from 3a. They change every time, so use only what I give you. (For example, `step by step for: 1, 3, 6` or `batch: 11, 15`.)

### 3c. Step by step (text only, no PDFs yet)
Go through my step-by-step jobs in the order I gave. For each job, one single step that shows everything, as text only:
1. Print the job name as a link, and say whether I should go for it.
2. Show a table: job number and link, keywords the job needs, which I have, which are missing.
3. Print my CV text from the professional summary down, showing changes like this:
   - Removals: ~~strikethrough~~
   - Additions: `inline code ticks` (bold is hard to read, never use bold for additions)
   - A small number before each change, e.g. (1), so I can ask for edits.
4. Print the cover letter text below it. Make paragraph 3 compelling for that company. Use the same numbered strikethrough and inline code tick format for any changes.

I may ask for changes to the CV or the cover letter. Apply them and show them with the same numbered format. `nx` approves the job and moves to the next one. `skip` drops the job. Keep the final agreed CV and cover letter text for each job (in the scratch folder) so it can be used in 3d. **Every marked change** gets a table giving the reason for it.

### 3d. Produce the step-by-step files (all at once)
When every step-by-step job is finished (or skipped), produce the CV PDF and cover letter PDF for each job I approved, all in one go, in today's `output/YYYYMMDD/` folder.

### 3e. Batch jobs
After 3d, show a table of the batch jobs I listed, with clickable links:

| # | Job title (link) | Company | Fit |
|---|---|---|---|

Then wait. I reply with the numbers I want. For those jobs, produce the tailored CV and cover letter PDFs in one go, with no step-by-step review, in `output/YYYYMMDD/batch/` (create it if missing). Use the same rules, keyword mapping and fit analysis as for any job. At the end, list what was produced and any gaps or assumptions (e.g. a requirement I don't have).

## 4. Producing files
- Copy the `.tex` templates from `reference_files/` and the project `fonts/` folder into a temp or scratch folder. Edit and compile there. Never edit `reference_files/`.
- Compile with Tectonic (`tectonic file.tex`), which uses the XeTeX engine the templates were made for. Tectonic cannot find Montserrat italics under the name `montserrat`, so in the temp copy only: copy `fonts/montserrat-*.ttf` to `fonts/mont-*.ttf` and replace `{montserrat}[` with `{mont}[` in the `.tex`. This does not change the look.
- After compiling, confirm the CV is exactly one page and the cover letter fits one page.
- Put only the finished PDFs in today's `output/YYYYMMDD/` folder (batch jobs in `output/YYYYMMDD/batch/`), named as in `rules/cv_and_cover_letter.md`. No `.tex` files in `output/`.
- Before showing me a file: check it follows `rules/cv_and_cover_letter.md` and `MEMORY.md`.

## 5. What a good result looks like
- Accurate, one-page, ATS-friendly CV and a human-sounding cover letter, both tailored to the job and in my templates.
- Changes shown with numbered strikethrough and inline code ticks, each with a reason.
- Gaps and open questions stated plainly.
- Nothing submitted or sent without my explicit OK.

## 6. Learn
If my feedback applies to future jobs (not a one-off edit), add a short line to MEMORY.md.
