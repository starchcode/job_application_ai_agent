# Job JSON Parser

How to read the job `.json` file I upload when I call `/apply`.

- The file contains one or more jobs. Parse every job in it.
- Extract for each job, where present: title, company, link, description, requirements, nice-to-haves, location, salary, deadline, how to apply.
- If a field is missing, say so and carry on with what is there.
- Keep the job's link so it can be shown as a clickable link.
- Work out the JSON structure from the file itself.

## Previously applied check
- Before the fit table, scan ALL of `output/` recursively, every `YYYYMMDD` folder of any date (not just the previous day) and every `batch/` subfolder, for two things:
  - PDF file names (`YYYYMMDD_COMPANY_JOBTITLE_cv_<name>.pdf` / `_coverletter_`): a job counts as already applied when its company and job title, lowercased with spaces and punctuation removed, match the `COMPANY_JOBTITLE` part of a file name. Allow for small title differences (e.g. extra words like "remote").
  - Earlier job `.json` files: match by job URL or job id first, then by company + title.
- Today's folder counts too, so a job that appears twice in one JSON, or is re-uploaded the same day, is flagged.
- Do not skip or drop flagged jobs. Only flag them (see `/apply` step 3a). If the match is uncertain, flag it as "possible match" and name the file.
