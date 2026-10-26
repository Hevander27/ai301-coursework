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

Hevander27

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/55#issuecomment-6407071626

````markdown
## Issue #55 — Implementation Plan

### 1. Reproduction & Root Cause
- Reproduced the issue using `--runxfail`:
  - **5 failed, 13 passed**
- Confirmed JavaScript detection inconsistency:
  - Without filename: `[]`
  - With `app.js`: `['JavaScript']`
- Identified narrow detection patterns in `ingestion/parsers/skill_extractor.py`:
  - **JavaScript:** `require('fs')` misses the `require\s+` pattern.
  - **TypeScript:** Detection relies only on `.ts` filenames.
  - **PostgreSQL:** `psycopg2` isn't recognized as a PostgreSQL dependency.
  - **Docker:** Dockerfile and Compose structures aren't detected without the word `docker`.
- The remaining causes were identified through code inspection, not separate test runs.

### 2. Proposed Changes
- Update the following functions:
  - `_detect_languages`
    - Recognize `require(...)` calls.
    - Detect TypeScript-specific syntax.
  - `_detect_databases`
    - Map `psycopg` dependencies to PostgreSQL.
  - `_detect_tools`
    - Recognize Dockerfile and Docker Compose structures.
- Remove the five `xfail` markers associated with #55, following `CONTRIBUTING` guidelines.

### 3. Out of Scope
- No changes to confidence scoring.
- No changes to unrelated technology detectors.
- Preserve existing behavior where Python `import` statements also produce a JavaScript entry.
  - Document this separately in the PR.

### 4. Testing & Expected Results
- Re-run the original reproduction.
- Expected results:
  - **Target test file:** `18 passed`, with and without `--runxfail`.
  - **JavaScript text-only control:** `['JavaScript']`.
  - **Full unit suite:** From `375 passed, 53 xfailed` to `380 passed, 48 xfailed`.
  - **SQL `FROM` snippet:** Continues to return no detections.

### 5. Open Question
- When both JavaScript and TypeScript signals appear in the same text:
  - Proposed behavior: **Report TypeScript**.
  - Flag this decision for review.

### 6. PR Considerations
- Reviewed existing PRs **#192** and **#198**.
- Implementation is independently developed from my own reproduction.
- Any overlap with existing PRs will be explicitly acknowledged in my submission.
````

---

## Your branch

**Branch**

fix/55-skill-extractor-detection-signals

**Evidence**

Repo: `codepath/pathreview-ai301-fa26-s1`. Before = `main` at `f89c06f` (the output from my Unit 2 repro comment). After = my branch `fix/55-skill-extractor-detection-signals` at `d670fe9`. Same machine: macOS 15.6.1, Python 3.11.15, pytest 9.1.1.

**Step 1: the issue's command**

```bash
.venv/bin/pytest tests/unit/test_skill_extractor.py -q -rx
```

Before:

```text
13 passed, 5 xfailed in 0.60s
```

After (the five #55 xfail markers removed, as CONTRIBUTING asks):

```text
18 passed in 0.12s
```

**Step 2: same tests with xfail markers ignored**

```bash
.venv/bin/pytest tests/unit/test_skill_extractor.py -q --runxfail
```

Before:

```text
FAILED tests/unit/test_skill_extractor.py::TestSkillExtractor::test_text_with_typescript_files
FAILED tests/unit/test_skill_extractor.py::TestSkillExtractor::test_database_technology_detection
FAILED tests/unit/test_skill_extractor.py::TestSkillExtractor::test_devops_tool_detection
FAILED tests/unit/test_skill_extractor.py::TestSkillExtractor::test_javascript_detection
FAILED tests/unit/test_skill_extractor.py::TestSkillExtractor::test_docker_compose_detection
5 failed, 13 passed in 0.10s
```

After:

```text
18 passed in 0.09s
```

**Step 3: the filename control**

```python
from ingestion.parsers.skill_extractor import SkillExtractor
e = SkillExtractor()
js = "const fs = require('fs');\nconst data = fs.readFileSync('file.txt');\nconsole.log(data);\n"
print([s.name for s in e.extract_skills(js)])
print([s.name for s in e.extract_skills(js, filename="app.js")])
```

Before:

```text
[]
['JavaScript']
```

After:

```text
['JavaScript']
['JavaScript']
```

**Step 4: whole unit suite, regression check**

```bash
.venv/bin/pytest tests/unit -q
```

Before:

```text
375 passed, 53 xfailed, 4 warnings in 13.62s
```

After:

```text
380 passed, 48 xfailed, 4 warnings in 8.06s
```

**Step 5: SQL false-positive check**

```python
print([s.name for s in SkillExtractor().extract_skills('SELECT name\nFROM users\nWHERE id = 1')])
```

Before: `[]`. After: `[]`.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

One full run: `agreement: 20/20 scored items  (bar: 18/20: PASS)`, with `categories: clear-accept 7/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`. This is the run in `eval-run.txt`. Because it already agreed on all 20, I did no `--only` re-runs and no second full run.

**Package analysis**

`pkg-14` (zellij-org/zellij, category `clear-accept`). My rubric decided `accept`; the gold label is `accept` ("honestly scoped-down ... defers the untestable Windows variant and says so; arguable on the deferral, ready as scoped").

It is one of the arguable ones, because the plan leaves two things open: it defers a Windows variant reported in the thread, and it says the "exact functions to be pinned in the PR after tracing the query issuance with debug logs". A stricter rubric could fail it on scope or on executability. Mine passed both on purpose. `scope-bounded` passed with "One change to the Unix reattach handshake; Windows variant and theme-detection caching explicitly deferred with reasons", because my pass condition says "Explicit deferrals with reasons ... are fine and do not fail." `executable` passed with "Names the zellij-server client attach/reattach path and zellij-client query issuance; approach is draining pending OSC responses before pane input is wired", because my check accepts a chosen code area plus a chosen approach and says "Pinning exact function names during the build is fine once the area and the approach are chosen." The diagnosis also passed because it predicts every control in the repro (clean fresh attach, leak on reattach, clean 0.44.1, the cache control), and the test plan names an observable result: "5 consecutive SSH reattach cycles with no rgb strings".

**Check rationale**

From `tools/plan-check/rubric.md`, check `executable`, pass condition as it reads now:

> Pass if a stranger could start the work without asking the author anything: the plan names the files or the specific code area where the change goes, AND commits to one approach (what the code will do differently). Pinning exact function names during the build is fine once the area and the approach are chosen. Fail if no files or code area are named; if the approach is left open ("somewhere", "X or Y, whichever is easier", "not sure which layer"); or if the plan is an investigation with no chosen change ("profile and optimize", "poke around", "investigate the input stack").

It reads this way because I first wanted to require named files and functions, and I rejected that before running it. Reading the packages, `pkg-14` is a gold accept that names only a code area ("the client attach/reattach path in `zellij-server`") and leaves the exact functions for the build, while `pkg-17` and `pkg-18` are gold rejects that also lack exact functions. What separates them is not how precise the file list is, but whether a decision has been made. So the check asks for an area plus one chosen approach, and its fail cases quote the specific ways a plan defers the decision ("whichever is easier", "not sure which layer", "investigate"). In my run that split held: `pkg-18` failed with "no files named; 'somewhere around linter execution' and 'upstream or in the vendored copies, whichever turns out to be easier'", and `pkg-14` passed.

**Trade-offs**

The sentence "Pinning exact function names during the build is fine once the area and the approach are chosen" gives up precision. A plan that names a broad area ("the networking code") plus a confident one-line approach could pass this check even though a stranger would still have to hunt for the right place, and I accept that this check will miss that case. I chose it because requiring exact functions would have rejected `pkg-14`, a gold accept, and failing honest plans that leave a detail to the build is worse than occasionally passing a vague-but-decided one.

Nothing else changed, and I know because there was only one rubric version: the single full run agreed on all 20, so there was no revision that could flip another package. The other `unbuildable` packages failed this check for the reasons it names: `pkg-10` with "no files or code area named; approach is 'Investigate', 'Look into', 'Optimize whatever the profiling turns up'" and `pkg-17` with "'gocui? tcell? not sure which layer'".

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
