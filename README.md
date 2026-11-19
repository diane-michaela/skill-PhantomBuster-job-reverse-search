<img width="482" height="482" alt="phantombuster (1)" src="https://github.com/user-attachments/assets/2bc83dc9-05c5-4143-b320-af90459cb675" />


# Job Reversed Search: a Claude skill (also runs as a ChatGPT GPT)

Find the companies hiring for a stack or a role by searching LinkedIn job posts through Google,
then hand the people search to PhantomBuster.

Give Claude a job description, an intake brief or a list of skills. The skill:

1. **Builds the Google query** (stack focus or role focus) and shows it to you before running it.
   No exclusion (`-stage`, `-alternance`...) unless you ask for one.
   France by default. For several countries (UK, Poland, Spain, Portugal, Lithuania...), it runs
   one query per country, because Google stops at about 100 to 120 results per query.
2. **Collects from Google only**: either you click the bookmarklet and drop its CSV in the chat,
   or Claude in Chrome runs the same bookmarklet code for you. It never uses a search API,
   never opens the job posts and never works around a CAPTCHA.
3. **Delivers one CSV**, `postings.csv`: one row per job post, with a **Company type** column
   (Product, Staffing or Unsure) and a **Country code** column (FR, UK, PL, ES...) taken from the
   LinkedIn subdomain. All countries go in the same file. No Google Sheet, no Drive file.
4. **Proposes a PhantomBuster chain** on the Product companies:
   LinkedIn Job Scraper → LinkedIn Company Employees Export → LinkedIn Profile Scraper.

## How to download and install the skill

### In Claude (claude.ai or the desktop app)

1. **Download the skill file:**
   [**job-reversed-search.skill**](https://github.com/diane-michaela/skill-PhantomBuster-job-reverse-search/releases/latest/download/job-reversed-search.skill)
   (or open [Releases](https://github.com/diane-michaela/skill-PhantomBuster-job-reverse-search/releases/latest)
   on the right of this page and click `job-reversed-search.skill` under **Assets**).
   Why: this single file holds the whole skill, `SKILL.md`, the scripts and the query rules.
   Do not download `SKILL.md` alone: the skill would miss its scripts.
2. **Do not unzip it.** Keep the `.skill` file as it is.
   Why: Claude expects the packed file and unpacks it itself.
3. **In Claude, open Settings → Capabilities → Skills**, click **Upload skill** and pick the file.
   Why: this is where Claude stores your own skills.
4. **Check it is turned on** in the list of skills.
   Why: an uploaded skill that is switched off never runs.
5. If you had an older version of this skill, **remove it**.
   Why: two versions with the same purpose compete, and Claude may pick the old one.

### In Claude Code

```
git clone https://github.com/diane-michaela/skill-PhantomBuster-job-reverse-search.git ~/.claude/skills/job-reversed-search
```

Why: Claude Code reads skills from `~/.claude/skills/`, one folder per skill, and this repo is
already laid out as that folder. To get a newer version later, run `git pull` inside
`~/.claude/skills/job-reversed-search`.

### In ChatGPT

The skill also runs as a custom GPT. One difference with Claude: ChatGPT cannot run code in
your browser, so you always collect the Google results yourself with the bookmarklet (one click
per country search).

**Build the GPT (a paid ChatGPT plan is needed to create one; anyone you share it with can use it):**

1. In ChatGPT, open **GPTs → Create → Configure**.
   Why: this is where custom GPTs are set up.
2. Paste the content of [`chatgpt/instructions.txt`](chatgpt/instructions.txt) into **Instructions**.
   Why: it is the ChatGPT version of `SKILL.md`, without the steps only Claude can do.
3. Under **Knowledge**, upload `references/boolean-rules.md`, `scripts/bookmarklet.js`,
   `scripts/merge-countries.py` and `scripts/build-postings.py`.
   Why: the GPT needs the query rules, the bookmarklet line to give you, and the two scripts
   that build `postings.csv`. The other scripts only serve Claude in Chrome, leave them out.
4. Under **Capabilities**, turn on **Code Interpreter & Data Analysis**.
   Why: without it, the GPT cannot run the scripts or give you the CSV file.

**No GPT? Use a normal chat:** paste `chatgpt/instructions.txt` at the start of a chat with
Code Interpreter, then upload the same four files. Or run the two Python scripts on your own
computer (Python 3, no extra package).

**How a run works in ChatGPT:** the GPT gives you the Google query → you click the bookmarklet
on the results page → you upload the downloaded CSV (one per country) → the GPT returns
`postings.csv` with the Company type column and proposes the PhantomBuster chain.

ChatGPT menu names change often: if one of them is not where this says, look for the closest one.

## Use it

The skill runs only when you ask for it, for example:
"job reversed search: React TypeScript Figma, France, stack focus", or
"run it for the UK, Poland and Spain".

## What's inside

| File | Role |
|---|---|
| `SKILL.md` | The instructions Claude follows |
| `references/boolean-rules.md` | Google operators and how to turn a JD into a query |
| `scripts/bookmarklet.js` | The bookmarklet; the line to paste is at the bottom of the file |
| `scripts/fast-extract.js` | Browser collection, default mode |
| `scripts/page-extract.js` | Browser collection, page by page |
| `scripts/readback.js` | Brings the collected rows back into Claude |
| `scripts/merge-countries.py` | Merges several countries into one list and removes postings found twice |
| `scripts/build-postings.py` | Builds `postings.csv` with the Company type column |
| `chatgpt/instructions.txt` | The ChatGPT version of `SKILL.md`, to paste into a custom GPT |
