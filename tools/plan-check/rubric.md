# Rubric: is this plan ready to post and build from?

A plan is ready when its cause fits the reproduction, it makes one
bounded change, a stranger could start building it, its test shows the
fix worked, and its comment respects the thread and the repo's rules.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| grounded-diagnosis | The plan's stated cause, read against every step and every control run in the repro evidence block (in live mode, the student's posted repro comment as quoted in the drafts). Also the thread highlights, if the plan borrows its cause from the thread. | Pass if the stated cause explains the failing step AND no step or control in the repro evidence contradicts it. Fail if a control shows the named component working in the same setup, or shows the damage already happening before or without that component, or if the plan adopts a thread or issue theory that the repro evidence rules out. Fail if the plan gives no cause at all. Confidence and length do not count; only fit with the evidence does. | required |
| bounded-scope | The plan's scope or "in / out" statement, its approach steps, and the plan comment's description of the work, read against what the issue and repro evidence say is broken. | Pass if every piece of planned work is needed to fix the reproduced behavior or to prove it fixed (the fix itself, regression tests, removing a covering xfail marker, a doc line for the changed behavior). Explicitly deferring related work to a separate issue or PR is fine and still passes. Fail if the plan also commits to work the issue did not ask for: a rewrite or redesign of the surrounding code, a library or dependency migration or upgrade, a new user-facing option or settings panel, a new abstraction, a CI matrix, or "while I'm here" cleanups. A correct core fix bundled with that extra work still fails. | required |
| executable | The plan's approach: the files, functions, or code locations it names, and the change it says it will make there. | Pass if a stranger could start the change today without asking the author anything: the plan names where the change goes AND commits to one chosen change there. "Where" can be a file, a function, or a specific code path inside a named module (for example "the reattach path in `zellij-server`'s client connection handling"). Leaving the exact function or line to be pinned during the build is fine when the code path and the change are both named. Fail if the plan only says it will "investigate", "explore", "profile", or "look into" things, lists options without choosing ("upstream or vendored, whichever is easier", "gocui? tcell?"), or names no file or location. | required |
| decisive-test | The plan's test plan, read against the repro evidence's steps and expected result. | Pass if the test names a specific observable result that would show the reproduced bug is gone: re-running the repro steps with a named expected output, exit code, or visible change, or a regression test that encodes the repro case and must pass. Fail if the only test is "run the full suite", "nothing else breaks", "it should feel fast", or anything else with no observable outcome tied to this bug. | required |
| thread-direction | The plan comment, read against the thread highlights. Only comments from the repo's OWNER, MEMBER, COLLABORATOR, or CONTRIBUTOR roles (maintainers) count as direction. | Pass if the thread has no explicit maintainer direction. If a maintainer named the culprit location, proposed or settled an approach, rejected an approach, or posted a patch and asked for testing, pass only if the plan comment engages that direction: it follows it, builds on it, or names it and gives a reason for going another way. Fail if the comment plans around that direction without mentioning it. | required |
| ai-disclosure | The repo facts block's contribution policy line, read against the plan comment's text. | Pass if the stated policy does not require disclosing AI use in comments. That includes "no stated AI policy", policies that only say you must understand and own AI-assisted work, and policies that only ask for disclosure in the pull request. If the policy requires disclosure of all AI use in any form, or of AI use in comments, pass only if the comment itself says AI was used and for what. Every package is treated as AI-assisted work, so a missing disclosure under such a policy fails. | required |
| honest-unknowns | The plan's risks or unknowns section and any hedged claims in the plan and comment. | Pass if the plan states at least one real open question or risk, or states that it has none and why, and does not present guesses as confirmed facts. | preferred |

## Verdict rule

- Accept (ready) only if all six required checks pass.
- Any required check that fails gives reject (hold).
- An unclear grade on a required check counts as a fail, so it also
  gives reject. Before grading unclear, re-read the part of the package
  the check names; grade unclear only if that part truly does not
  settle the question.
- The "no direction in the thread" and "no disclosure required" cases
  are passes, not unclears. Missing optional context is never a reason
  for unclear.
- The preferred check (honest-unknowns) is reported but never changes
  the verdict, whether it passes, fails, or is unclear.
