# Form Filling (Chrome)

Rules for `/apply` in the "with filling forms" mode. Personal answers (address, gender, citizenship, start date, salary, languages and so on) are NOT here. They live in `additional_information/about_me.md`. Read it before filling any form.

## Never submit
- Never click the final Submit / Apply / Send button, and never submit any step that sends the application. Fill, upload, and stop one click before it. I submit myself.
- It is fine to click "Next", "Save and Continue", "Review" and similar page steps, and "Apply" / "Apply now" / "Start application" / "Apply manually" buttons that lead to the real form.
- Report what is filled and what is left for me.

## Tabs
- Use one Chrome tab group for the whole session. Never close my tabs. If the tab group disappears, tell me, create a new one, and reopen only what is needed.
- Tab order for each job: the LinkedIn job tab first, then the tab that "Apply" opened right next to it (it opens automatically). Then the next job's LinkedIn tab, and so on.
- Easy Apply jobs: only the LinkedIn tab. Click Easy Apply to open the window, do not step through it, and wait until the tailored CV (and cover letter if needed) exists.
- Opening a page is not filling it. While reviewing CV text, only open tabs. Fill nothing until I consent.

## Getting to the real form
- Keep clicking "Apply", "Apply now", "Apply for this job", "Apply for this role", "Start application", "Apply manually" and similar until the page shows the actual form fields for this job. Do not stop on the job description page.
- Prefer "Apply manually" over autofill or "use my last application", so the tailored CV is the one used.
- If a site needs an account or sign-in (including "Continue with Google" or "Create account"), stop and tell me. I sign up or sign in myself, then say so, and you continue from there. Never create accounts and never type passwords.
- Reject optional cookies (choose "Reject all" / "Required only" / "Decline"). If a banner blocks a click, reject it first.
- Never assume an earlier tab still exists. Check the tab list first.

## What to fill
- Check the form first. If it has a cover letter field, upload the cover letter. If it only takes a resume, make and upload the CV only, and do not make a cover letter.
- Upload the tailored CV from `output/YYYYMMDD/` (or `batch/`). Never leave a previously used CV selected. After upload, confirm the new file is the selected one (LinkedIn keeps old ones in the list).
- Some forms reject long file names. Upload from a short copy kept in the scratch folder, for example `YYYYMMDD_company_role_cv_<name>.pdf`, and leave the file in `output/` as it is.
- Warn me when a site says a resume cannot be replaced after upload.
- Names and job titles must be in proper case ("Jane Smith", "Full Stack Web Developer"), not ALL CAPS. Sites that read the CV often fill capitals, so fix them. Check fields that auto-filled from the CV, for example phone numbers that repeat the country code.
- Use facts from `about_me.md`. Where a form needs a personal, legal or work-history fact that is not in my files, ask me. Never guess, never answer a skills question with something untrue (for example "yes" to Java or Azure hands-on experience when I have none). If the honest answer would probably end the application, leave it blank and ask me.
- LinkedIn Easy Apply pre-fills answers from past applications (drug test, start immediately, hybrid and so on). Do not trust them. Confirm with me, or use `about_me.md`.
- Consent, terms and privacy checkboxes that are required: tick "I agree". Optional ones (marketing, SMS, email newsletters, "use my data for other roles", AI interview recording) stay unticked.
- Address: only the minimal address in `about_me.md`. Eircode/postcode only if the form requires it.
- Expected salary: leave the field empty if the form allows it. If it is required, write the friendly line in `about_me.md`.
- Languages: only what `about_me.md` says. Do not add extra languages unless the job requires them.
- Sensitive optional fields (gender, ethnicity, disability, veteran): use `about_me.md` where the form asks. Leave disability and veteran status for me unless `about_me.md` answers them.

## Technical notes for Chrome tools
- LinkedIn Easy Apply "Upload resume": the file input only exists after the button is clicked. In the page, patch `HTMLInputElement.prototype.click` so a created file input is appended to the body and the native file dialog does not open. Then click "Upload resume", find the `file-input` ref, and use the file upload tool. Patch each LinkedIn tab separately. Never click a file button without the patch (the native dialog freezes the page).
- Other sites: look for a `file-input` ref with `find` before clicking anything. Several sites already expose it.
- Custom dropdowns often ignore direct value setting. Click the dropdown, type the option, and press Enter, or click the option.
- Date fields: click the field, then type the digits.
- After filling, take a screenshot or read the field values to check the result.

## End of a session
- When all forms are filled, give a short summary listing each job and its status: filled and ready for me to submit, filled with questions left for me, waiting on sign-in, skipped as a duplicate, or skipped.
- Say plainly what I still have to do (answers, consent, final submit), and any change made to approved CV text (for example a dropped bullet to keep the CV on one page).
