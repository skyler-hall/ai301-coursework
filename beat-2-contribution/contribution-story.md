# Contribution: Add header and footer to default base template

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Skyler Hall
**GitHub Username:** skyler-hall
**Issue:** https://github.com/render-engine/render-engine/issues/609
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built my issue scout and fixed its rubric after a weak first scan**

I installed the issue scout, wrote my fit profile and three gh search queries (Python and TypeScript good first issues, plus Python LLM help wanted), and ranked results by fewest comments. My first scan accepted all 5 issues, but two were the only issue in tiny repos where nobody had ever replied, and they still passed because "Maintainer responds" was only a preferred check. I made that check required and stricter, added the AI policy row the verdict rule named but the table was missing, and dropped issues opened in the last 14 days. When I graded my picks from the Issue Selection site, it rejected 5 of 6 as already claimed, which showed me how fast good first issues get taken.

**Unit 6: Installed the voice guard and fixed it so it actually fires on Windows**

I added an Upstream section to my voice guide with rules for claims, questions, and pings, then installed the voice-guard hook. The wiring test ran unblocked at first because Claude Code on Windows runs commands through a PowerShell tool, not Bash, so the hook's matcher never matched and check_voice.py skipped anything not named Bash. I widened the matcher to Bash|PowerShell and patched that check, and my guard log now shows two stops on the --help test and a pass on my real claim comment.

My CLAUDE.local.md found that CONTRIBUTING lives in docs/docs/contributing/ and that uv sync rewrote uv.lock unless I passed --frozen, so I wrote both down to keep the lockfile out of my PR.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

My scout's first wild scan ran three gh search queries from my fit profile (Python and TypeScript good first issues, plus Python LLM issues labeled help wanted) and ranked them by fewest comments. It accepted all five top results, but most were brand-new issues in repos with 0 or 1 stars, so I didn't use them. My real picks came from the AI301 Issue Selection site, and I graded six of them in two rounds: GRCEngClub/claude-grc-engineering#163, ArduPilot/ardupilot#14439, ICTU/quality-time#12823, rogerSuperBuilderAlpha/cursor-boston#553, pyvista/pyvista#1629, and render-engine/render-engine#609. Five were rejected because someone had already claimed them, usually with an open PR. render-engine#609 was the only accept. After the unit 6 scope patch I graded it again and it still passed, now with an "ambitious" difficulty note: the header and footer blocks in _base.html are empty, but the maintainers never decided what the defaults should contain. So my claim asks that question first.

This issue fits my profile because it is Python plus Jinja HTML templates, close to my web work on Recyclepedia, and the result is something I can see in a browser. I want to learn how a real project's template system is organized and how to follow a repo's own rules, like waiting to be assigned and adding an AI attestation. Honestly, the scout's first scan was weak: ranking by fewest comments pulled in day-old issues from tiny repos, and its claim check only named PRs linked in the sidebar, even though most of the claims were PRs mentioned in the thread. Grading one issue at a time is where it earned its keep: it caught five claimed issues I couldn't see from the Issue Selection site, and it found render-engine's "wait to be assigned" rule before I posted.

Claim comment: https://github.com/render-engine/render-engine/issues/609#issuecomment-6710634394

**Toolkit**

- Tool: issue-scout, with my search-strategy.md and rubric.md. I ran one scan, then graded my Issue Selection picks with "issue-scout: grade", and graded again after the unit 6 scope patch.
- What it produced: an accept for render-engine#609 with every required check passing, an "ambitious" difficulty note naming the open question (what the default header and footer should show), and rejects for five other picks that were already claimed.
- Difficulties: the first scan surfaced brand-new issues in tiny repos; "Maintainer responds" was only preferred, so silent repos got accepted; the verdict rule named an AI policy check the table didn't have; my old scope check would have rejected any issue with an open design edge; and the claim check rejects a whole checklist issue (like pyvista#1629) because of other people's PRs on other items.
- What changed: I made "Maintainer responds" required and stricter, added the AI policy row, added a 14-day minimum issue age to my strategy, and swapped my scope check for the unit 6 scope-feasible row with a difficulty note. I would still change the claim check so open PRs only count when they cover the same item.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
