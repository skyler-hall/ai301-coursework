# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

skyler-hall

**Plan comment**

PASTE-COMMENT-LINK-HERE

Plan for #68, built on my repro report above (`main` at `2f4e82f`).

**Cause:** `KeywordSearcher.index()` passes the tokenized corpus to `BM25Okapi` even when it is empty, and `rank_bm25` divides by the corpus size (`self.avgdl = num_doc / self.corpus_size`), so `index([])` raises `ZeroDivisionError` at `keyword_search.py` line 25. My control run showed `search()` already returns `[]` with the `keyword_search_empty_index` warning when no index exists, so the fix only needs to be in `index()`.

**Change:**
- In `rag/retriever/keyword_search.py`, return early from `index()` when `chunks` is empty: set `self.chunks = []` and `self.bm25 = None`, log `chunk_count=0`, and skip `BM25Okapi`. Resetting `bm25` also clears any older index.
- In `tests/unit/test_keyword_search.py`, remove the strict xfail marker from `test_empty_index`, as the issue and CONTRIBUTING ask.
- Not changing `search()`, `HybridRetriever`, or anything in `rank_bm25`.

**Test:** re-run my repro steps. `index([])` should exit with no traceback, `index([])` then `search("python")` should print `[]`, `test_empty_index` should pass without `--runxfail`, and `tests/unit/test_keyword_search.py` should go from `16 passed, 1 xfailed` to `17 passed`.

**Open question:** a corpus with only empty text also crashes, at a different line in `rank_bm25`. When I ran `KeywordSearcher().index([{"text": ""}])` it ended with:

```
    self.average_idf = idf_sum / len(self.idf)
ZeroDivisionError: division by zero
```

That is not what this issue reports, so I'm leaving it out of this fix and can open a separate issue for it.

Branch will be `fix/68-empty-keyword-index` on my fork.

I used Claude to help me draft this plan. I checked the code and ran the commands myself.

---

## Your branch

**Branch**

fix/68-empty-keyword-index

**Evidence**

Before (on `main` at `2f4e82f`, from my Unit 2 repro report, Windows 11, Python 3.13.7):

```
> python -c "from rag.retriever.keyword_search import KeywordSearcher; KeywordSearcher().index([])"
Traceback (most recent call last):
  ...
  File "C:\Users\Yeyian\pathreview-ai301-fa26-s3\rag\retriever\keyword_search.py", line 25, in index
    self.bm25 = BM25Okapi(tokenized_corpus)
  File "C:\Users\Yeyian\.venv-repro\Lib\site-packages\rank_bm25.py", line 83, in __init__
    super().__init__(corpus, tokenizer)
  File "C:\Users\Yeyian\.venv-repro\Lib\site-packages\rank_bm25.py", line 27, in __init__
    nd = self._initialize(corpus)
  File "C:\Users\Yeyian\.venv-repro\Lib\site-packages\rank_bm25.py", line 52, in _initialize
    self.avgdl = num_doc / self.corpus_size
ZeroDivisionError: division by zero

> python -m pytest tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index -q --runxfail
F
...
>       searcher.index([])
tests\unit\test_keyword_search.py:140:
rag\retriever\keyword_search.py:25: in index
    self.bm25 = BM25Okapi(tokenized_corpus)
...
E       ZeroDivisionError: division by zero
FAILED tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index - ZeroDivisionError: division by zero
1 failed in 0.27s
```

Before, whole test file on `main` (Linux shell, Python 3.10.12):

```
$ python -m pytest tests/unit/test_keyword_search.py -q
........x........                                                        [100%]
16 passed, 1 xfailed in 0.21s
```

After (on `fix/68-empty-keyword-index` at `dc2ba5c`, Linux shell, Python 3.10.12):

```
$ python -c "from rag.retriever.keyword_search import KeywordSearcher; KeywordSearcher().index([])"
2026-10-07 00:42:00 [info     ] keyword_index_built            chunk_count=0
exit code: 0

$ python -c "from rag.retriever.keyword_search import KeywordSearcher; s = KeywordSearcher(); s.index([]); print(s.search('python'))"
2026-10-07 00:42:00 [info     ] keyword_index_built            chunk_count=0
2026-10-07 00:42:00 [warning  ] keyword_search_empty_index
[]

$ python -m pytest tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index -q
.                                                                        [100%]
1 passed in 0.22s

$ python -m pytest tests/unit/test_keyword_search.py -q
.................                                                        [100%]
17 passed in 0.13s
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run 1: 19/20 (categories: clear-accept 6/7, scope-creep 4/4, thread-convention 2/2, unbuildable 3/3, wrong-cause 4/4). The one miss was pkg-14, failed on `executable`.
2. Partial run, `--only pkg-14`: 0/1 (before the fix, to read the check's evidence line).
3. Partial run after loosening `executable`, `--only pkg-14,pkg-10,pkg-17,pkg-18`: 4/4.
4. Full run 2: 20/20 (categories: clear-accept 7/7, scope-creep 4/4, thread-convention 2/2, unbuildable 3/3, wrong-cause 4/4). This is the run in `eval-run.txt`.

**Package analysis**

pkg-14 (zellij-org/zellij#5174, clear-accept). Gold label: accept. In my first full run my rubric said reject, failing only `executable`. The grader's evidence line was: "Names only crates/subsystems ('client attach/reattach path in zellij-server', 'zellij-client's terminal query issuance') and says 'exact functions to be pinned in the PR after tracing'; no concrete file or function." My first version of the check required "at least one concrete file, function, or code location", and the grader read a code path inside a crate as not concrete enough. But the plan does commit to one change ("consuming or draining pending OSC query responses in the client attach path ... before pane input is wired"), names the code path, and says how the exact function will be found (`zellij --debug` output it already has). A stranger could start on that. The difference from the real unbuildable packages (pkg-10, pkg-17, pkg-18) is that those never choose a change at all. After I reworded the check, run 2 graded pkg-14 accept, matching gold.

**Check rationale**

| executable | The plan's approach: the files, functions, or code locations it names, and the change it says it will make there. | Pass if a stranger could start the change today without asking the author anything: the plan names where the change goes AND commits to one chosen change there. "Where" can be a file, a function, or a specific code path inside a named module (for example "the reattach path in `zellij-server`'s client connection handling"). Leaving the exact function or line to be pinned during the build is fine when the code path and the change are both named. Fail if the plan only says it will "investigate", "explore", "profile", or "look into" things, lists options without choosing ("upstream or vendored, whichever is easier", "gocui? tcell?"), or names no file or location. | required |

It reads this way because of pkg-14. My first version said the plan must name "at least one concrete file, function, or code location AND commits to one chosen approach", and the word "concrete" made the grader demand a file path or function name. That failed a plan that was clear about what to change and where. I moved the weight from "how exact is the location" to "is there one chosen change in a named place", because that is what a stranger needs to start. I kept the fail list of "investigate / explore / profile" words and the "options without choosing" quotes so the vague plans still fail on the thing that really makes them unbuildable: no decision.

**Trade-offs**

Loosening `executable` risks letting through a plan that names a module path and a change but is still too vague to start. To check that the change didn't flip any package that agreed before, I re-ran the three unbuildable packages as canaries with `--only pkg-14,pkg-10,pkg-17,pkg-18`. pkg-10, pkg-17, and pkg-18 all stayed reject, and pkg-14 flipped to accept. The confirming full run then showed no other package changed (20/20, every category matched). The case I accept it will miss: a plan that names a real code path and one change, but where that change is the wrong layer. This check won't catch that; it is left to `grounded-diagnosis`.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
