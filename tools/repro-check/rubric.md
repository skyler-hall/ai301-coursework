# Rubric: is this reproduction package ready to post?

Read the whole package first: issue context, thread highlights, repo
facts, then the claim comment, then the repro report. Every check reads
the thing itself against the issue, never the formatting. A short,
plain report can pass every check; a long, confident one can fail them.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| env-recorded | The repro report's environment record (usually an "Environment:" line or table), read against the issue body and thread for which dimensions matter (for example OS, driver, build profile, browser and language, shell) | Pass if the report names the version of the software it tested AND the OS or platform it ran on, AND names every dimension the issue or its thread says changes the behavior (for example the minikube driver on a driver-specific issue, or debug vs release build when the build profile changes the failure). Fail if there is no environment record at all, or the version or platform is missing, or an issue-critical dimension is missing. | required |
| env-faithful | The environment record read against the issue's stated target (version, platform, branch), plus any sentence in the report or claim that names a difference | Pass if the tested environment matches what the issue targets, OR every material difference (older or newer version, different OS, different shell, different install) is stated out loud in the report or claim. Fail if the report tests a materially different target (for example an older major version than the one the issue was confirmed on) and does not say so. A newer version that is named as newer is fine. | required |
| steps-rerunnable | The repro report's steps: commands, inputs, config, and files, read against the trigger conditions named in the issue and by maintainers in the thread | Pass if a stranger with only public materials could re-run the attempt: the commands and inputs are shown, or pinned precisely by reference to the issue's own public snippet or by an exact description of the file's contents, AND every trigger condition the issue or a maintainer names is included (for example the `--replace` flag, the specific driver). Fail if any step depends on private code, private config, or an unshared repo, or says only "set up the project", or skips or changes a named trigger condition. | required |
| behavior-matches | The artifacts in the repro report (output excerpts, logs, error text, exit codes, prompts), read against the exact symptom the issue describes | Pass if an artifact shown in the report exhibits the issue's specific behavior (same error type or message, same exit code class, same wrong output), not an adjacent one. ALSO pass for an honest cannot-reproduce: the report plainly says it did not reproduce and shows the artifact of what actually happened when the issue's trigger was run. Fail if there is no artifact, or the artifact shows a different error (for example a graceful validation error or compile error where the issue reports a panic, crash, or wrong result), or the artifact only shows the program runs or is installed, or the input was changed so the issue's trigger never ran. | required |
| claims-backed | Every factual claim in the claim comment and the report ("reproduced", "confirmed on X", "not limited to Y", a stated root cause, "guaranteed", "100%", expected vs actual) read against the artifacts actually shown | Pass if every headline claim (that the bug reproduced or did not, where, on what scope, and any root cause) is supported by an artifact in the package or is clearly marked as a guess or hypothesis, AND the expected/actual lines agree with what the artifacts show. A side observation that names its exact command (for example a control run described in one sentence, "dropping flag X gives the correct result") does not need its own pasted output, as long as the headline claim has one. Fail if the package asserts reproduction, a root cause, or a wider scope (another release, another platform) that no shown artifact supports; or narrates an artifact as something it is not (calling a graceful error or garbled output a crash); or states expected/actual backwards from what was shown. | required |
| claim-specific-and-honest | The candidate claim comment, read against the issue title, body, and thread | Pass if the claim comment names at least one detail specific to THIS issue (a symptom, a trigger, a version, a code location, a thread pointer) AND states a concrete next step of investigation or reporting, AND promises no fix, deadline, or guarantee. Fail if it is interchangeable boilerplate that would fit any issue ("assign me", "+1", "I love this project"), has no stated next step, or promises a fix, a timeframe, or a guaranteed result. | required |
| ai-disclosure | The repo-facts block's contribution policy line (CONTRIBUTING.md, AI_POLICY.md, or similar), read against the claim comment and repro report | First decide whether the policy REQUIRES disclosure of AI use for issue comments or issue reports. If it does not (no AI policy; a permissive policy; disclosure asked only in pull requests; or a rule that comments be in the contributor's own words without asking for a disclosure), this check passes. If it does require disclosure for comments or issues, treat the package as AI-assisted (all course packages are) and pass only if the claim comment or repro report contains an explicit statement of AI use (what tool or assistance, and to what extent). Fail if disclosure is required and no such statement appears, no matter how good the rest of the package is. | required |
| control-run | The repro report's artifacts | Pass if the report shows a control run (the same command with the trigger removed or changed) that behaves correctly, which isolates the trigger. Otherwise fail. | preferred |

## Verdict rule

Accept if every required check passes. Reject if any required check is
graded fail or unclear. Preferred checks are reported but never change
the verdict. Unclear counts as fail, with one exception: in live mode
on a claim-only draft, checks that need the repro report
(env-recorded, env-faithful, steps-rerunnable, behavior-matches, and
the report half of claims-backed) are graded unclear with "not yet
applicable: claim-only draft" and are left out of the verdict; the
verdict then rests on claim-specific-and-honest, ai-disclosure, and
claims-backed as applied to the claim comment alone (a claim-only
draft must promise the report, not assert a reproduction it has not
shown).
