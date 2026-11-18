---
name: submit-deals-to-github-lists
description: Submit a product's Black Friday / Cyber Monday (or other seasonal) deal to curated GitHub deal-list repos via pull requests or issues, following each repo's rules.
---

# Submit deals to GitHub deal-list repos

Use this when someone wants their product's sale (Black Friday, Cyber Monday, launch sale, etc.) listed in community "awesome deals" repos on GitHub. Works for one or several products in one run.

## 1. Collect the deal details first

Never guess a price, code, date or URL. Ask for anything missing, and check prices on the product's own site.

For each product:
- Name and official URL (no shorteners; add referral or UTM tags only if the user wants them, and never on lists that forbid them)
- One-line description and product type (app / SaaS / boilerplate / course / book...). The type decides which lists accept it
- Platform (macOS, web, iOS...)
- Regular and sale price for every tier, and whether it's one-time or a subscription
- Discount %, coupon code, and whether it's applied automatically
- Start and end dates
- Launch date (some lists need the product to have been public for 6+ months)

Also confirm:
- Which GitHub account to use, and that it's signed in to the browser
- Whether the PR text should carry any tool or AI attribution footer. Default: none
- Existing forks on that account (`github.com/<user>?tab=repositories&type=fork`). Reuse them

## 2. Build the repo list and a fit matrix

Start from the user's list or article, or search for "GitHub repositories to submit Black Friday deals <year>". For each repo, read the **live** README and CONTRIBUTING / AGENTS.md / CURATOR.md / PR template. Third-party articles and summaries go stale; the repo files win.

For each repo, record:
- Is it the current year's edition? Look at the title and the date of the last commit. Skip lists that are clearly dead
- Accepted product types. Common exclusions: "software only, no boilerplates or courses", "security only", "public 6+ months", "no AI wrappers"
- How to submit: append a row to README.md, add one file per deal (YAML/JSON), use an issue form, or use an external form
- Row format, column order, emoji legend (e.g. 💸 for 50%+ off), date format, where new rows go (almost always the **end** of the category)
- One PR per product, or all products in one PR
- Required disclosure ("vendors must say so"), no affiliate links, a star request
- Whether the README is generated, so you must not edit it by hand

Show the user the matrix (repo × product: ✅ category / ❌ reason / ⚠️ caveat) before submitting.

## 3. Submit, repo by repo

Track progress with a task list. For each repo:

**README-row repos**
1. Open `https://github.com/<owner>/<repo>/edit/<default-branch>/README.md`. If prompted, click "Fork this repository" (an existing fork is reused automatically).
2. Insert rows programmatically rather than typing into the editor. GitHub's editor is CodeMirror 6:
   ```js
   const cc = document.querySelector('.cm-content');
   const v = cc.cmView?.view || cc.cmTile?.view;
   const doc = v.state.doc.toString();
   // find the target category heading, then the blank line ending its table
   const h = doc.indexOf('\n## Category Name\n');
   const end = doc.indexOf('\n\n', doc.indexOf('\n|', h));
   v.dispatch({ changes: { from: end, insert: '\n' + row } });
   ```
   Read the lines around the insertion point back to confirm placement.
3. Click "Commit changes...". The commit message box may be prefilled by Copilot, so set its value directly instead of typing into it, and clear any suggested extended description. Then click "Propose changes".
4. On the compare page, check: "Able to merge", 1 file changed, additions only, no deletions.
5. Click "Create pull request". If there's a template, fill it in honestly and tick only the boxes that are true. Otherwise write a short body: which category each row went in, the deal summary, the pricing link, and "I'm the vendor/developer". No attribution footer unless the user asked for one.

**File-per-deal repos** (e.g. `deals/<year>/<category>/<slug>.yml`)
- Use `https://github.com/<owner>/<repo>/new/<branch>?filename=<path>` and set the editor content the same way. A web commit adds one file, so it's usually one PR per product, which fits these repos' "one deal per file" rule.
- Respect field length limits and allowed fields. Quote strings that contain colons.

**Repos that want a regenerated README alongside a data file**
- Read their build script. If the output is a simple render (e.g. it appends one table row), commit the data file first, then edit README.md on the same fork branch (`/<you>/<fork>/edit/<branch>/README.md`, then "Commit directly to the branch"), and only then open the PR.
- Check that the existing README matches the script's output before trusting your replication. In the PR, say you replicated the script's output; don't claim you ran it.

**Issue-form repos**
- Use the issue form, or a blank issue that mirrors the form's fields, with vendor disclosure and the official pricing link. Submit one issue per product.

## 4. Gotchas

- **Fork name collisions:** many deal repos share a name (e.g. `black-friday-deals`). If the account already has a fork with that name from another owner, the in-editor "Fork" button silently fails. Fork via `https://github.com/<owner>/<repo>/fork` with a custom name like `<owner>-<repo>`.
- **Old forks:** the web editor builds the new branch from upstream, so a stale fork is fine. Still confirm the compare page shows only your commit.
- **Default branch** may be `master`. A 404 on `raw/main` means try `master`, or use `/raw/HEAD/`.
- After the final submit click, wait a few seconds and read `location.href` to capture the PR or issue URL.
- Don't put the same product in several categories of one list unless that list explicitly allows it.
- Never paste affiliate or referral links into lists that ban them.

## 5. Report

Finish with a table: repo → PR/issue link per product, or the reason it was skipped. Note anything that needs the user (maintainer approval of CI checks, a star request, an issue-based submission that may take longer). Keep a reusable record of the repos that worked so next season's run is faster.
