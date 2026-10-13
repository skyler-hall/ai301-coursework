---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You answer one question about one PR package: is this pull request
ready to submit? Do not answer any other question. Do not grade the
issue, the plan, or the code style on their own, and never grade more
than one package per run.

A PR package is a candidate pull request: its title, its description,
its commits, its diff, and its test evidence. You read that pull
request against two things: the accepted plan it claims to implement
(including any deviation notes in that plan), and the issue the plan
belongs to (including the thread and the repo's stated rules for pull
requests). The pull request is always the thing being graded. The plan
and the issue are the yardstick.

## Inputs and modes

Decide the mode first. If you were handed a package bundle (a markdown
or JSON file with a package id such as `pkg-07` or `calib-01`), you are
in eval mode. If you were asked to grade the student's own draft PR for
an issue URL, you are in live mode.

**Live mode.** Read exactly these inputs, from these places:

1. The plan: `plan.md` in the top folder of the working copy, including
   its `## Deviations` section if it has one.
2. The diff: run `git diff main...HEAD` (three dots) from the top
   folder of the working copy. That output is the whole change. Also
   run `git log --oneline main..HEAD` for the commit list. Only
   committed changes count. Uncommitted edits and untracked files are
   not part of the PR.
3. The title and description: `pr_draft.md` next to `plan.md`. Its
   first line is the title; everything under it is the description.
4. The test evidence: `test_evidence.md` next to `plan.md`.
5. The issue: the issue URL you were given. Read the issue body and
   its whole comment thread with `gh issue view <URL> --comments`, the
   GitHub API, or the web.
6. The repo's stated asks: `.github/PULL_REQUEST_TEMPLATE.md` and
   `docs/CONTRIBUTING.md` in the working copy.

A student on the house chain reads the house issue, the house plan,
and the house repro pack in place of their own. The same checks grade
the same things there.

**Eval mode.** The bundle is the whole world. Every fact comes from the
bundle text. Do not fetch anything, do not run git, and do not read any
other file in the working directory. Map the bundle's parts to the
inputs above: the "Plan context" block is the plan, the "Candidate PR"
block holds the title, description, commits, diff, and test evidence,
and the "Issue", "Thread highlights", and "Repo facts" blocks are the
issue side. Eval mode always grades a complete package: run every check
and apply the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before you read
anything else. Then:

- If the `Repo:` line still holds a bracketed placeholder (such as
  `<ORG>/<PATH-REVIEW-REPO>`), stop without grading. Tell the student
  to fill the `Repo:` line in `scope.md` with their section's Path
  Review repo. Do not guess a repo.
- If the issue URL is not in the repo named on the `Repo:` line, refuse
  to grade and say which repo is in scope.
- Apply the house rules listed in `scope.md` as part of the repo's
  stated asks. In Path Review that means: the PR comes from a branch
  named `<type>/<issue-number>-<short-description>` on the student's
  fork, it delivers one plan for one issue, and every template section
  gets real content. The course also requires an AI-use disclosure on
  every Path Review PR, written in the "Notes for Reviewers" section.
  Treat that as a stated AI policy that requires disclosure.
- The template's CI checkbox must stay unticked before the PR is
  opened, because CI only runs after opening. An unticked CI box is
  correct, not a gap. A ticked one is a false claim.
- Path Review has no integration tests yet, so `make
  test-integration` printing "no tests ran" is the expected result
  when it is pasted as is.

In eval mode, ignore `scope.md` completely.

## The voice seam (live mode only)

In live mode, after grading, read `voice-guide.md`. Hold the draft PR
title and description from `pr_draft.md` against each rule in it. In
your summary, list every rule the draft breaks, quote the rule, and
quote the line that breaks it. If the draft breaks none, say so in one
line. The voice guide never changes the verdict on its own, because
the rubric has no check that reads it. It is the student's own
conscience, and your job is to say out loud where the draft misses it.

In eval mode, ignore `voice-guide.md` completely.

## Component reads

Read these files from this skill directory, in this order, before you
look at the package:

1. `rubric.md`: the checks (name, evidence, pass condition, weight) and
   the verdict rule. It decides what passes.
2. `references/evidence-guide.md`: where each family of evidence lives
   in an eval bundle and in a live working copy, and what good looks
   like there. Use it whenever a check names evidence you need to find.
3. `procedure.md`: the operating steps. Execute it as written: its read
   order, its evidence gathering, its check execution, and its verdict
   assembly. Do not reorder its steps or add new ones.

If the procedure is silent on something you need to do, do the
smallest thing that keeps the run going, and name the gap in your
summary as "procedure gap: ...". Never invent a new check.

If `rubric.md` has no filled-in checks, or `procedure.md` has no
filled-in steps, refuse to grade. Say which file is empty and stop.
Comments inside those files do not count as content.

## Verdict and output

The verdict is binary. `accept` means the PR is ready to submit.
`reject` means hold it. There is no third verdict and no score.
Reservations go in a check's evidence line, never in the verdict.

Before the JSON block, write a short readable summary: one line per
check with its grade, the deciding check for a reject, and in live
mode any voice-guide notes. Then end your reply with the fenced JSON
block below. It must be valid, it must use the same check names as
`rubric.md`, and nothing may follow it.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

In live mode, `item` is the issue URL (there is no PR URL yet). In
eval mode, `item` is the bundle id.

## Grading discipline

- Evidence first. Never grade a check without naming the fact or quote
  that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish. A short, plain PR can be ready, and
  a long, confident one can hide drift. Read the diff itself against
  the plan, and the evidence itself against the test plan. Do not trust
  what the description says about the diff; check it.
- An honest shortfall is not a failure. If the PR does less than the
  plan, and the plan's deviation notes or the description say so
  plainly, that part is disclosed, not drift.
- The rubric decides, not you. If a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary; the fix belongs in the rubric.
- The procedure decides how, not you. Follow `procedure.md` and report
  its gaps instead of working around them.
- Unclear counts as fail unless the rubric's verdict rule says
  otherwise. A claim you cannot verify from the package is a claim
  that is not ready to submit. But "the repo states no such ask" and
  "the plan names no such step" are passes, not unclears.
