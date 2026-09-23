# Unit 1 — Issue Selection

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68

**Verdict output**

Mode: live. Candidate is in the scoped repo (codepath/pathreview-ai301-fa26-s3), so it's a valid candidate; the Path Review house rule on claim comments applies.

Issue #68 — "Keyword search raises ZeroDivisionError" (bug, good first issue, rag, tier-1)

Required checks:
- Repo not archived — pass. API archived: False, disabled: False.
- Repo actively maintained — pass. pushed_at: 2026-09-16, 7 days before today (2026-09-23), well inside 90 days.
- Issue not already claimed — pass. assignees: []; repo has 0 PRs total so no linked PR. The one comment is a claim — acordero4852, "Hi! I'd like to take this one on." — but its author_association is NONE, i.e. a classmate, and scope.md's house rule says other students' claim comments do not block an issue here.
- Scope fits a newcomer — pass. One bounded fix in one module: "index([]) raises ZeroDivisionError … index() shouldn't raise on an empty corpus either", two named files (rag/retriever/keyword_search.py, tests/unit/test_keyword_search.py), estimated effort 2-4 hours. Not an umbrella issue, not a usage question, no maintainer comment about unresolved core internals.
- AI contribution policy allows this workflow — pass. No root CONTRIBUTING.md / AI_POLICY.md / AGENTS.md (404); docs/CONTRIBUTING.md and the PR template contain no AI-use restriction. Silence passes.

Preferred checks:
- Maintainer responds to issues — pass. Issue #52 opened 2026-09-10, first reply by Aburke225 (COLLABORATOR) on 2026-09-16 — 6 days, inside 30.
- Recent release — fail. Zero releases and zero tags in the repo; a course repo simply doesn't cut releases.
- Beginner-friendly label — pass. Carries "good first issue" and "tier-1", applied by the collaborator who opened it.

Fit: this is Python (a rag/retriever module plus a pytest unit test) in a fairly large existing codebase — squarely the "read someone else's codebase, make a clean scoped contribution" practice from my fit profile, and away from the C++/Rust systems work I'd rather avoid.

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68",
  "checks": [
    {"name": "Repo not archived", "grade": "pass", "evidence": "GitHub API repo object: archived: False, disabled: False"},
    {"name": "Repo actively maintained", "grade": "pass", "evidence": "pushed_at 2026-09-16T21:50:20Z — 7 days before today (2026-09-23), within 90 days"},
    {"name": "Issue not already claimed", "grade": "pass", "evidence": "assignees: []; repo has 0 PRs total so no linked PR; only claim comment is from acordero4852 (author_association NONE), which the Path Review house rule says does not block"},
    {"name": "Scope fits a newcomer", "grade": "pass", "evidence": "Body scopes one bug to rag/retriever/keyword_search.py + its test, remove one xfail marker, 'Estimated effort: 2-4 hours'; no umbrella/tracking framing and no maintainer note about unresolved core internals"},
    {"name": "AI contribution policy allows this workflow", "grade": "pass", "evidence": "No root CONTRIBUTING.md / AI_POLICY.md / AGENTS.md (404); docs/CONTRIBUTING.md and PR template contain no AI-use restriction — silence passes"},
    {"name": "Maintainer responds to issues", "grade": "pass", "evidence": "Issue #52 opened 2026-09-10, first reply by Aburke225 (COLLABORATOR) on 2026-09-16 — 6 days, inside 30"},
    {"name": "Recent release", "grade": "fail", "evidence": "/releases returns 0 entries and /tags returns 0 — no release within 12 months"},
    {"name": "Beginner-friendly label", "grade": "pass", "evidence": "Labels include 'good first issue' (and 'tier-1'), added by the collaborator who opened it"}
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

**Run history**

1. Smoke test, `--limit 3`: agreement 2/3 scored items.
2. Re-graded `--only issue-01` after revising the "Scope fits a newcomer" check: agreement 1/1 scored items.
3. Full run: agreement 18/20 scored items (bar: 18/20: PASS).
4. Confirming full run with `--save-run eval-run.txt`: agreement 18/20 scored items (bar: 18/20: PASS). This matches the committed `eval-run.txt`.

**Issue analysis**

issue-15. My rubric graded it **accept**; the gold label is **reject** (category: scope). The bundle (zulip/zulip#19589) asks to add a Slack-style `command` field to outgoing webhooks. It reads as one bounded change on the surface, so my "Scope fits a newcomer" check passed it: it isn't framed as an umbrella/tracking issue, and no maintainer comment explicitly says it needs unresolved core changes. But the bundle also shows multiple contributors claimed it over about three years and were auto-unassigned after 14+ days of inactivity, with two closed, unmerged PR attempts behind it. My check never looks at that history. The evidence guide names "an issue open for years with several abandoned attempts" as its own difficulty signal, and my rubric doesn't have a check for it, so it missed a real scope problem that isn't visible from a single read of the issue body.

**Check rationale**

Current wording of "Scope fits a newcomer" in `rubric.md`:

> the issue describes one bounded piece of work: it is not explicitly described as a tracking or umbrella issue meant to be split into separate issues or PRs, it is not a pure usage question, and no maintainer comment says the fix needs core internal changes with the design still unresolved. A single issue with a checklist of related edits within one feature area still counts as bounded.

I wrote the last sentence after my rubric wrongly rejected issue-01 (a docs issue with a checklist of edits across several files, all for one feature) under the original wording, which read any checklist as an "umbrella." Adding that sentence let a single issue with several related edits still pass.

**Trade-offs**

That same loosening is what let issue-15 (and issue-20) through as false accepts: the check now only fails scope for an *explicit* "split into separate issues" framing or an *explicit* maintainer statement, so it has no way to catch an issue whose real difficulty shows up only as a pattern of repeated abandoned attempts, which is exactly what happened on issue-15. I re-ran `--only issue-15,issue-20` after the fix and confirmed both still miss. I kept the trade rather than re-tightening blindly: the fix resolved a genuine false negative (issue-01) without costing any required-check category floor, and the miss it causes is a narrower, specific gap (history-of-abandoned-attempts) rather than a broad one.

---

## Selection rationale

1. I've done Python work before, like a RAG pipeline over FIU professor reviews and a Discord bot using Python and Discord.py for Shellhacks. This issue is Python, in a `rag/retriever` module, and estimated at 2 to 4 hours. That makes it a manageable weekend task alongside my classes and Hard Rock shifts. This fits because I've already worked with Python and RAG style code, so I can actually make progress instead of learning a new language on top of learning the codebase. The 2 to 4 hour estimate is realistic given my schedule this semester.
2. The rubric correctly caught that the issue is unclaimed in any way that matters. There's one classmate comment, but the house rule says that doesn't block anyone. It also correctly caught that the scope is bounded: one file, one test, nothing vague. What the rubric can't see is that I'm comfortable reading someone else's code. I got that experience from my Vanguard internship, where I worked inside a large existing Appian codebase, and from my THREAD database project, where I had to work inside code my teammates wrote.
3. I don't expect it to be hard to claim, since there's no real competing claim, just one classmate comment. The fix itself should be low risk too, since it touches one function and one test. The main work will be setting up the project locally and understanding how the retriever is used elsewhere before I make changes.
