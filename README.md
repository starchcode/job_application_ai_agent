# Job Application Agent

A Claude Code agent that tailors your CV and cover letter to each job, using your own reference files. You review and approve every change. It never applies, submits or sends anything for you.

## How it works
1. You give it a JSON file of jobs.
2. It ranks the jobs against your CV and shows a numbered table.
3. You choose which jobs to review step by step, and which to batch.
4. For each job it suggests changes to your CV and a cover letter. You approve or edit them.
5. It compiles the final PDFs into a dated folder in `output/`.

Everything it writes about you comes from your files. It will not invent skills, experience or facts. If a job needs something you don't have, it tells you instead.

## Set up (once)
1. Install [Claude Code](https://claude.com/claude-code) and open this folder in it.
2. Install a LaTeX engine. On macOS: `brew install tectonic`.
3. Add the folders and files below. They are git-ignored because they hold personal data, so after cloning you create them yourself.

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

## Use it
1. Get your jobs into a `.json` file. Each job should have at least a title, company, link and description.
2. In Claude Code, type `/apply` and attach the JSON file to the message.
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
rules/                         Rules for parsing jobs and writing the CV and cover letter
additional_information/        Your private facts and personal rules (git-ignored)
reference_files/               Your CV, cover letter template, certificates (git-ignored, read-only)
fonts/                         Fonts used by the templates
output/YYYYMMDD/               JSON and PDFs for that day (git-ignored)
output/YYYYMMDD/batch/         PDFs for batch jobs
```

## Customise
- Change how documents are written by editing the files in `rules/`.
- Give feedback in the chat. Feedback that applies to future jobs is saved to `MEMORY.md`.
