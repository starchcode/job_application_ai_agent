---
name: apply
description: Process job applications from an uploaded job JSON file. Parses the jobs, compares them with my CV, then walks through each job to tailor my CV and cover letter PDFs from my .tex templates. Use when I upload a .json file of jobs and call /apply.
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

**Commands:** `nx` = next step. `skip` = skip this job and go to the next one.

**Fit table (all jobs).** Compare each job with my skills and CV. Show a table sorted strongest to weakest fit, with emojis, concise:

| # | Job title (link) | Skill required | You | Fit |
|---|---|---|---|---|

The number lets me refer to or skip jobs. Then go through jobs one at a time. Always show the job as a clickable link.

**Step 1: CV plan**
1. Print the job name as a link, and say whether I should go for it.
2. Show a table: job number and link, keywords the job needs, which I have, which are missing.
3. Print my CV text from the professional summary down, showing changes like this:
   - Removals: ~~strikethrough~~
   - Additions: **bold**
   - A small number before each change, e.g. (1), so I can ask for edits.

**Step 2: CV and cover letter text**
1. Generate the CV PDF from what we agreed.
2. Print the cover letter text. Make paragraph 3 compelling for that company.
3. I may request changes to either. Show them with the same numbered strikethrough and bold format.

**Step 3:** When all is agreed, generate the cover letter PDF.

**Every marked change** gets a table giving the reason for it.

## 4. Producing files
- Copy the `.tex` templates from `reference_files/` and the project `fonts/` folder into a temp or scratch folder. Edit and compile there. Never edit `reference_files/`.
- Compile with Tectonic (`tectonic file.tex`), which uses the XeTeX engine the templates were made for. Tectonic cannot find Montserrat italics under the name `montserrat`, so in the temp copy only: copy `fonts/montserrat-*.ttf` to `fonts/mont-*.ttf` and replace `{montserrat}[` with `{mont}[` in the `.tex`. This does not change the look.
- After compiling, confirm the CV is exactly one page and the cover letter fits one page.
- Put only the finished PDFs in today's `output/YYYYMMDD/` folder, named as in `rules/cv_and_cover_letter.md`. No `.tex` files in `output/`.
- Before showing me a file: check it follows `rules/cv_and_cover_letter.md` and `MEMORY.md`.

## 5. What a good result looks like
- Accurate, one-page, ATS-friendly CV and a human-sounding cover letter, both tailored to the job and in my templates.
- Changes shown with numbered strikethrough and bold, each with a reason.
- Gaps and open questions stated plainly.
- Nothing submitted or sent without my explicit OK.

## 6. Learn
If my feedback applies to future jobs (not a one-off edit), add a short line to MEMORY.md.
