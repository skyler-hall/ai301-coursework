# Evidence guide: where proof lives in a reproduction package

This is the map for the rubric. For each kind of proof, it says where
to look and what good looks like. In eval mode the bundle is the whole
world: quote from it and fetch nothing. In live mode, the drafts are
the candidate side and GitHub is the issue side.

## Environment

- Where it lives: in an eval bundle, the repro report's "Environment:"
  line or table, plus any sentence in the report or claim comment that
  says how the setup differs from the issue ("filed against 13.0.0,
  tested on 15.2.0"). The issue's target lives in the issue body (its
  version line or installed-versions block) and in the thread
  highlights (for example "confirmed on main" or "maintainers could not
  reproduce on the Store release"). In live mode: the draft's
  environment line, the issue body, the issue's comments, and the
  repo's bug report template (`.github/ISSUE_TEMPLATE/`), which lists
  what the maintainers need.
- What good looks like: the software version and the OS or platform
  are both named, and any dimension the issue says matters (driver,
  build profile, browser language, shell) is named too. The versions
  match what the issue targets, or the difference is called out in
  plain words. A report with no environment line at all fails, even if
  its log looks right, because a stranger cannot place the attempt.

## Steps

- Where it lives: the repro report's steps, commands, code blocks, and
  descriptions of input files. Trigger conditions live in the issue's
  own steps and in maintainer comments in the thread highlights (an
  OWNER or MEMBER naming "you also need flag X"). In live mode: the
  draft's steps, read against the issue body and the maintainer
  comments on GitHub.
- What good looks like: a stranger starting from a clean machine could
  run the same thing. Commands are shown or pinned by reference to the
  issue's public snippet ("ran the issue's script verbatim"), or the
  input file is described exactly. Every named trigger is present.
  Steps that live in a private repo, use an unshared config, or say
  "set up the project" are not followable. Steps that quietly change
  the issue's input (a different flag, a different syntax, an unbound
  variable) do not test the issue.

## Behavior shown

- Where it lives: the artifacts inside the repro report: pasted
  terminal output, log excerpts, error messages, exit codes, console
  output, rendered prompts, and "Actual:" lines tied to those
  artifacts. The target behavior lives in the issue body (the exact
  error text, exit code, or wrong output the reporter showed). In live
  mode: the draft's pasted output against the issue body on GitHub.
- What good looks like: the artifact shows the same symptom the issue
  shows. Compare the specifics: a panic or crash (exit 101, stack
  trace) is not the same as a graceful validation error (exit 1, a
  usage message); a compile error is not the same as a runtime path
  error; garbled output with the program still running is not a crash;
  a version banner or session list only proves the program runs. A
  control run (trigger removed, correct result) is strong support. An
  honest cannot-reproduce shows the real output of running the issue's
  trigger.

## Honesty

- Where it lives: every claim sentence in the claim comment and the
  report ("confirmed", "reproduced", "root cause is", "not limited
  to", "guaranteed", "100% reproducible", "on two machines"), and the
  "Expected:" and "Actual:" lines, each read against the artifacts
  actually shown in the package.
- What good looks like: every claim points to an artifact you can see,
  or is labeled as a guess ("looks like", "my hypothesis"). An honest
  cannot-reproduce says so first, shows the attempt, and names what
  differed from the reporter's setup; that is a pass. Red flags:
  certainty words stacked on no artifact, a root cause asserted with
  no trace or experiment, a claim about a release or platform the
  report never tested, and an "Expected" that describes what was
  observed instead of what the issue says should happen.

## Comms

- Where it lives: the claim comment (read against the issue title,
  body, and thread), and the repo-facts block's "bug reports" and
  "contribution policy" lines (template asks and any AI policy). In
  live mode: the draft claim comment, the repo's CONTRIBUTING.md,
  AI_POLICY.md or similar, and `.github/ISSUE_TEMPLATE/`; for Path
  Review, also the house rules in `scope.md`.
- What good looks like: the claim names something only this issue has
  (a symptom, a flag, a code location a maintainer pointed to), says
  what the author will do next, and promises only investigation and a
  report, never a fix, a date, or a guarantee. Boilerplate that fits
  any issue ("kindly assign me", "+1", "same here") fails.
- AI policy: read the contribution policy line closely. Disclosure is
  required only when the policy says AI use must be disclosed for
  issues or comments (for example "all AI usage in any form must be
  disclosed"). Treat every course package as AI-assisted; when
  disclosure is required, look for an explicit sentence naming the AI
  help and its extent, and fail without one. When the policy has no
  AI rule, is permissive, asks for disclosure only in pull requests,
  or asks only that comments be in the contributor's own words, no
  disclosure is needed and the check passes.
