---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

<!--
THIS IS THE PART YOU WRITE, and it is the last one: the frame itself.
Weeks 1 through 3 handed you a working SKILL.md and you filled the
files behind it; this week the frame ships as headings, and you write
what it says. The frontmatter above and the section headings below are
fixed (CONTRACT.md's layout rule); the instructions under each heading
are yours. Write instructions to the tool, in the imperative, the way
weeks 1-3's frames spoke to you: what to read, in what order, what to
refuse, what to emit. Your executor in the rotation is the test: a
frame gap they hit (cannot tell what the tool reads, or how a verdict
gets assembled) is a missing sentence here.

One section is not yours: the JSON schema in "Verdict and output" is
reproduced from CONTRACT.md verbatim and may not be altered. Your
words decide everything around it.
-->

## The question

Answer one question about one PR package: **is this pull request ready to submit?** A PR package is a candidate pull request (its title, description, commits, diff, and test evidence), read against the plan it claims to implement (including the plan's deviation notes), the issue that plan belongs to, and the repo's stated standards (its PR template and contribution policy). Grade exactly one package per run. Never grade the code's general quality, the plan's merits, or anything outside this question.

## Inputs and modes

Run in exactly one of two modes.

**Live mode** grades the student's own PR before it is opened. Read these inputs, from these sources:

- **Plan:** `plan.md` in the working copy, including its `## Deviations` section.
- **Diff:** everything the branch changes relative to `main`, from `git diff main...HEAD` (three dots) run in the working copy. Read the commits from `git log main..HEAD`. Only committed changes count; ignore uncommitted files.
- **Title and description:** `pr_draft.md`. Its first line is the title; the rest is the description.
- **Test evidence:** `test_evidence.md`.
- **Issue side:** the issue thread at the URL the student gives, the repo's `.github/PULL_REQUEST_TEMPLATE.md`, and `docs/CONTRIBUTING.md`, read from the live repo or the working copy.

A house-chain student has no plan or repro of their own: read the house plan and the house repro pack wherever these steps say plan or repro, and grade them with the same checks. If a named input file is missing, grade the checks that need it as `fail` with "absent: <file>"; do not search the directory for substitutes.

**Eval mode** grades a package bundle. The bundle is the whole world: take every fact from the bundle text, fetch nothing, run no commands, and read no other file. Always grade every check with the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` from this skill's own directory (`~/.claude/skills/pr-precheck/scope.md`, next to this file), not from the student's working copy, before anything else. Apply its house rules when reading evidence. Refuse to grade a PR whose issue or base repository is outside the repo `scope.md` names, and say why. If the `Repo:` line still holds a bracketed placeholder, stop without grading and tell the student to fill the `Repo:` line in `scope.md` with their section's Path Review repo. Never guess a scope. In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` from this skill's own directory (next to this file) and hold the outgoing PR title and description against its rules after the verdict is decided. In the summary, list each rule the draft breaks, quoting the rule and the offending line. The voice guide never changes the verdict, because it is the student's personal standard and the rubric holds the universal ones. In eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read every component from this skill's own directory, next to this file. Read `rubric.md` for the checks and the verdict rule, and `references/evidence-guide.md` for where each evidence family lives and what good looks like. Then execute `procedure.md` as written, stage by stage: Read order, Evidence gathering, Check execution, Verdict assembly. Where the procedure is silent on a step you need, report the gap in the summary and grade only what the written steps support; never improvise a step around the gap. If `rubric.md` has no checks or `procedure.md` has no steps, refuse to grade and say which file is empty.

## Verdict and output

The verdict is binary: `accept` means ready to submit, `reject` means hold. There is no third verdict; put reservations in check evidence lines. Before the block you may print a short summary, one line per check, plus voice-guide notes in live mode. End the reply with this fenced JSON block, valid, with one entry per rubric check, and nothing after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- **Evidence first.** Never grade a check without naming the fact or quote that decided it. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** Read the diff against the plan and the description against the diff. A terse, complete PR can be ready, and a confident, beautiful one can hide drift.
- **The rubric decides.** If a check passes by its stated condition but feels wrong, it passes; note the tension in the summary.
- **The procedure decides how.** Follow `procedure.md` as written and report its gaps.
- **Unclear defaults to fail.** Treat `unclear` as the rubric's verdict rule says; where it is silent, an unverifiable claim fails.
