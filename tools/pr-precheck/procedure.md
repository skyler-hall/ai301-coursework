# Procedure: how this tool grades a PR package

These steps grade a pull request someone else wrote. They do not fix
the PR or rewrite its description.

## Read order

Read the yardstick before the thing being graded, so the description's
story cannot color what you expect the diff to contain.

1. Read the issue first (title, body, labels). Write one line: the
   behavior the issue says is broken.
2. Read the plan second, including its repro evidence and its
   deviation notes. Write down:
   - the scope: what is in, and what is explicitly out;
   - the files the plan says it will touch;
   - each piece of work the plan promises (one line each);
   - any work the plan or its deviation notes defer or drop;
   - the test plan, item by item: the repro command, its
     expected-after result, each control, each named test, and each
     named suite or check command.
3. Read the thread highlights third. List every OWNER, MEMBER,
   COLLABORATOR, or CONTRIBUTOR comment that names a culprit, settles
   or rejects an approach, or posts a patch. If none, write "no
   maintainer direction".
4. Read the repo facts fourth (live mode: the PR template,
   `docs/CONTRIBUTING.md`, and the house rules in `scope.md`). Write
   down: every template section, every item stated as required (issue
   link line, checklist, changelog or whatsnew or release-note entry),
   any title format, any named test or check command, and the AI
   policy. If the AI policy requires disclosure, write "disclosure
   required"; otherwise write "no disclosure required".
5. Read the diff fifth, then the commit list. List every changed file
   and, for each hunk, one line saying what it does.
6. Read the test evidence sixth.
7. Read the title and description last.

In live mode the same order applies to: the issue page and its
comments (1, 3), `plan.md` (2), the template, CONTRIBUTING, and
`scope.md` (4), the output of `git diff main...HEAD` and `git log
--oneline main..HEAD` (5), `test_evidence.md` (6), and `pr_draft.md`
(7).

## Evidence gathering

Each gather is a side-by-side. Record it as a short table or list.

1. plan-fidelity, direction one: for each hunk from read-order note 5,
   mark it "planned" (quote the plan line), "test or proof",
   "deviation note" (quote the note), "explanatory comment", or
   "extra".
2. plan-fidelity, direction two: for each piece of promised work from
   note 2, mark it "in diff" (name the hunk), "deferred or dropped with
   a note" (quote the plan or description), or "missing".
3. description-fidelity: list each factual claim the title and
   description make about the change. Next to each, write the hunk
   that supports it or "not in diff". Copy any fidelity phrase
   ("exactly", "no changes beyond", "only") word for word, and check it
   against the "extra" marks from step 1.
4. decisive-evidence: next to each test-plan item from note 2, write
   what the evidence shows for it: the output or result quoted, "stated
   with result" (quote it), or "not mentioned". For the repro item,
   also note whether the command run on the branch is the same failing
   command or input from the plan, or a different path or a control.
5. checks-run: list every suite or check command named in the plan's
   test plan and in the repo facts. Next to each, quote the outcome the
   evidence or description shows, or write "not shown".
6. clean-diff: scan every added and removed line for debris: debug
   prints or logs, commented-out code, unused or dead helpers, added
   TODO or FIXME, whitespace or re-indent-only hunks, import shuffles
   not needed by the fix, and hunks in unrelated code. Quote each one
   found, or write "none found".
7. template-and-asks: next to each section and required item from note
   4, write "filled" (quote a line), "boilerplate", "missing", or for
   entries that must be in the diff, the file that holds it or "not in
   diff". Skip asks a package cannot show (CLA, vouching, CI after
   opening, response times).
8. ai-disclosure: take note 4. If disclosure is required, quote the
   description's sentence saying AI was used and for what, or write
   "none".
9. maintainer-direction: for each item from note 3, quote where the PR
   follows or addresses it, or write "not addressed".
10. title-quality: copy the title and any stated title format.

## Check execution

1. Run the checks in rubric order: plan-fidelity,
   description-fidelity, decisive-evidence, checks-run, clean-diff,
   template-and-asks, ai-disclosure, maintainer-direction,
   title-quality.
2. Grade each check only from the evidence gathered for it, applying
   the rubric's pass condition word for word. Re-read the package for a
   check only if its gathered evidence is empty.
3. Grade every check, even after one has failed. Do not stop early.
4. If the part a check needs is truly absent (no test evidence section
   at all, no description), grade fail, not unclear. An absent part is
   a fact the package shows.
5. If the repo or plan simply has no such ask (no template, no AI
   policy, no named suite), grade pass, not unclear.
6. Grade unclear only when the part exists but can be read two ways
   after one careful re-read. Put both readings in the evidence line.
7. Judge what the PR does, not how it looks. Length, polish, and
   confident wording count for nothing.
8. Write one evidence line per check that quotes or names the deciding
   fact.

## Verdict assembly

1. Collect the seven required grades.
2. If all seven are pass, the verdict is accept.
3. If any required grade is fail or unclear, the verdict is reject.
4. Report the two preferred grades, but leave them out of the verdict.
5. For a reject, name the deciding check in the summary: the first
   required check in rubric order that did not pass. Quote its evidence
   line. List any other failed required checks after it.
6. For an accept, say that all seven required checks passed.
7. In live mode, add the voice-guide notes to the summary after the
   check lines.
8. End with the JSON block from SKILL.md, with all nine checks in
   rubric order, and put nothing after it.
