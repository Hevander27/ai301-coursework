# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/242

**Branch**

fix/55-skill-extractor-detection-signals

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

In order:

1. Full run: `agreement: 19/20 scored items  (bar: 18/20: PASS)`, `categories: clear-accept 6/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`. The one miss was `pkg-05` (gold accept, graded reject on `repro-before-after`).
2. Partial run after loosening `repro-before-after`: `--only pkg-05,pkg-04,pkg-07,calib-04 --include-calibration`, `agreement: 3/3 scored items` (pkg-05 now accept; canaries pkg-04 and pkg-07 still reject; calib-04 still reject).
3. Full run with `--save-run eval-run.txt`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`, `categories: clear-accept 7/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`. This is the committed `eval-run.txt`.

**Package analysis**

`pkg-05` (nushell, category `clear-accept`). In my final run my rubric decided `accept`; the gold label is `accept` ("repro table before/after shown; template sections filled"). In my first run it decided `reject`, and that miss is why my test-evidence check reads the way it does.

The PR pastes the issue's failing case before and after: before, only the `atuin | none | up` row; after, both `atuin | control | char_r` and `atuin | none | up`. Its plan's test plan also names a second item, "re-run with two same-name same-key bindings, expect one row plus a warning", and the evidence reports that only in words: "Same-key redefine prints the one-time warning." My first rubric required "the after result as pasted output" and that the evidence "covers every failure mode the plan's test plan names", so the grader failed it with "the plan's second failure mode (same-name same-key: one row plus warning) is only asserted ... with no output shown." That was stricter than the gold label: the issue's failing case is decisively shown, and the secondary check is reported, not hidden. After I changed the check to require pasted output for the issue's failing case and a reported result (pasted or stated) for every other test-plan item, the final run passed it with "Issue script shown before (only 'up' row on 0.115.0) and after (control/char_r and none/up rows); same-key warning result stated".

**Check rationale**

From `tools/pr-precheck/rubric.md`, check `repro-before-after`, pass condition as it reads now:

> Pass if the evidence re-runs the plan's repro on the failing path the issue reports (the same command, input, or scenario that broke, not a control or a path the fix never touched) and shows the after result as pasted output or a transcript, with the before result shown or quoted from the repro, AND reports a result for every other item the plan's test plan names (pasted output, or a stated observed result such as "same-key redefine prints the one-time warning"). Fail if the issue's failing case is only claimed, not shown ("tested locally", "works on my machine", "cargo test passes" with no repro output); if the evidence exercises only a control or an unchanged path; or if an item the plan's test plan names is silently missing from the evidence.

It reads this way because my first version ended "AND covers every failure mode the plan's test plan names" and failed evidence that "re-runs only some of the failure modes", which the grader read as "every item needs pasted output". That rejected `pkg-05`, a gold accept that pastes the main repro and states its secondary warning check in words. I did not drop the second half entirely, because `calib-04` (the worksheet borderline) is rejected precisely for being "silent on the --exec abort the plan named". So the line I drew is between stated and silent: the issue's failing case must be shown as output, and every other test-plan item must at least be reported. The "not a control or a path the fix never touched" clause is there for `pkg-07` and `pkg-14`, whose evidence runs a real command on the wrong path.

**Trade-offs**

Loosening the check gives up some rigor: a PR can now pass by stating a secondary result in words ("prints the warning") without pasting it, so a false statement about a secondary check would get through. I accept that, because the main failing case still has to be shown as output.

Because this was a loosening, I re-ran canaries before the confirming full run: `--only pkg-05,pkg-04,pkg-07,calib-04 --include-calibration`. `pkg-04` and `pkg-07` (two already-agreeing `not-tested` rejects) stayed `reject`, and `calib-04`, the borderline the change was most likely to flip, stayed `reject`. The confirming full run then matched gold on all 20, with `not-tested 4/4`: `pkg-10` still failed with "Only 'Verified working on my machine: a full day of WebDAV syncs against Koofr' is given; the plan's 350 ms delayed-connect repro is not re-run", and `pkg-14` with "Evidence runs 'http GET pie.dev/headers X-Custom-Header: value' (a casing check), not the issue's single-header POST JSON case".

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
