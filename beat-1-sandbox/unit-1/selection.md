# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/55

**Verdict output**

My skill's live-mode output, pasted verbatim. One live run graded three candidates (#61, #55, #66); #55 is the second entry in the JSON block, with verdict `accept`.

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

````
## Summary

All three candidates are from the scoped repo and clear the repo-wide checks: the maintainer (Aburke225) has made commits within the last 19 days, the repo is active (pushed 2026‑09‑16, not archived), and `docs/CONTRIBUTING.md` says nothing that bans AI-assisted work (several merged-in-spirit comments even include AI disclosures, confirming conditions-not-bans). Per the Path Review house rule, the multiple student claim comments on each issue don't block them — none has an assignee or an open/merged PR closing it.

**Ranked (all accepted):**

1. **#61 — Health check DB probe raw SQL string** (best fit). Confirmed by ~7 independent reproductions as a one-line fix (wrap `"SELECT 1"` in `sqlalchemy.text()`), carries an explicit `good first issue` label, and is trivially testable via `curl /health` or a unit test — matches your preference for a tightly bounded, easily-verified fix in backend code.
2. **#55 — Skill extractor misses JS/TS**. Bounded to `ingestion/parsers/skill_extractor.py` with 5 named failing tests pinpointing exactly what to widen; `tier-1` labeled. Good Python scope, slightly more surface area than #61 (three detector functions).
3. **#66 — structlog not captured by caplog**. Bounded to configuring structlog in `tests/conftest.py`; one reproducer noted the title's "suite-wide" claim doesn't hold (only one test file uses `caplog` currently), which narrows scope favorably but makes it more of a test-infra tweak than a product fix.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/61",
    "checks": [
      {"name": "Maintainer activity", "grade": "pass", "evidence": "Owner Aburke225 committed to main on 2026-09-16, 19 days before capture date 2026-10-05"},
      {"name": "Repository in use", "grade": "pass", "evidence": "repo not archived; pushed_at 2026-09-16T21:48:27Z, 19 days before capture date"},
      {"name": "Newcomer-sized scope", "grade": "pass", "evidence": "Six independent reproductions confirm a one-line fix: wrap 'SELECT 1' in sqlalchemy.text() in api/routes/health.py:32"},
      {"name": "Issue unclaimed", "grade": "pass", "evidence": "assignees: [] on the issue; search for PRs closing #61 returns none; house rule ignores the six student claim comments"},
      {"name": "AI contribution policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no AI policy language; multiple accepted comments include AI-use disclosures"},
      {"name": "First-issue signal", "grade": "pass", "evidence": "issue carries the 'good first issue' label"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/55",
    "checks": [
      {"name": "Maintainer activity", "grade": "pass", "evidence": "same commit evidence as above"},
      {"name": "Repository in use", "grade": "pass", "evidence": "same repo-facts evidence as above"},
      {"name": "Newcomer-sized scope", "grade": "pass", "evidence": "Bug is bounded to _detect_languages/_detect_tools/_detect_databases in ingestion/parsers/skill_extractor.py, with 5 named xfailed tests marking exactly what to fix"},
      {"name": "Issue unclaimed", "grade": "pass", "evidence": "assignees: [] on the issue; no PR found closing #55; house rule ignores the three student claim comments (jing2003, TeooL, Btran206)"},
      {"name": "AI contribution policy", "grade": "pass", "evidence": "same CONTRIBUTING.md evidence as above"},
      {"name": "First-issue signal", "grade": "pass", "evidence": "labeled 'tier-1' (description: 'Starter difficulty'), though not explicitly 'good first issue'"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/66",
    "checks": [
      {"name": "Maintainer activity", "grade": "pass", "evidence": "same commit evidence as above"},
      {"name": "Repository in use", "grade": "pass", "evidence": "same repo-facts evidence as above"},
      {"name": "Newcomer-sized scope", "grade": "pass", "evidence": "Fix is bounded to configuring structlog in tests/conftest.py; a reproducer noted caplog is currently used only in test_batch_processor.py, narrowing rather than widening scope"},
      {"name": "Issue unclaimed", "grade": "pass", "evidence": "assignees: [] on the issue; no PR found closing #66 (search hit was an unrelated PR closing #22); house rule ignores the claim comments (cgudumotu, jesswsong x2)"},
      {"name": "AI contribution policy", "grade": "pass", "evidence": "same CONTRIBUTING.md evidence as above"},
      {"name": "First-issue signal", "grade": "pass", "evidence": "labeled 'tier-1' (Starter difficulty), though not explicitly 'good first issue'"}
    ],
    "verdict": "accept"
  }
]
```
````

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

In order (all on 2026-09-23, times UTC):

1. ~07:07, full run attempted: every package errored with "You've hit your limit" (Claude usage limit), so it produced no agreement score.
2. 22:09, full run: 17/20. Missed `issue-04`, `issue-15`, `issue-19`, all on `Newcomer-sized scope`.
3. 22:37, `--only issue-04,issue-15,issue-19` after revising `Newcomer-sized scope`: 3/3 (partial run, no bar verdict).
4. 23:20, full run: 19/20. Missed `issue-20`.
5. 23:25, full run: 19/20. Missed `issue-20`.
6. 23:31, full run with `--save-run eval-run.txt`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`, with `categories: claimed 4/4  clear-accept 8/8  dead-repo 3/3  policy 1/1  scope 4/4`. This is the committed `eval-run.txt`.

Runs 3 through 6 all used the same rubric, the one uploaded to `tools/issue-select/rubric.md`.

**Issue analysis**

`issue-15` (zulip/zulip#19589, category `scope`). My final rubric decided `reject`; the gold label is `reject` ("years of design debate and two abandoned PRs behind a friendly label").

My first rubric got it wrong. In run 2 it accepted, because my old scope check only asked whether "the issue asks for one bounded contribution", and the issue text on its own does read as one bounded change. The grader's evidence was "Single bounded change: split bot mention into `command` field and remainder into `text` for Slack-compatible outgoing webhook; no umbrella/core-internals statement." Nothing in that check looked at the issue's history, and my `Issue unclaimed` check deliberately treats "Closed unmerged PRs are abandoned attempts rather than active claims", so the abandoned PRs never counted against it anywhere.

The revised check adds "Also fail when an old issue shows a repeated pattern of abandoned contributor attempts or multiple closed unmerged implementation PRs that provides evidence the task is substantially harder or less settled than its newcomer-facing label suggests." In the final run the scope check failed with "Open since 2021 with roughly a dozen claimants auto-unassigned after 14 days of inactivity, and two linked PRs (#20840, #23123) closed unmerged: a repeated abandoned-attempt pattern despite the good-first-issue label." So the history now counts as a scope signal, not a claim signal, and the verdict matches gold.

**Check rationale**

From `tools/issue-select/rubric.md`, check `Newcomer-sized scope`, pass condition as currently written:

> Pass if the issue describes a specific bug, feature, or bounded outcome that a contributor can work toward. Multiple closely related fixes, causes, checklist items, or implementation suggestions may still constitute one bounded contribution. A short or terse issue does not fail merely for lacking detailed reproduction steps. Fail if it is an umbrella/tracking issue spanning independent work, a pure usage/support question, an unresolved design discussion with no maintainer-set direction, or a maintainer explicitly says the work requires core internals or is unsuitable for a newcomer. Also fail when an old issue shows a repeated pattern of abandoned contributor attempts or multiple closed unmerged implementation PRs that provides evidence the task is substantially harder or less settled than its newcomer-facing label suggests.

It reads this way because my first version, "Pass if the issue asks for one bounded contribution and is not an umbrella/tracking issue, pure usage/support question, unresolved design discussion, or work that a maintainer explicitly says requires changes to core internals.", missed in both directions in run 2. It was too strict on two gold accepts: `issue-04` failed because "'Including remove identity, fuse spiders, remove self loops, etc.' is several rules with an open-ended list, not one bounded contribution", and `issue-19` failed because it "lists two root causes plus three suggestions ... not one bounded contribution". And it was too lenient on `issue-15`, as above. So I loosened it (closely related fixes, causes, and suggestions can still be one contribution; a terse issue does not fail for lacking repro steps) and tightened it (design discussion only fails when there is "no maintainer-set direction", plus the abandoned-attempts clause). The `--only` re-run on those three came back 3/3.

**Trade-offs**

The loosening cost me stability on `issue-20` (excalidraw/excalidraw#11811, gold `reject`: "one-line feature wish with no spec and a product decision hiding inside"). With the same final rubric it was accepted in the 23:20 and 23:25 full runs and rejected only in the 23:31 run, where the scope check failed with "Requests a new first-class company-logo element with "Logo asset TBD", an unspecified company and possible app wiring; no maintainer direction". The sentence that rescued `issue-04` and `issue-19`, "A short or terse issue does not fail merely for lacking detailed reproduction steps", can read as permission for a one-line wish, and only the "no maintainer-set direction" clause holds it back. My `--only` run re-graded just the three packages I had missed and did not include a scope canary like `issue-20`, so the side effect only showed up on the next full run. I accept this as one of the arguable scope calls: my committed run matches gold on it, but I know this check can split on terse feature requests.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. **Fit to my interests and time.** I work in both Python and JavaScript/TypeScript, and this bug is a Python detector that fails to recognize JavaScript and TypeScript, so I can judge the test inputs from both sides. It is bounded to one file, `ingestion/parsers/skill_extractor.py`, and five existing `xfail` tests define when it is done, which fits the time I have alongside the course's pace.
2. **What the verdict got right, and what I weighed that it could not.** The skill correctly found that the work is bounded to the three detector functions with five named failing tests, that no one holds it (classmates' claims don't block under the house rule, and there is no open PR), and that the repo has no AI policy against my workflow. It ranked #61 first because it is a one-line fix with a `good first issue` label. I chose #55 anyway: #61 already has 21 comments and many reproductions converging on the same one-line change, so there is little left for me to learn, while #55 matches my JS/TS background and leaves real design questions, like how to widen detection without adding false positives. The rubric cannot weigh my interests or how much I would learn, and its `First-issue signal` check marked #55 lower only because it is labeled `tier-1` instead of `good first issue`.
3. **Anticipated difficulty in claiming it.** Three classmates had already claimed it and posted reproductions, and one had posted a fix plan. Under the house rule that does not block me, but my claim has to add something of my own rather than echo theirs: my own reproduction on macOS, where the existing reports were from Windows and WSL. My claim also has to promise only investigation, not a fix or a date, since someone else may open a PR first.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
