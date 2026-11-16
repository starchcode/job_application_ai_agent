---
name: extract-linkedin-jobs
description: Extract every job from my LinkedIn job tracker (Saved tab by default, all pages) into one .json file in output/YYYYMMDD/ that I can feed to /apply. Uses Claude in Chrome, opens each job in a separate tab and never touches my open tracker tab. Use when I call /extract-linkedin-jobs or ask to export my saved LinkedIn jobs to JSON.
---

# /extract-linkedin-jobs: export my LinkedIn tracker jobs to JSON

Usage: I call `/extract-linkedin-jobs`. I have `https://www.linkedin.com/jobs-tracker/` open in Chrome. Optionally I name another tab (Saved, In Progress, Applied, Archived). Default is **Saved**. Applied can be hundreds of jobs: confirm before doing it.

## 1. Read first
`CLAUDE.md`, `MEMORY.md`, and `rules/job_json_parser.md` (the output is meant to be uploaded to `/apply`). Read only. This skill reads LinkedIn and writes one JSON file. It never applies, saves, unsaves, messages or clicks anything that changes my LinkedIn account.

## 2. Browser rules
- Use Claude in Chrome (`mcp__claude-in-chrome__*`). Load all needed tools in one ToolSearch call: `tabs_context_mcp, navigate, get_page_text, read_page, find, javascript_tool, computer, tabs_create_mcp, tabs_close_mcp, browser_batch`.
- **Never close or navigate my own tracker tab.** If the session cannot see it, open your own tab on the tracker URL (`tabs_context_mcp` with `createIfEmpty: true`), use a second tab for the job pages, and close the job tab when finished. Leave the tracker tab open.
- If a page is a login or checkpoint page, stop and ask me to sign in. Never enter credentials.

## 3. Collect the job links
1. In the tracker tab, note the count on the chosen tab (e.g. "Saved · 34").
2. Collect `a[href*="/jobs/view/"]` links with `javascript_tool` (strip the query string). The list is paginated (about 10 per page): scroll down, click `Page 2`, `Page 3`, `Next` and so on until the collected total equals the tab count. Keep clicking by page number buttons through JS; combine pages into a set of job ids.
3. Do not stop at page 1. If the total does not match the tab count, say so.

## 4. Read each job
- For each job: `navigate` to `https://www.linkedin.com/jobs/view/<id>/`, `wait` 3 s, then `get_page_text`. Put 3 to 4 jobs in one `browser_batch` call (navigate, wait, get_page_text, repeat). Large batch outputs are saved to a file; post-process those with a small python script (cut from "About the job" to "… more" / "Set alert for similar jobs", and the "About the company" block) so only the relevant text is read.
- Do NOT try to fetch job HTML with `fetch()` or iframes (the page is client-rendered and iframes are throttled). Do NOT post data to localhost from the page (LinkedIn's CSP blocks it). `javascript_tool` output is capped at about 700 characters, so use it only for links and small values.
- From the header take: company, job title, location line, work mode (On-site / Hybrid / Remote), type (Full-time / Contract), salary if shown.

## 5. Build the JSON
One array, one object per job, in tracker order. Keys:

```
company_name        string
job_title           string
url                 string  https://www.linkedin.com/jobs/view/<id>/
job_location        { "country": string, "city": string }
job_requirements    array of strings (qualifications, skills, experience, must-haves, nice-to-haves)
other_description   string (company about, role purpose, duties, salary, benefits, schedule, everything else)
employment_type     string (e.g. "Hybrid, Full-time", "Remote, Contract")
```

Rules:
- Include all of the description's essence. Reword and condense (copyright); never invent facts. If something is not stated (city, salary, requirements), say "Not specified" or leave it out. Do not guess.
- Keep salary, day rate, schedule (shifts, on-call, travel), eligibility (right to work, driving licence) and onsite days: they affect whether I apply.
- Duplicate postings (same company and title, different URL) stay as separate entries.
- Keep `url` exact: `/apply` uses it to flag jobs I already applied to.
- Save progress as you go in small part files in the scratchpad directory (a part file per batch), so a long run is not lost. Merge and sort with python, check the count against the tracker count, check for duplicate or missing ids.

## 6. Save
1. Create `output/YYYYMMDD/` for today if missing; reuse it if it exists.
2. Write `output/YYYYMMDD/linkedin_<tab>_jobs.json` (e.g. `linkedin_saved_jobs.json`), UTF-8, indented, `ensure_ascii=False`. If the file exists, ask me before overwriting or add a numbered suffix.
3. No download prompt and no `.tex` files. The file is a local clickable link.

## 7. Report
Short summary: how many jobs, which tab and pages, the file link, anything thin or missing (jobs with little text, missing city), and any tabs not covered. Offer to run `/apply` on the file. Say plainly that descriptions are condensed, not verbatim.
