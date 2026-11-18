# Submit Deals to GitHub Lists: a Claude skill

A Claude skill that lists your product's Black Friday / Cyber Monday (or any seasonal) deal in the curated "awesome deals" repos on GitHub, following each repo's own rules.

## What it does

1. **Collects your deal details**: prices per tier, discount, coupon code, dates, product type. It never guesses.
2. **Reads each repo's live rules** (README, CONTRIBUTING, PR templates) and builds a fit matrix, because many lists exclude boilerplates, courses, young products or AI wrappers.
3. **Submits the right way for each repo**:
   - appends a row at the end of the right category in `README.md`
   - adds a per-deal YAML/JSON file when the README is generated
   - regenerates the README when the repo asks for it
   - falls back to the issue form when that's the only option
4. **Checks every PR** (mergeable, additions only), discloses that you're the vendor, and adds no AI attribution footer unless you ask.
5. **Reports** a table of PR and issue links, plus what was skipped and why.

It also handles the usual traps: fork-name collisions between repos that share a name, reusing old forks, `main` vs `master`, and Copilot-prefilled commit messages.

## Requirements

- Claude with a browser it can drive (the Claude desktop app's built-in browser or Claude in Chrome)
- That browser signed in to the GitHub account you want to submit from

## Install

**Claude Code**: copy the skill folder into your skills directory:

```bash
git clone https://github.com/aswilam/submit-deals-to-github-lists
cp -r submit-deals-to-github-lists/submit-deals-to-github-lists ~/.claude/skills/
```

**Claude apps**: zip the `submit-deals-to-github-lists` folder and upload it in Claude's skills settings.

## Use

Ask something like:

> Submit my Black Friday deal for MyApp (50% off all licenses, code BF26, until Dec 6) to the repos in this article: <link>

Claude will ask for anything missing, show you which lists fit, and then open the PRs.

## Where to find lists

Search for "GitHub repositories to submit Black Friday deals <year>". Lists change every season, so the skill always reads each repo's current rules instead of relying on a hardcoded list.

## License

MIT
