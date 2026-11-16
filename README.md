# Job Application Agent

A Claude Code agent that tailors your CV and cover letter to each job, using your own reference files. You review and approve every change. It never applies, submits or sends anything for you.

## How it works
1. You give it a JSON file of jobs. If your jobs are saved on LinkedIn, `/extract-linkedin-jobs` can build that file for you.
2. It ranks the jobs against your CV and shows a numbered table.
3. You choose which jobs to review step by step, and which to batch.
4. For each job it suggests changes to your CV and a cover letter. You approve or edit them.
5. It compiles the final PDFs into a dated folder in `output/`.
6. Optional: it can also open each job in Chrome and fill the application forms for you, upload the CV and stop before the final Submit. You always click Submit yourself.

Everything it writes about you comes from your files. It will not invent skills, experience or facts. If a job needs something you don't have, it tells you instead.

## Set up (once)
1. Install [Claude Code](https://claude.com/claude-code) and open this folder in it.
2. Install a LaTeX engine. On macOS: `brew install tectonic`.
3. Optional, only for `/extract-linkedin-jobs`: install the Claude in Chrome extension and sign in to LinkedIn in Chrome.
4. Add the folders and files below. They are git-ignored because they hold personal data, so after cloning you create them yourself.

### Files you need to add

| Where | What |
|---|---|
| `reference_files/` | Your CV as a `.tex` file, and your cover letter as a `.tex` template with placeholders (company, job title, why you want to join). Optionally your certificates (PDF) and any example applications. The agent only reads these and never changes them. |
| `additional_information/about_me.md` | Private facts that are not in your CV: extra skills, certificates, work rights, achievements. The agent adds new facts here when you tell it. |
| `additional_information/personal_rules.md` | Optional. Any personal rules |
| `MEMORY.md` | Create it with one line: `Only save feedback that applies to future jobs, not one-off edits.` The agent adds lessons from your feedback. |
| `fonts/` | The fonts your `.tex` files use. The included templates use Open Sans and Montserrat `.ttf` files. They are open source and tracked in git. |

Your `.tex` files must compile with XeLaTeX (they use `fontspec`) and load fonts from `./fonts/`. Keep the CV to one page.

`output/` is created automatically.

## Get your jobs from LinkedIn (optional)
Skip this if you already have a jobs `.json` file.
1. In Chrome, open your LinkedIn job tracker: `https://www.linkedin.com/jobs-tracker/`.
2. In Claude Code, type `/extract-linkedin-jobs`. By default it exports the **Saved** tab. Name another tab (In Progress, Applied, Archived) if you want it; the agent asks before doing a large one such as Applied.
3. The agent goes through every page of the tab and opens each job in a separate Chrome tab. Your tracker tab stays open and untouched, and nothing on LinkedIn is changed. If LinkedIn asks you to sign in, do it yourself.
4. It saves `output/YYYYMMDD/linkedin_<tab>_jobs.json` (for example `linkedin_saved_jobs.json`). Each job has `company_name`, `job_title`, `url`, `job_location`, `job_requirements`, `other_description` and `employment_type`.
5. Job descriptions are condensed and reworded, not copied word for word. Check the original posting for anything important such as salary or eligibility.
6. Upload that file with `/apply` (see below).

## Use it
1. Get your jobs into a `.json` file (from `/extract-linkedin-jobs` or your own). Each job should have at least a title, company, link and description.
2. In Claude Code, type `/apply` and attach the JSON file to the message. The agent first asks whether to work **with filling forms** (Chrome) or **without**.
3. The agent saves the JSON in `output/YYYYMMDD/` (today's date) and shows a table of all jobs, strongest fit first, numbered 1, 2, 3 and so on.
4. Reply with the numbers you want, for example:
   ```
   step by step for: 1, 3, 6
   batch: 8, 9
   ```
   The numbers above are only examples. Use the numbers from your table.
5. **Step by step jobs:** for each job the agent shows, in one message, whether to go for it, the keywords you have and miss, suggested CV changes and a draft cover letter. Removals appear as ~~strikethrough~~ and additions as `inline code`, each with a number and a reason. Ask for changes by number, for example "change 3 to ...".
   - `nx` approves the job and moves to the next one.
   - `skip` drops the job.
6. When the series is finished, the agent produces all the CV and cover letter PDFs in `output/YYYYMMDD/`.
7. **Batch jobs:** the agent shows a table of your batch jobs. Reply with the numbers you want and it produces their PDFs in `output/YYYYMMDD/batch/` without step by step review.
8. Apply to the jobs yourself using the PDFs.

### With filling forms (optional mode)
Needs the Claude in Chrome extension. After you pick the jobs, the agent opens each step-by-step job in Chrome, one LinkedIn tab followed by the tab its Apply button opens, and goes on until it reaches the real form (Easy Apply jobs stay in LinkedIn). You review the CV and cover letter text as usual. When the files are made the agent asks for your consent, then fills the forms, uploads the tailored CV and cover letter where a form has a field for it, and asks you questions it can't answer from your files. It stops before the final Submit and gives a short status summary for every job. You finish any leftovers, submit and close the tabs. Only then does it show the batch jobs table and open those jobs the same way, so you can pick which to prepare. If a site needs an account, the agent stops and you sign up yourself. The details are in `rules/form_filling.md`. Standing answers (address, citizenship, salary wording and so on) are kept in your private `additional_information/about_me.md`.

## Rules for the CV and cover letter
- Only facts from your reference files and `additional_information/` are used. Nothing is invented.
- The CV layout does not change and stays on one page. Keywords from the job description are used where they truthfully fit.
- The cover letter has 4 short paragraphs (about 170 words) and is written from the facts in your reference files, with a paragraph on why you want to join that company.
- You approve every change before a PDF is made, except in batch mode, where the agent tailors the files directly and lists any gaps or assumptions at the end. Review those files before you send them.
- Files are named `YYYYMMDD_COMPANYNAME_JOBTITLE_cv_<name>.pdf` and `..._coverletter_<name>.pdf`.

## Folder guide
```
CLAUDE.md                      Agent instructions (always read first)
MEMORY.md                      Lessons from your feedback (git-ignored)
.claude/skills/process_job_applications/SKILL.md   The /apply workflow
.claude/skills/extract-linkedin-jobs/SKILL.md      The /extract-linkedin-jobs workflow
rules/                         Rules for parsing jobs, writing the CV and cover letter, and filling forms in Chrome
additional_information/        Your private facts and personal rules (git-ignored)
reference_files/               Your CV, cover letter template, certificates (git-ignored, read-only)
fonts/                         Fonts used by the templates
output/YYYYMMDD/               Job JSON (uploaded or extracted from LinkedIn) and PDFs for that day (git-ignored)
output/YYYYMMDD/batch/         PDFs for batch jobs
```

## Customise
- Change how documents are written by editing the files in `rules/`.
- Give feedback in the chat. Feedback that applies to future jobs is saved to `MEMORY.md`.
