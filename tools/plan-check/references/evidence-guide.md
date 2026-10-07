# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

- Where it lives (eval): the plan's cause is in the "Candidate plan"
  section, usually a line starting "Cause:" or "Diagnosis:" (sometimes
  repeated in the plan comment). The behavior it must explain is in
  the "Repro evidence" block: the numbered steps, the artifact or
  output, the "Control" runs, and the Expected / Actual lines.
- Where it lives (live): the cause is in the student's plan.md
  (Diagnosis section). The repro evidence is the student's posted
  repro comment on the issue, as quoted in plan.md or comment.md.
- What good looks like: the cause explains the failing step, and every
  control run fits it. Example: calib-01's cause (the commits view is
  not refreshed after push) fits step 4, where re-entering the view
  fixes the color. Bad: a control shows the blamed component working
  in the same setup, or the damage is already visible in a step before that
  component runs, or the plan repeats a thread theory
  the evidence rules out (calib-03).

## Scope

- Where it lives (eval): the plan's "Change:" or "Scope:" paragraph,
  its In / Out (or "Not in scope") lines, its numbered approach steps,
  and any extra work promised in the plan comment ("Beyond the fix, I
  plan to...").
- Where it lives (live): plan.md's Scope and Approach sections and the
  draft comment.md.
- What good looks like: every item is the fix, its tests, removing the
  covering xfail marker, or a line deferring related work elsewhere
  (calib-01: "Out: any change to how push status is computed"). Bad: a
  fix wrapped in a rewrite, migration, new option, new abstraction, or
  CI expansion, even when the core fix is right.

## Executability

- Where it lives (eval): the plan's Change / Approach section: file
  paths in backticks, function or branch names, and the verb of the
  change ("add the commits context to its post-push refresh scope").
- Where it lives (live): plan.md's "Files I'll touch" and Approach
  sections.
- What good looks like: a named place (a file, a function, or a
  specific code path inside a named module) and one chosen change
  there, so a stranger could open the code and start. The exact
  function can be left to pin during the build if the path and the
  change are named.
  Bad: "investigate", "explore", "profile and optimize", or options
  left open ("upstream or vendored as needed").

## Test plan

- Where it lives (eval): the plan's "Test:" or "Test plan:" lines,
  read next to the repro evidence's steps and Expected line.
- Where it lives (live): plan.md's Test plan section, read next to the
  student's repro steps.
- What good looks like: the repro steps re-run with a named expected
  result ("at step 3 the color must flip without leaving the view"),
  or a regression test built from the repro case that must pass.
  Bad: "run the full test suite", "should feel fast", "nothing else
  should break".

## Honesty

- Where it lives (eval): a "Risk" or "Unknowns" line in the plan, and
  hedged words in the plan and comment ("I have not yet measured",
  "may be one layer up or down").
- Where it lives (live): plan.md's Risks and unknowns section, and the
  "## Deviations" section after the build, where any change from the
  posted plan is recorded with the reason.
- What good looks like: at least one real open question is named with
  what will be done about it. Bad: guesses stated as confirmed causes,
  or a build that changed with no deviation recorded.

## Comms

- Where it lives (eval): the "Candidate plan comment" section, read
  against the "Thread highlights" (each line shows the author's role:
  OWNER, MEMBER, COLLABORATOR, CONTRIBUTOR, or NONE) and against the
  "Repo facts" block's "contribution policy" line.
- Where it lives (live): the draft comment.md, read against the
  issue's live comments (maintainer comments carry a role badge) and
  the repo's CONTRIBUTING.md, PR template, and any AI policy file.
- What good looks like: if a maintainer named a culprit, set or
  rejected an approach, or posted a patch to test, the comment names
  that direction and follows it or explains why not ("My plan
  follows the approach proposed here: ..."). If the policy
  requires disclosing AI use in comments or in any form, the comment
  says AI was used and for what ("I used Claude to help draft this
  plan; I checked every step myself"). Bad: a
  comment that plans around an owner's posted patch without
  mentioning it, or no disclosure where the policy requires one.
