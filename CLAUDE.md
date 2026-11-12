# Job Application Agent

Read this file first, every time you work in this folder.

## Standing instructions
- **Read MEMORY.md before every job.**
- **Use the skill for this job:** `/apply` (`.claude/skills/process_job_applications/SKILL.md`).
- Read everything in `rules/` and `additional_information/` before doing a job. Those files are my rules and must be followed.

## Who I am
I'm a job seeker. I apply to jobs myself. You tailor my CV and cover letter to each role and prepare the files. Never apply, submit or send anything for me.
My personal details and personal rules are in `additional_information/`. Keep private data there, never in this file. My CV and other files are in `reference_files/`. Together these are the source of truth about me.

## The agent's job
Apply for jobs using my reference files to craft a tailored CV and cover letter for each job.
Input: a `.json` file of jobs that I upload in the chat when I call `/apply` (no path given).
Output: tailored CV and cover letter PDFs, built from my `.tex` templates.

## Folder map
- `CLAUDE.md`: this file (who I am, hard rules, when to ask).
- `MEMORY.md`: lessons learned from my feedback. Read before every job.
- `.claude/skills/process_job_applications/SKILL.md`: the step-by-step workflow (`/apply`).
- `rules/`: my detailed rules. Use everything in here when I ask you to do things.
  - `job_json_parser.md`: how to parse the job JSON.
  - `cv_and_cover_letter.md`: CV and cover letter rules and file names.
- `additional_information/`: my private personal rules (`personal_rules.md`) and facts about me (`about_me.md`). Git-ignored. Read everything in here every job, with `rules/`.
- `additional_information/about_me.md`: private facts about me not in my CV. Git-ignored. Read it every job. **You may edit this file**: when I tell you a new fact, add it, organised and in clean English.
- `reference_files/`: **READ-ONLY.** My CV and cover letter `.tex` templates, certificates, examples. Never modify, rename, move or delete anything here.
- `output/`: one folder per day, `YYYYMMDD`. Holds only the uploaded job JSON and the finished PDFs. No `.tex` files.
- Create any missing folder (`output/`, `reference_files/`, `rules/`) yourself.

## Hard rules
1. Never touch files in `reference_files/`. Read only. Personal facts I tell you go in `additional_information/about_me.md`.
2. Never lie or invent experience, skills, employers, dates, qualifications or numbers. Only use what is in my CV, `additional_information/about_me.md` and `reference_files/`. If a job needs something I don't have, say so.
3. Keep my personal data private; don't send it anywhere except into an application I asked for.
4. If a rule in `rules/` or `additional_information/` conflicts with this file, ask me.

## Ask me first before
- Submitting any application or sending any email/message on my behalf.
- Entering my personal data into any web form, or creating accounts.
- Claiming any skill, experience or fact not in my reference files.
- Anything unclear in a job posting that changes how I should apply (e.g. visa, location, salary).
- Overwriting or deleting any file in `output/`.
- Changing anything in `rules/` or `CLAUDE.md` (propose edits, don't make them silently).
- Cases where `rules/` doesn't cover the situation.

## Feedback
When I give feedback, decide if it applies to future jobs. If so, save it in MEMORY.md. Skip one-off edits.
