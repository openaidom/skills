# Discovering sources

Builds and extends the source list (`openaidom/job-search/sources`). `SKILL.md` defines the list, source health and the ledger; this file is the method for finding new sources. Coverage by large general boards varies by country and field. Check where this kind of job is actually advertised: national and sector boards, agencies and staffing firms, local employers, public employment services, public-sector and NGO portals, and employer career pages.

Inputs, from the profile and preferences: the countries where the user can work, languages, field, titles (including adjacent ones) and workplace preference. Assume none of them. If one is missing, ask once, or work from what is known and record the gap.

Spend at most half of the run's time allowance on discovery. Candidates not yet tested stay in the source list as `unknown` and are picked up in a later run.

## 1. Find candidates

Search the web for each country and field, in each language, with patterns such as `<country> job board <field>`, `<country> jobs <title>`, `remote jobs open to <country>`, `<country> recruitment or staffing agency <field>`, `<country> public employment service jobs`, `<country> government jobs`, `<country> NGO jobs`, `<professional body> jobs`. A domain that recurs across queries is the strongest candidate. Consider national and sector boards, agencies, large local employers, public-sector and NGO portals, professional bodies, universities, and remote boards that accept the country. Write each query to the ledger before running it.

The table in `finding-and-validating-listings.md` lists board feeds for common applicant-tracking systems. An ATS platform is not a source: build an `employer_ats` entry once a listing address gives the company slug.

## 2. Vet each candidate

Make a few requests, sequentially and politely. A source is added as `unknown` and becomes `active` only when all of these hold. Otherwise mark it `rejected` with the first failed check as its reason code, and go on to the next candidate. Save listings seen while vetting as `unchecked`.

1. Terms and `robots.txt` do not forbid automated reading of listings (`forbidden`). Check this before requesting any listing.
2. Listings are public without sign-in (`login_wall`).
3. Some listings are dated, or evidently current, within the last 30 days (`dead`).
4. Each listing names the employer or the agency and shows how to apply (`no_listing_details`).
5. Listings can be reached by a search address, category address or feed (`not_searchable`). Look for `/feed`, `/rss`, `sitemap.xml`, `<link rel="alternate">` and a public JSON address the page itself calls. A page that only renders in a script is `access: browser` and counts only if a page-reading capability can read it.
6. It returns listings the list does not already have, or is the original source of listings already found (`irrelevant`).

If the usual channel for this kind of job cannot pass these checks (sign-in apps, messaging groups, newspapers, in person), record it as a coverage gap for the report.

Never add a source from memory, a search snippet or another list without these requests. Record how to query by title and place, how to page, and how to tell a listing is closed.

## 3. Expand from what you find

For each employer in a listing, find its career page or ATS board, if it has one, from the listing address, add it as an `employer_ats` source, and check its other open roles. An employer board is vetted by one request that returns its listings (the terms and sign-in checks still apply); it then becomes `active` and joins the frontier. When a board repeats listings from another, record that and prefer the original.
