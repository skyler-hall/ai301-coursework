# Procedure: how this skill grades a plan package

These steps grade a plan someone else wrote. They do not help write or
fix the plan.

## Read order

1. Read the issue section first (title, body, labels). Write down, in
   one line, the behavior the issue says is broken.
2. Read the repro evidence block second, before the plan. Write down:
   the failing step and its output, every control run and what it
   showed working, and the expected vs actual lines. These notes are
   the yardstick for grounded-diagnosis and decisive-test, so they must
   come from the evidence before the plan's story can color them.
3. Read the thread highlights third. For each comment, note the
   author's role. List every comment from an OWNER, MEMBER,
   COLLABORATOR, or CONTRIBUTOR that names a culprit location, proposes
   or settles an approach, rejects an approach, or posts a patch and
   asks for testing. If there are none, write "no maintainer direction".
4. Read the repo facts block fourth. Copy the contribution policy line
   and note whether it requires AI disclosure in comments or for all AI
   use. If not, write "no disclosure required".
5. Read the candidate plan fifth, then the candidate plan comment last.
   The plan and comment are graded against notes 1 to 4, never the
   other way around.
6. In live mode, the "repro evidence" is the student's posted repro
   comment as quoted in plan.md or comment.md, the "thread highlights"
   are the issue's live comments, and the "repo facts" are the repo's
   CONTRIBUTING and templates (see the evidence guide).

## Evidence gathering

1. grounded-diagnosis: copy the plan's stated cause in one sentence.
   Next to it, list each repro step and control from read-order note 2
   and mark whether it fits or contradicts that cause.
2. bounded-scope: list every piece of work the plan and the comment
   commit to (approach steps, "in scope" items, extras in the comment).
   Mark each item "needed for this bug", "test or proof", "deferred",
   or "extra".
3. executable: copy every file, function, or code location the plan
   names, and copy the one approach it chooses. If it lists options
   without choosing, copy those options.
4. decisive-test: copy the test plan text. Write down the observable
   result it names (output, exit code, assertion, visible change), or
   write "none named".
5. thread-direction: take the list from read-order note 3. For each
   direction, search the plan comment for a mention of it (by name,
   file, approach, or patch). Record "engaged" with the quote, or
   "not engaged".
6. ai-disclosure: take read-order note 4. If disclosure is required,
   search the plan comment for a sentence saying AI was used and for
   what. Record the quote or "none".
7. honest-unknowns: copy the plan's risks or unknowns lines, if any.

## Check execution

1. Run the checks in rubric order: grounded-diagnosis, bounded-scope,
   executable, decisive-test, thread-direction, ai-disclosure,
   honest-unknowns.
2. Grade each check only from the evidence gathered for it above,
   using the rubric's pass condition word for word. Do not re-read the
   whole package for a check unless its gathered evidence is empty.
3. Grade every check, even after one has already failed. Do not stop
   early.
4. If the evidence a check needs is truly absent from the package (for
   example, the plan has no test plan at all), grade that check fail,
   not unclear, because a missing part is a fact the package shows.
5. Grade unclear only when the needed part exists but can be read two
   ways after one careful re-read. Write both readings in the evidence
   line.
6. Judge what the plan does, not how it looks. A short plan can pass
   every check; a long, confident plan can fail them.
7. For each check, write one evidence line that quotes or names the
   fact that decided it.

## Verdict assembly

1. Collect the six required grades.
2. If all six are pass, the verdict is accept.
3. If any required grade is fail or unclear, the verdict is reject.
4. Ignore honest-unknowns for the verdict; still report its grade.
5. In the summary, name the deciding check for a reject (the first
   required check in rubric order that did not pass) and quote its
   evidence line. For an accept, say all six required checks passed.
6. End with the JSON block from SKILL.md, using the same check names
   as the rubric, and put nothing after it.
