# Rubric: is this pull request ready to submit?

A PR is ready when the diff does what the plan said (or says plainly
where it does not), the description tells the truth about the diff,
the evidence re-runs the bug and shows it gone, the repo's own checks
were run, the diff holds only the change, and the repo's stated asks
are met. Checks 1 and 2 cover silent drift, 3 and 4 cover not tested,
5 covers unreviewable, and 6 and 7 cover the standards wall.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| plan-fidelity | Every file and hunk in the diff, read against the plan's scope, its "files" list, its approach steps, and its deviation notes. Then the plan's promised work, read back against the diff. | Pass if (a) every hunk in the diff does work the plan names, work needed to prove the fix (a test, removing a covering xfail marker), or work a deviation note names, AND (b) every piece of work the plan promised shows up in the diff, or is explicitly deferred or dropped in the plan's deviation notes or in the PR description. Fail if the diff adds work the plan does not name (a new option, flag, or setting; a rename pass; a rewrite of a function the plan did not touch; a change to a file the plan never mentions), even when the core fix is right. Fail if promised work is missing with no note. A short comment explaining the fixed line is not extra work. | required |
| description-fidelity | Each factual claim the title and description make about the change ("implements the plan exactly", "no changes beyond the plan", "docs now cover X", "adds a test for Y", "changelog entry added"), read against the diff. | Pass if every claim about what the PR changes is true of the diff. Fail if the description claims something the diff does not contain (a docs update, a test, an entry that is not in the diff), or claims fidelity ("exactly as planned", "no other changes", "no functional changes outside X") that the diff contradicts. Judge claims by substance: one test function that covers three cases supports "tests for three cases". A description that says less than the diff is graded by plan-fidelity, not here. | required |
| decisive-evidence | The test evidence section, read against the plan's test plan and the repro's failing step. | Pass if the evidence re-runs the plan's repro on the failing path (the same command or input that failed before) on the branch, AND shows a specific observable after-result (output, exit code, error text, a named visible behavior) that matches the plan's expected-after, AND every other item the plan's test plan names (controls, a second repro, a named test) is either shown or reported with its result. Fail if the evidence is only "works now", "tested locally", "verified on my machine", or "tests pass" with nothing observable; if it only runs a control or a path the fix did not change; or if an item in the plan's test plan is never mentioned. | required |
| checks-run | The test evidence and description, read against the test or check commands that the plan's test plan or the repo facts (template, contributing guide) name. | Pass if each named suite or check command appears with its outcome (pass, fail with reason, or "no tests ran"). A failing check reported honestly with its reason passes. Pass if neither the plan nor the repo names any command. Fail if a named suite or check is never shown or is described only as "should pass" or "will rely on CI". | required |
| clean-diff | The unified diff and the commit list. | Pass if every hunk is part of the change. Fail if the diff contains any of: a debug print or log left in, commented-out code, a dead or unused helper added, a TODO or FIXME added, a re-indent or formatting-only hunk, an import reshuffle the fix does not need, or a hunk in code unrelated to the fix. One piece of debris is enough to fail. A short comment explaining why the fixed line exists is not debris. Commit message wording alone does not fail this check. | required |
| template-and-asks | The repo facts block's PR template sections and stated asks (in live mode, `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, and the house rules in `scope.md`), read against the description and the diff. | Pass if every section the PR template names has real content, and every item the repo states as required for this kind of PR is met: the issue-link line it asks for (such as "closes #"), a required checklist, and any required changelog, whatsnew, or release-note entry, which must actually appear in the diff. Pass if the repo has no template and states no such asks. Fail if a required section is missing or left as boilerplate, the asked-for issue link is absent, or a required entry is claimed or expected but not in the diff. Do not grade asks a PR package cannot show (signed CLA, vouching, CI status after opening, response times). | required |
| ai-disclosure | The repo facts block's contribution or AI policy (in live mode, the course rule in `scope.md` and SKILL.md), read against the description. Every package is treated as AI-assisted work. | Pass if the stated policy does not require disclosing AI use ("no stated AI policy", or a policy that only asks that the human understand the work). If the policy requires disclosing AI use, pass only if the description itself says AI was used and for what. Fail if disclosure is required and the description has none. | required |
| maintainer-direction | The thread highlights, read against the description and diff. Only OWNER, MEMBER, COLLABORATOR, or CONTRIBUTOR comments count. | Pass if there is no explicit maintainer direction, or the PR follows it, or the description names it and gives a reason for going another way. Fail if the PR goes against a maintainer's stated approach without saying so. | preferred |
| title-quality | The title, read against the diff and any title format the repo states. | Pass if the title names the specific change (not "fix bug" or "update") and follows any stated title format. | preferred |

## Verdict rule

- Accept (ready to submit) only if all seven required checks pass.
- Any required check that fails gives reject (hold).
- An unclear grade on a required check counts as a fail, so it also
  gives reject. Grade unclear only after re-reading the part of the
  package the check names, and only if that part can truly be read two
  ways.
- "The repo states no such ask", "the plan names no such step", and
  "no AI disclosure is required" are passes, not unclears. Missing
  optional context is never a reason for unclear.
- An honest, disclosed shortfall is a pass on plan-fidelity, not a
  fail: less than everything is fine when the plan or description says
  so plainly.
- The two preferred checks (maintainer-direction, title-quality) are
  always reported but never change the verdict.
