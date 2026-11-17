# Job Reversed Search: a Claude skill

Find the companies hiring for a stack or a role by searching LinkedIn job posts through Google,
then hand the people search to PhantomBuster.

Give Claude a job description, an intake brief or a list of skills. The skill:

1. **Builds the Google query** (stack focus or role focus) and shows it to you before running it.
   No exclusion (`-stage`, `-alternance`...) unless you ask for one.
2. **Collects from Google only**: either you click the bookmarklet and drop its CSV in the chat,
   or Claude in Chrome runs the same bookmarklet code for you. It never uses a search API,
   never opens the job posts and never works around a CAPTCHA.
3. **Delivers one CSV**, `postings.csv`: one row per job post, with a **Company type** column
   (Product, Staffing or Unsure). No Google Sheet, no Drive file.
4. **Proposes a PhantomBuster chain** on the Product companies:
   LinkedIn Job Scraper → LinkedIn Company Employees Export → LinkedIn Profile Scraper.

## Install

**In Claude (claude.ai or the desktop app)**

1. Download `job-reversed-search.skill` from the
   [latest release](../../releases/latest).
2. In Claude, go to **Settings → Capabilities → Skills** and upload that file.

That one file holds everything: `SKILL.md`, the scripts and the query rules.

**In Claude Code**

```
git clone https://github.com/diane-michaela/skill-PhantomBuster-job-reverse-search.git ~/.claude/skills/job-reversed-search
```

Update it later with `git pull` in that folder.

## Use it

The skill runs only when you ask for it, for example:
"job reversed search: React TypeScript Figma, France, stack focus".

## What's inside

| File | Role |
|---|---|
| `SKILL.md` | The instructions Claude follows |
| `references/boolean-rules.md` | Google operators and how to turn a JD into a query |
| `scripts/bookmarklet.js` | The bookmarklet; the line to paste is at the bottom of the file |
| `scripts/fast-extract.js` | Browser collection, default mode |
| `scripts/page-extract.js` | Browser collection, page by page |
| `scripts/readback.js` | Brings the collected rows back into Claude |
| `scripts/build-postings.py` | Builds `postings.csv` with the Company type column |
