# Evidence guide: where evidence lives in a PR package

Four families, one per harness category. A package that fails none of
them is a clear accept.

## Plan fidelity (harness category: silent-drift)

Checks: plan-fidelity, description-fidelity.

Where it lives:

- Eval bundle: the "Plan context" block holds the plan's scope ("in
  scope", "not in scope"), its "Files:" list, its approach steps, its
  test plan, and any deviation or deferral note. The "Candidate PR"
  block's "Diff" section shows what actually changed; its "Description"
  and "Title" hold the claims about the change.
- Live: `plan.md` (sections Scope, Files I'll touch, Approach, and
  `## Deviations`), the output of `git diff main...HEAD` (three dots)
  from the working copy, and the description in `pr_draft.md`.

What good looks like:

- Every changed file and hunk falls inside the plan's stated scope or
  files list, is a test or marker removal that proves the fix, or is
  named in a deviation note. A one-line comment explaining the fixed
  line is fine.
- Every piece of promised work is in the diff, or the plan's deviation
  notes or the description say plainly it was deferred or dropped and
  why. That is an honest shortfall and it passes.
- Every claim the description makes about the change can be pointed to
  in the diff.

What drift looks like:

- More than the plan: a new config option or flag, a rename pass, a
  rewrite of nearby code, or a hunk in a file the plan never mentions,
  with no deviation note.
- Less than the plan: promised work (a docs update, a second fix) is
  missing, and nothing says so.
- A description claiming "exactly as planned", "no changes beyond the
  plan", or "docs now cover X" when the diff shows otherwise.

## Test evidence (harness category: not-tested)

Checks: decisive-evidence, checks-run.

Where it lives:

- Eval bundle: the "Test evidence" section of the candidate PR (and
  before/after blocks that sometimes sit in the description), read
  against the plan context's "Test plan" and its repro evidence (the
  failing step and controls). Repo-required commands are in the "Repo
  facts" block (for example "pass the test suite (yarn test)").
- Live: `test_evidence.md` (repro before on `main` and after on the
  branch, plus the output of `make test-unit`, `make
  test-integration`, `make lint`, and `make typecheck`), read against
  the Test plan in `plan.md`. The template's Testing section in
  `pr_draft.md` should paste the same results. "no tests ran" from
  `make test-integration` is the expected Path Review result.

What good looks like:

- The plan's own failing repro command is re-run on the branch, and
  the evidence shows a specific after-result (output, exit code, error
  gone, a named visible change) that matches the plan's expected-after.
  A before is shown or quoted from the repro.
- Every other test-plan item (controls, second repro, named tests) is
  shown or reported with its result.
- Each named suite or check command appears with its outcome. An
  honest failure with a reason is still evidence.

What not tested looks like: "works now", "tested locally", "verified
on my machine for a day", "cargo test passes" with the plan's repro
never shown; evidence that runs only a control or a path the fix did
not change; a test-plan item never mentioned.

## Diff quality (harness category: unreviewable)

Check: clean-diff.

Where it lives:

- Eval bundle: the candidate PR's "Diff" section (every `+` and `-`
  line) and its "Commits" list.
- Live: `git diff main...HEAD` and `git log --oneline main..HEAD`.
  Before pushing, `git diff main...HEAD --stat` should list only the
  files the fix changes.

What good looks like: every hunk is the fix, its test, or a needed
marker removal. A reviewer can see the change in one read.

Debris tells: a debug `print`, `eprintln!`, or `console.log` left in
(even commented out); commented-out earlier attempts; a dead function
or one behind `allow(dead_code)`; an added TODO or FIXME; a hunk that
only re-indents or re-wraps; import reshuffles the fix does not need;
edits to unrelated code. Commit messages like "wip" are a hint to look
closer, not a fail by themselves.

## Standards and comms (harness category: standards-wall)

Checks: template-and-asks, ai-disclosure, plus the preferred checks
maintainer-direction and title-quality.

Where it lives:

- Eval bundle: the "Repo facts" block's "pull requests" line (template
  sections, required checklist, issue-link line, changelog or whatsnew
  asks) and its "contribution policy" line (AI policy). The PR's
  description shows whether each ask is met, and the diff shows whether
  a required entry file was actually added. Maintainer direction is in
  "Thread highlights".
- Live: `.github/PULL_REQUEST_TEMPLATE.md` (Summary, Issue with
  "Closes #", Changes, Testing checklist, Screenshots / Demo, Notes for
  Reviewers), `docs/CONTRIBUTING.md` (branch naming, remove the xfail
  marker and any `pyproject.toml` suppression for a seeded bug), and
  the house rules in `scope.md`. The course requires an AI-use
  disclosure in "Notes for Reviewers". The issue's comment thread is on
  GitHub.

What compliant looks like: every template section has real content,
the issue-link line has the issue number, a required entry is in the
diff, and when the policy asks for it the description says AI was used
and for what. In Path Review, the CI box is unticked before opening
and the xfail box is ticked only if the marker is gone from the diff.

What a wall looks like: the template's checklist or issue line skipped;
a stated changelog or whatsnew entry missing from the diff; no AI
disclosure under a policy that requires one. (Whether the description's
claims match the diff belongs to plan fidelity, above.)
