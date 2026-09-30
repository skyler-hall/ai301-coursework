# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

skyler-hall

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68#issuecomment-5901495124

Hi, I'd like to work on this one. The issue says `KeywordSearcher.index([])` in `rag/retriever/keyword_search.py` raises a `ZeroDivisionError` on an empty corpus, and `test_empty_index` in `tests/unit/test_keyword_search.py` is marked xfail for it.

My next step is to set up the repo from its README on my machine, run that test and a short script that calls `index([])` then `search()`, and post a repro report here with my environment and the exact output I get, whether or not it reproduces.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68#issuecomment-5901941558

Repro report for #68: reproduced on current `main`.

**Environment**

- OS: Windows 11 (build 10.0.26100), Command Prompt
- Python 3.13.7
- Code: my fork of this repo, `main` at commit `2f4e82f` (up to date with `codepath/pathreview-ai301-fa26-s3:main`), no source changes
- Packages: `rank-bm25==0.2.2`, `structlog==26.1.0`, `pytest==9.1.1`, `numpy==2.5.3` in a fresh venv

I did not run the full `make setup` stack (Docker/Postgres). `rag/retriever/keyword_search.py` only imports `rank_bm25` and `structlog`, and `rag/__init__.py` and `rag/retriever/__init__.py` are empty, so those two packages plus pytest are enough to exercise this code path.

**Steps**

From my home folder (`C:\Users\Yeyian`), which is why the venv paths in the tracebacks sit one level above the repo:

```
git clone https://github.com/skyler-hall/pathreview-ai301-fa26-s3.git
python -m venv .venv-repro
.venv-repro\Scripts\activate
pip install rank-bm25==0.2.2 structlog==26.1.0 pytest==9.1.1
cd pathreview-ai301-fa26-s3
```

(I ran the install unpinned; the pins above are the versions `pip freeze` reported afterward.)

1. Trigger, calling `index([])`:

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
```

2. Control, calling `search()` without ever calling `index()`:

```
> python -c "from rag.retriever.keyword_search import KeywordSearcher; print(KeywordSearcher().search('python'))"
2026-09-29 20:18:17 [warning  ] keyword_search_empty_index
[]
```

3. The covering test with its xfail marker ignored:

```
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

(`...` marks lines I trimmed from the traceback; nothing else was changed.)

**Expected:** `index([])` builds an empty index without raising, and a following `search()` returns `[]`, the same way `search()` already handles an index that was never built (step 2).

**Actual:** `index([])` raises `ZeroDivisionError: division by zero` from `BM25Okapi(tokenized_corpus)` at `keyword_search.py` line 25, inside `rank_bm25`'s `_initialize` (`num_doc / self.corpus_size` with an empty corpus). The test fails the same way at line 140. This matches the issue.

Next I'll look at guarding the empty case in `index()` and removing the xfail marker, as the issue describes.

I used Claude to help me organize this report. I ran every command above myself on my machine, and the output is copied from my terminal.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run: agreement 19/20 scored items (bar: 18/20: PASS). pkg-03 disagreed: my rubric rejected it on claims-backed.
2. Partial re-run after revising claims-backed, `--only pkg-03,pkg-13,pkg-15,pkg-17,pkg-20,calib-02 --include-calibration`: agreement 5/5 scored items, and calib-02 also matched.
3. Confirming full run with `--save-run eval-run.txt`: agreement 20/20 scored items (bar: 18/20: PASS), every category matched (clear-accept 8/8, disclosure 1/1, no-evidence 4/4, unfollowable-comms 3/3, wrong-target 4/4). This matches the committed `eval-run.txt`.

**Package analysis**

pkg-03 (BurntSushi/ripgrep#2779). On my first run my rubric graded it **reject**; the gold label is **accept** (category: clear-accept). The report shows the exact 12-line file and the issue's command with its wrong output (`1:fnord 2:boccob 3:d321fdddffff 4:clowns`), states the version difference (filed on 13.0.0, tested on 15.2.0), and then adds one sentence: "Dropping `-r '$1'` from the same command reports 1, 4, 7, 10 correctly." My claims-backed check failed it because that control run was only described in prose, with no pasted output. That was my rubric being too literal: the headline claim (the bug reproduces on 15.2.0) was fully backed by a shown artifact, and the control sentence named its exact command, so anyone could re-run it. It was a side observation, not the proof. After I revised the check to require artifacts for headline claims only, my rubric graded pkg-03 accept on the next two runs, matching the gold label.

**Check rationale**

Current wording of claims-backed in `rubric.md`:

> | claims-backed | Every factual claim in the claim comment and the report ("reproduced", "confirmed on X", "not limited to Y", a stated root cause, "guaranteed", "100%", expected vs actual) read against the artifacts actually shown | Pass if every headline claim (that the bug reproduced or did not, where, on what scope, and any root cause) is supported by an artifact in the package or is clearly marked as a guess or hypothesis, AND the expected/actual lines agree with what the artifacts show. A side observation that names its exact command (for example a control run described in one sentence, "dropping flag X gives the correct result") does not need its own pasted output, as long as the headline claim has one. Fail if the package asserts reproduction, a root cause, or a wider scope (another release, another platform) that no shown artifact supports; or narrates an artifact as something it is not (calling a graceful error or garbled output a crash); or states expected/actual backwards from what was shown. | required |

The sentence "A side observation that names its exact command ... does not need its own pasted output, as long as the headline claim has one" is the revision. The first version said "Pass if every claim is supported by an artifact in the package," which treated every sentence as needing its own paste, so it failed pkg-03 for describing a control run in words. I kept the fail list (unbacked reproduction, root cause, or wider scope, and narrating an artifact as something it is not) because that is what catches the no-evidence and wrong-target packages like pkg-15's "I verified this race condition" and pkg-17 calling garbled output a crash.

**Trade-offs**

The revision loosened claims-backed, so it could have flipped packages that were rejected for unbacked claims. Before spending a full run I re-ran `--only` with canaries: pkg-15 and pkg-13 (no-evidence, confident claims with no artifact), pkg-17 (wrong-target, garbled output narrated as a crash), pkg-20 (the only disclosure package, a single-package category), and calib-02 as a free trap check. All still matched gold, and the confirming full run stayed at 20/20 with every category matched. The case I accept it will now miss: a report whose only proof of the bug is a one-sentence description with a command, if the grader treats that sentence as a "side observation." The rule only relaxes for claims that sit next to a headline claim that already has pasted output, so a report with no pasted output at all still fails.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
