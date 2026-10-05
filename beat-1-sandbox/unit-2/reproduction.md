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

Hevander27

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/55#issuecomment-5988766107

````markdown
Hi, I'd like to work on #55 as part of my AI301 coursework.

My first step is to run the five tests this issue names in `tests/unit/test_skill_extractor.py` on a clean checkout of `main`, with `--runxfail` so the actual assertion failures show instead of `xfailed`. I'll do that on macOS, since the reports so far are from Windows and WSL. Then I'll post my environment, the exact commands, and the output here.

After that I'll read `_detect_languages` in `ingestion/parsers/skill_extractor.py` to see why JavaScript/TypeScript files, that arrive without a `.js`/`.ts` filename, a `package.json` mention, or a matching `import`/`require` line, gets no language entry.
````

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/55#issuecomment-5988808897

````markdown
Reproduced #55 on macOS on an unmodified checkout of `main`.

**Environment**
- macOS 15.6.1 (Apple Silicon, arm64)
- Python 3.11.15 (installed with uv; the system Python is 3.9, below the repo's 3.11 minimum)
- pytest 9.1.1
- Repo: `codepath/pathreview-ai301-fa26-s1` at `f89c06f` (main), `git status --porcelain` empty before and after
- Setup: the Python steps from `make setup` / `docs/SETUP.md` (venv plus `pip install -e ".[dev]"`). I skipped Docker, the migrations, the seed data, and the frontend install, because these unit tests don't touch the database or the UI.

**Steps**
```bash
git clone https://github.com/codepath/pathreview-ai301-fa26-s1.git
cd pathreview-ai301-fa26-s1
git checkout f89c06f
cp .env.example .env
python3.11 -m venv .venv
.venv/bin/python -m pip install --upgrade pip setuptools wheel
.venv/bin/pip install -e ".[dev]"

# 1. the issue's command
.venv/bin/pytest tests/unit/test_skill_extractor.py -q -rx

# 2. same tests with the xfail markers ignored, to see the real failures
.venv/bin/pytest tests/unit/test_skill_extractor.py -q --runxfail
```

**Observed, step 1**
```text
13 passed, 5 xfailed in 0.60s
```
All five are marked `issue #55: skill extractor does not detect JavaScript/TypeScript`: `test_text_with_typescript_files`, `test_database_technology_detection`, `test_devops_tool_detection`, `test_javascript_detection`, `test_docker_compose_detection`.

**Observed, step 2** (trimmed to the failing assertion of each test, plus the summary)
```text
>       assert any("typescript" in s.lower() for s in skill_names)
E       assert False
tests/unit/test_skill_extractor.py:65: AssertionError
>       assert any("postgres" in s.lower() or "sql" in s.lower() for s in skill_names)
E       assert False
tests/unit/test_skill_extractor.py:146: AssertionError
>       assert any("docker" in s.lower() for s in skill_names)
E       assert False
tests/unit/test_skill_extractor.py:162: AssertionError
>       assert any("javascript" in s.lower() or "js" in s.lower() for s in skill_names)
E       assert False
tests/unit/test_skill_extractor.py:195: AssertionError
>       assert any("docker" in s.lower() for s in skill_names)
E       assert False
tests/unit/test_skill_extractor.py:213: AssertionError
FAILED tests/unit/test_skill_extractor.py::TestSkillExtractor::test_text_with_typescript_files
FAILED tests/unit/test_skill_extractor.py::TestSkillExtractor::test_database_technology_detection
FAILED tests/unit/test_skill_extractor.py::TestSkillExtractor::test_devops_tool_detection
FAILED tests/unit/test_skill_extractor.py::TestSkillExtractor::test_javascript_detection
FAILED tests/unit/test_skill_extractor.py::TestSkillExtractor::test_docker_compose_detection
5 failed, 13 passed in 0.10s
```

**Control: same text, with and without a filename**

I ran the JavaScript snippet from `test_javascript_detection` through the extractor twice, changing only the `filename` argument:

```python
from ingestion.parsers.skill_extractor import SkillExtractor
e = SkillExtractor()
js = "const fs = require('fs');\nconst data = fs.readFileSync('file.txt');\nconsole.log(data);\n"
print([s.name for s in e.extract_skills(js)])
print([s.name for s in e.extract_skills(js, filename="app.js")])
```
```text
[]
['JavaScript']
```

**Expected:** the extractor reports JavaScript (or TypeScript) for code like the test snippets, and the five tests above pass.

**Actual:** with text alone, no JavaScript or TypeScript entry is produced and all five tests fail their assertions. The same JavaScript text is detected as soon as a `.js` filename is passed, which matches the issue's description that detection leans on the `filename` argument. Same result as the Windows and WSL reports above, now on macOS.

Next I'll read `_detect_languages`, `_detect_tools`, and `_detect_databases` against these five test inputs before proposing a change.
````

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Two full runs, no partial (`--only`) runs, in this order:

1. Full run with my first rubric: `agreement: 20/20 scored items  (bar: 18/20: PASS)`, with `categories: clear-accept 8/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4`.
2. Full run after tightening the `claim-comment-specific` check (see Check rationale): `agreement: 20/20 scored items  (bar: 18/20: PASS)`, same category line. This is the run saved in `eval-run.txt`.

Final score: 20/20.

**Package analysis**

`pkg-20` (ghostty-org/ghostty#13604, category `disclosure`). My rubric decided `reject`; the gold label is `reject`.

The repro itself is excellent, and my rubric said so: every proof check passed. For example, `behavior-matches-issue` passed with "Step 1 reproduces the issue's exact trigger and returns `997;2` (light), matching the issue's reported wrong symptom; control run returns `997;1`", and the optional `control-run` check passed too. The only failure was `ai-disclosure`: "Policy requires disclosure of 'all AI usage in any form' covering comments; neither the claim comment nor the repro report states AI use."

It read the package that way because my `ai-disclosure` check tells the grader to "Treat every eval package as AI-assisted work, as course packages are", and to fail "only when the policy requires disclosure that covers issue comments or all AI usage in any form, and neither comment states AI use." Ghostty's repo-facts line says "All AI usage in any form must be disclosed", which is the comment-covering kind. I also wrote the verdict rule so that "a strong repro report does not rescue ... a missing disclosure", which is why a near-perfect package still held. The same check passes pkg-03 (ripgrep's "own words" rule) and pkg-09 (fd's disclosure rule is scoped to pull requests), because those policies put no disclosure duty on issue comments.

**Check rationale**

From `tools/repro-check/rubric.md`, check `claim-comment-specific`, pass condition as it reads now:

> Pass if the claim comment refers to something particular to this issue (its symptom, trigger, component, version, or a pointer from the thread), so it could not be pasted unchanged onto a different issue, AND names a concrete next action the author will take on this issue (what they will investigate, trace, test, read, or set up next). Wanting the issue, claiming it, or a general offer to help is not by itself a next action. Fail if it names no next action; if it is a +1, a me-too, or a request for updates; if it is interchangeable boilerplate (praise for the project, "please assign this to me", "reserve it for me") that would fit any issue; if it promises an outcome or a deadline ("a fix guaranteed by Friday"); or if it self-assigns.

Why it reads this way: my first version said the comment must state "an intent the author can act on now (what they will investigate, test, or read next, or that they want to take the issue on)". When I hand-graded `calib-02` as a warm-up, its claim comment ("+1!! ... search is basically broken ... claiming this one, someone has to do it 🙏") named the symptom and said the author wanted the issue, so it could pass the old wording. That package was rejected anyway on other checks, but in live mode a claim-only draft is graded mostly by this check, so a "+1, claiming this" draft could have gotten through. I rejected "wants to take the issue on" as enough and now require a concrete next action, and I added the sentence "Wanting the issue, claiming it, or a general offer to help is not by itself a next action" so the grader cannot read the old loophole back in.

**Trade-offs**

Tightening the check changed one package's result. In run 2, `pkg-15`'s claim comment went from pass to fail on `claim-comment-specific`: "Comment names no concrete next action; 'I should be able to have a fix approach to discuss soon' promises an outcome rather than stating what will be investigated next." Its verdict did not change, because it was already rejected on `behavior-matches-issue` ("No artifact (output, log, screenshot) is present"). A tighter check can only turn accepts into rejects, so before re-running I checked that all 8 gold-accept claim comments name a next step, and run 2 confirmed it: all 8 still accepted.

What I give up: a short, sincere claim like "I'd like to take this one" now fails until it says what I will do first, even though a maintainer might be happy with it. I accept that, because my own claims should always name a first step. I also saw three grades on checks I did not change move between runs (`pkg-06` steps, `pkg-17` environment, `pkg-19` claims); none changed a verdict, because each of those packages also fails another required check.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
