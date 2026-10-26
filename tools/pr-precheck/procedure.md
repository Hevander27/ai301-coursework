# Procedure: how this tool grades a PR package

<!--
THIS IS THE PART YOU WRITE (second week running for the procedure).
Week 3 you wrote these steps for a plan package; this week the graded
object is a PR package, and the read that matters most is a
side-by-side: the diff against the plan, the evidence against the test
plan, the description against both. Your week-3 procedure is the
pattern; do not paste it unchanged, because its read order was built
for a different object.

Your rotation is the design brief again, and this week friction routes
three ways: a stall on WHAT to decide is a rubric gap, a stall on
WHERE to look is a procedure gap (this file), and a stall on what the
tool even reads or outputs is a frame gap (your SKILL.md). A complete
procedure lets someone who has never seen a PR package before grade
one exactly the way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
plan's scope pair before opening the diff, and list the files the plan
names" is a step; "understand the change" is a wish.
-->

## Read order

Read the whole package once, in this order, before grading any check. The plan comes before the PR because every check measures the PR against something the plan or the repo already fixed.

1. **Repo facts.** Write down the pull-request line word for word: every template section, checklist item, required artifact (changelog, whatsnew), and issue-reference ask. Write down the contribution-policy line's AI rule, and whether it requires disclosure.
2. **The issue and thread.** Note the reported failing case in one line, and any maintainer direction.
3. **Plan context.** Note the repro evidence's failing case and its before result; the plan's list of changes; its in-scope and not-in-scope lines; its files; its test plan, listing every failure mode it names; and any deviation notes.
4. **Title and description.** Note every claim about what the PR does, every section heading present, the issue reference, and any AI-use statement.
5. **Commits.** Note each message.
6. **Diff.** For each file and hunk, write one line: what it changes.
7. **Test evidence.** Note each command run, which path it exercises, and its before and after output.

In live mode, steps 1 and 2 come from the live repo (`.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, the issue thread); step 3 from the student's `plan.md`; step 4 from `pr_draft.md` (its first line is the title); steps 5 and 6 from `git log main..HEAD` and `git diff main...HEAD`; step 7 from `test_evidence.md`.

## Evidence gathering

Gather one piece of evidence per check, side by side, before grading.

1. **diff-matches-plan.** Make a two-column list: each hunk from Read order step 6 next to the plan item or deviation note it implements, or "none". Then list each plan change next to the hunk that delivers it, or "missing", and say whether the deviation notes and the description both disclose it. Then list each description claim about the change next to the hunk that makes it true, or "contradicted".
2. **repro-before-after.** Put the issue's failing case next to each test-evidence command and mark whether it runs the failing path, a control, or an unchanged path. List each item the plan's test plan names next to the evidence that reports it (pasted output, or a stated observed result), or "not run" if the evidence is silent on it. Quote the before and after output.
3. **diff-is-clean.** Scan every hunk for these patterns and quote each hit: print or log calls with "debug" or temporary text; lines starting with a comment marker that contain code; functions or variables nothing calls; new TODO or FIXME; whitespace-only, re-indent, or import-order changes on lines the fix does not otherwise touch.
4. **meets-repo-standards.** Put each ask from Read order step 1 next to where it is met (description section, diff file, issue reference line), or "missing". Quote the description's AI-use sentence, or record "none", and record whether the policy requires one.
5. **commits-describe-change (preferred).** Quote the commit messages.

## Check execution

1. Grade the checks in rubric order, one at a time, using only the evidence recorded for that check. Re-read the package only if a recorded item is missing.
2. Apply the pass condition literally. Look first for the fail cases it names in the recorded evidence; if one is present, the check fails.
3. If evidence a check needs is absent from the package (no test output, no diff, no description), grade `fail` and write "absent: <what is missing>". Grade `unclear` only when the evidence is present and genuinely supports both readings, and write both readings.
4. Keep checks independent. A correct, well-tested fix does not make the cleanliness or standards check pass, and a description problem does not make the test-evidence check fail.
5. Do not grade polish. Length, a beautiful checklist, or confident wording are not evidence. Compare the description to the diff, never the description to itself.
6. For each check, write one evidence line: the quote or fact that decided it.

## Verdict assembly

1. Collect the grades of the four required checks.
2. If every required check is `pass`, the verdict is `accept`. If any required check is `fail` or `unclear`, the verdict is `reject`. The preferred check never changes the verdict.
3. In the summary, name the deciding check for a reject (the first failing required check in rubric order) and quote its evidence line.
4. In live mode, after the verdict, hold the PR title and description against `voice-guide.md` and list any broken rule. This never changes the verdict.
5. End with the JSON block from SKILL.md, one entry per rubric check including the preferred one, and nothing after it.
