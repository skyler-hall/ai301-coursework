# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/194

**Branch**

fix/68-empty-keyword-index

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Partial smoke run (`--limit 3`): 3/3 (pkg-01, pkg-02, pkg-03 all agreed).
2. Full run: crashed before scoring with `UnicodeEncodeError: 'charmap' codec can't encode characters`. Windows Python sent the prompt to Claude in cp1252, which can't encode the box-drawing characters in one package. No score, and no eval-run.txt was written. I fixed it with `export PYTHONUTF8=1`, with no changes to my skill files.
3. Full run, saved with `--save-run eval-run.txt`: 20/20 (categories: clear-accept 7/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3). Bar 18/20: PASS.

**Package analysis**

pkg-20 (ghostty-org/ghostty#13604). Gold label: reject. My rubric: reject (agree).

The fix itself is strong. It follows the maintainer's own diagnosis, the diff stays inside the plan, and the mode-2031 evidence shows a clear before and after. So plan-fidelity, decisive-evidence, and clean-diff all pass. The package fails on ai-disclosure. The repo facts say ghostty's AI_POLICY.md requires disclosing all AI usage, with the tool and how much it helped, and the PR description has no sentence about AI at all. My rubric treats every package as AI-assisted work, so a required disclosure that is missing is a fail, and any failed required check gives reject. This is the case my standards checks exist for: a PR can be technically perfect and still not be ready, because the repo said what it needs and the PR ignored it.

**Check rationale**

From tools/pr-precheck/rubric.md:

| decisive-evidence | The test evidence section, read against the plan's test plan and the repro's failing step. | Pass if the evidence re-runs the plan's repro on the failing path (the same command or input that failed before) on the branch, AND shows a specific observable after-result (output, exit code, error text, a named visible behavior) that matches the plan's expected-after, AND every other item the plan's test plan names (controls, a second repro, a named test) is either shown or reported with its result. Fail if the evidence is only "works now", "tested locally", "verified on my machine", or "tests pass" with nothing observable; if it only runs a control or a path the fix did not change; or if an item in the plan's test plan is never mentioned. | required |

It reads this way because the not-tested packages each fail in a different way, and "tests pass" alone catches none of them. pkg-04 and pkg-10 say "works now" or "verified on my machine" without showing the plan's repro, so the check asks for a specific observable after-result. pkg-07 and pkg-14 do run something, but only the control or a path the fix never touched, so the check says the re-run has to be the same failing command or input. calib-04 shows one repro but never mentions the second failure mode in its test plan, so every test-plan item has to be shown or reported. I let controls and suites be "reported with its result" instead of requiring full pasted output, because clear accepts like pkg-19 and pkg-13 report their controls in one line and they are still ready.

**Trade-offs**

clean-diff fails a package on one piece of debris. That is strict on purpose: pkg-12, pkg-15, and pkg-18 each bury a correct fix under debug lines, dead code, or churn, and pkg-18 is arguable because its debris is mostly mechanical. The cost is that a careful PR with one leftover line gets held, and I accept that miss. To keep it from hitting clear accepts, I wrote one exception into clean-diff and plan-fidelity: a short comment explaining the fixed line is not debris. pkg-11 adds that kind of comment and still came out accept, and so does my own PR. Nothing else needed changing after the full run: it agreed on all 20, so I made no revisions and ran no `--only` canaries, and the saved eval-run.txt is the same rubric I uploaded.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.