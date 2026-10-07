# Plan for #68: `KeywordSearcher.index([])` raises `ZeroDivisionError`

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68
My repro report: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68#issuecomment-5901941558

## Diagnosis

`KeywordSearcher.index()` always hands its tokenized corpus to
`BM25Okapi`, even when the corpus is empty. `rank_bm25` divides by the
corpus size while building the index, so an empty list crashes inside
the library before `index()` can finish.

The repro evidence this rests on (from my repro report, run on `main`
at `2f4e82f`):

- Step 1, `index([])`:
  ```
  File "...\rag\retriever\keyword_search.py", line 25, in index
    self.bm25 = BM25Okapi(tokenized_corpus)
  File "...\rank_bm25.py", line 52, in _initialize
    self.avgdl = num_doc / self.corpus_size
  ZeroDivisionError: division by zero
  ```
- Step 2 (control), `search()` with no index ever built:
  ```
  [warning  ] keyword_search_empty_index
  []
  ```
- Step 3, the covering test with `--runxfail`:
  ```
  rag\retriever\keyword_search.py:25: in index
  E       ZeroDivisionError: division by zero
  FAILED tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index
  ```

So the crash is in `index()` at line 25, not in `search()`. The control
in step 2 shows `search()` already returns `[]` when `self.bm25` is
`None`. The fix belongs in `index()`, and once `index()` leaves the
searcher in that same "no index" state, `search()` needs no change.

## Scope

In scope:

- An early return in `KeywordSearcher.index()` for an empty `chunks`
  list, so it does not call `BM25Okapi`.
- Removing the `@pytest.mark.xfail(strict=True, ...)` marker from
  `test_empty_index`, as the issue and `docs/CONTRIBUTING.md` ask.
  The marker is strict, so the test would XPASS and fail CI if I left
  it.

Not in scope:

- Any change to `search()`, `_tokenize()`, or `HybridRetriever`.
- Any change to `rank_bm25` or how scores are computed.
- Corpora that are non-empty but have no tokens (see Risks). That is a
  different input than the one in this issue.

## Files I'll touch

- `rag/retriever/keyword_search.py` (`KeywordSearcher.index`)
- `tests/unit/test_keyword_search.py` (`test_empty_index` marker only)

## Approach

1. Create branch `fix/68-empty-keyword-index` from `main`.
2. In `index()`, before tokenizing: if `chunks` is empty, set
   `self.chunks = []` and `self.bm25 = None`, log
   `keyword_index_built` with `chunk_count=0`, and return. Setting
   `bm25` back to `None` also clears an index left over from an
   earlier non-empty call, so a later `search()` returns `[]` instead
   of scoring old chunks.
3. Delete the xfail marker (lines 134 to 137) from `test_empty_index`.
   The test body stays as it is.
4. Run the test file and my repro commands again (test plan below).

## Test plan

Re-run my Unit 2 repro steps on the branch:

1. `python -c "from rag.retriever.keyword_search import KeywordSearcher; KeywordSearcher().index([])"`
   Expected after the fix: no traceback, exit code 0.
2. `python -c "from rag.retriever.keyword_search import KeywordSearcher; s = KeywordSearcher(); s.index([]); print(s.search('python'))"`
   Expected: the `keyword_search_empty_index` warning and `[]`, the
   same output as the step 2 control in my repro.
3. `python -m pytest tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index -q`
   Expected: `1 passed` (the marker is gone, so no `--runxfail`).
4. `python -m pytest tests/unit/test_keyword_search.py -q`
   Expected: `17 passed`, no `xfailed` and no `XPASS`. Before the fix
   this file shows `16 passed, 1 xfailed`.

## Risks and unknowns

- A corpus that is not empty but has no tokens (for example
  `index([{"text": ""}])`) also crashes, in a different place in
  `rank_bm25` (`self.average_idf = idf_sum / len(self.idf)`). I ran
  this while checking edge cases:
  ```
  > python -c "from rag.retriever.keyword_search import KeywordSearcher as K; K().index([{'text': ''}])"
  ...
      self.average_idf = idf_sum / len(self.idf)
  ZeroDivisionError: division by zero
  ```
  It is not what #68 reports, so I am
  leaving it out and can open a separate issue for it.
- I have not run the full `make check` stack (Docker and Postgres)
  locally. I will run `ruff`, `black`, `mypy`, and the unit tests on
  the changed files, and rely on CI for the rest.
- My local Python is 3.13 on Windows. I don't expect the fix to depend
  on the version, but CI will be the real check.

## Deviations

The code change held to the plan. Commit `dc2ba5c` on
`fix/68-empty-keyword-index` touches only the two files listed above:
an early return in `index()` that sets `self.chunks = []` and
`self.bm25 = None` and logs `keyword_index_built` with
`chunk_count=0`, and the removed xfail marker on `test_empty_index`.
The only thing I added that the plan did not spell out is a short
code comment above the early return saying why it is there (BM25Okapi
divides by the corpus size). It does not change behavior.

One difference from the test plan: I ran the after checks in a Linux
shell with Python 3.10, not the Windows Command Prompt with Python
3.13 from my repro report. All four expected results matched
(exit code 0, `[]`, `1 passed`, `17 passed`). `ruff`, `black --check`,
and `mypy` on the changed files also passed. CI on the pull request
will cover the full suite on the repo's Python version.

The empty-text corpus crash from Risks is still out of scope and
unchanged.
