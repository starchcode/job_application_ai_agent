# Job JSON Parser

How to read the job `.json` file I upload when I call `/apply`.

- The file contains one or more jobs. Parse every job in it.
- Extract for each job, where present: title, company, link, description, requirements, nice-to-haves, location, salary, deadline, how to apply.
- If a field is missing, say so and carry on with what is there.
- Keep the job's link so it can be shown as a clickable link.
- Work out the JSON structure from the file itself.
