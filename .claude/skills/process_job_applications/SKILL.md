---
name: apply
description: Process job applications from an uploaded job JSON file. Parses the jobs, compares them with my CV, then lets me pick which jobs to review step by step and which to batch, and tailors my CV and cover letter PDFs from my .tex templates. Asks first whether to also fill the application forms in Chrome (never submits). Use when I upload a .json file of jobs and call /apply.
---

# /apply: process job applications

Usage: I call `/apply`. The job `.json` is either already in today's `output/YYYYMMDD/` folder, or I upload it in the chat (no path is given). Check the folder first; only if no `.json` is there and none is uploaded, tell me it isn't uploaded.

## 0. Ask the mode first
Before doing anything else, ask me one short question: **"With filling forms, or without?"**
- **Without:** follow this file as written, skipping everything marked "(with forms)". Do not open Chrome.
- **With:** follow this file including everything marked "(with forms)", and also read `rules/form_filling.md`. Never submit any application, ever.

## 1. Read first (every time)
1. `CLAUDE.md` (who I am, hard rules, when to ask me).
2. `MEMORY.md` (lessons from my past feedback; apply them).
3. Everything in `rules/`: `job_json_parser.md` (parsing) and `cv_and_cover_letter.md` (CV and cover letter rules, file names), plus everything in `additional_information/` (my private personal rules, e.g. which name to use). If a rule file is empty or doesn't cover a case, ask me.
4. `additional_information/about_me.md` (private extra facts about me; you may edit it when I tell you new facts) and `reference_files/` (READ-ONLY: my CV `.tex`, cover letter `.tex` template, certificates, examples). Treat my CV and `additional_information/about_me.md` together as the source of truth.

## 2. Set up
1. Create `output/YYYYMMDD/` for today. If it exists, reuse it. Never duplicate or overwrite it.
2. Look for a `.json` already in today's folder. If there is one, use it (if several, ask which). Otherwise, if I uploaded one, save it inside. If neither exists, tell me the JSON isn't uploaded and wait. Don't overwrite a same-named file there; ask me or add a numbered suffix.
3. Parse the jobs as described in `rules/job_json_parser.md`.

## 3. Workflow

**Commands:** `nx` = approve this job and go to the next one. `skip` = skip this job and go to the next one.

### 3a. Fit table
Compare each job with my skills and CV. Show one table of all jobs, sorted strongest to weakest fit, with emojis, concise:

| # | Job title (link) | Applied? | Skill required | You | Fit |
|---|---|---|---|---|---|

- **One row per job.** Each job is its own row.
- **Numbering:** number the rows 1, 2, 3 ... in table order (strongest fit first). Never use the job's position or id in the JSON file.
- **Link:** the job title is a clickable link to the job's URL from the JSON.
- The number lets me refer to jobs.
- **Applied-before flag:** run the "Previously applied check" in `rules/job_json_parser.md` against every folder in `output/`, of any date, not just the most recent one. Add a first-table column `Applied?`. For a job already applied to, show `⚠️ Applied before (YYYYMMDD)` and name the matching file(s) (CV and/or cover letter). For an uncertain match show `❓ Possible match (YYYYMMDD)`. Otherwise leave it `🆕`. Still rank flagged jobs by fit and never drop them; I decide whether to redo them.

Then **stop and wait**. Do not start any job or produce any file.

### 3b. I pick the jobs
I choose jobs by their table numbers, in one or both of these forms:
- `step by step for: <numbers>`: review these jobs one at a time (3c).
- `batch: <numbers>`: set these aside for the batch stage (3e).

The numbers are the table numbers from 3a. They change every time, so use only what I give you. (For example, `step by step for: 1, 3, 6` or `batch: 11, 15`.)

**(with forms) Open the step-by-step jobs in Chrome now**, before reviewing any text. Use one Chrome tab group, never close my tabs, and open tabs in this order for each job in my order: the job's LinkedIn tab, then the tab that "Apply" opens right beside it. For an Easy Apply job open only the LinkedIn tab and open its Easy Apply window, nothing more. Keep clicking Apply buttons until you reach the actual form fields (see `rules/form_filling.md`). Fill nothing yet. If a site needs a sign-up or sign-in, tell me and carry on with the other jobs. Check what each form asks for (cover letter field or resume only, and any questions) so the right files are made.

### 3c. Step by step (text only, no PDFs yet)
Go through my step-by-step jobs in the order I gave. For each job, one single step that shows everything, as text only. **(with forms)** Also say in one line what the form asks for (for example: resume only, no cover letter field), and make the cover letter only if the form has a field for it or the job clearly benefits from one; say so when you skip it.
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

### 3d-2. (with forms) Fill the step-by-step forms
After 3d, ask me: **"Do I go ahead and fill the forms?"** Only on my yes:
1. For each job, fill the form in its open tab and upload the tailored CV (and the cover letter where the form has a field), following `rules/form_filling.md` and my facts in `additional_information/about_me.md`.
2. Ask me a clarifying question whenever a form needs something I haven't told you. Do not guess.
3. Stop before the final Submit on every form. Never apply for me.
4. When done, tell me you are done with a short summary listing each job and its status, what is left for me, and any change you made to approved text (for example a dropped bullet to keep the CV on one page).
5. I finish leftovers, submit and close the tabs myself.

### 3e. Batch jobs
**(without forms)** After 3d, show a table of the batch jobs I listed, with clickable links:

| # | Job title (link) | Company | Fit |
|---|---|---|---|

Then wait. I reply with the numbers I want. For those jobs, produce the tailored CV and cover letter PDFs in one go, with no step-by-step review, in `output/YYYYMMDD/batch/` (create it if missing). Use the same rules, keyword mapping and fit analysis as for any job. At the end, list what was produced and any gaps or assumptions (e.g. a requirement I don't have).

**(with forms)** Do not print the batch table until I have finished the step-by-step forms and tell you so (or you ask me and I say yes). Then:
1. Print the table above, adding a column for what Apply leads to (direct form, sign-up needed, Easy Apply). Before printing, open each batch job as in 3b: its LinkedIn tab, then the tab Apply opens (or the Easy Apply window), until the form or the sign-in wall is reached. Fill nothing.
2. I reply with the numbers I want. Close the tabs you opened for the jobs I did not pick. If I pick none, close them all.
3. For the picked jobs, produce the CV and cover letter PDFs in `output/YYYYMMDD/batch/` with no step-by-step review (check each form first and make a cover letter only if it is needed), then fill the forms and upload the files as in 3d-2. If a site needs sign-up, tell me and wait.
4. Give the same end summary. I submit myself.

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
- Nothing submitted or sent without my explicit OK. Applications are never submitted by you, in either mode; I always click Submit myself.
- (with forms) A short end-of-session summary listing each job's status.

- Duplicate postings (the same role posted twice, or two jobs sharing one application form) are applied to once. Say so and skip the duplicate.
- After writing any CV edit, if the CV spills over one page, drop the least relevant bullet and tell me which one. Never shrink the layout.

## 6. Learn
If my feedback applies to future jobs (not a one-off edit), add a short line to MEMORY.md.
