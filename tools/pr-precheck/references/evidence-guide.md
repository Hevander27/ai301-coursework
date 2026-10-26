# Evidence guide: where evidence lives in a PR package

<!--
THIS IS THE PART YOU WRITE (third week running: the map stays in your
hands). Your tool uses this guide as its map: for every kind of
evidence a rubric check names, this file says WHERE to find it in a PR
package and WHAT GOOD LOOKS LIKE when you do.

The four families below are the harness's failure categories under
the names the eval README uses: plan fidelity = silent-drift, test
evidence = not-tested, diff quality = unreviewable, standards and
comms = standards-wall. A package that fails none of them is a
clear-accept. Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the plan-context block's scope pair and test
  plan, the candidate PR's diff, commits, description, or
  test-evidence section, the repo-facts block's template asks and
  stated policy). In live mode (where in your working copy and on
  GitHub: your plan.md and its deviation notes, your branch's diff,
  your draft title and description, your captured test output, the
  repo's PR template and CONTRIBUTING.md).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("every changed file falls inside the
  plan's stated boundary or a deviation note") over adjectives ("the
  diff is clean").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts three ways: your
procedure says WHEN to gather each family, this guide says WHERE, and
your SKILL.md says the tool reads both. Write the map you wish your
executor had.
-->

## Plan fidelity (harness category: silent-drift)

**Where it lives.** In an eval bundle: the "Plan context" section (plan changes, in-scope and not-in-scope lines, deviation notes) next to the "Diff" and the "Description". In my own PR: `plan.md` (including `## Deviations`) next to `git diff main...HEAD` and `pr_draft.md`.

**What good looks like.** Every hunk maps to a plan item or a deviation note; every plan item is in the diff or disclosed as left out in both the plan and the description; every description claim is true of the diff.

**Traps.** A beautiful description saying "implements the plan exactly" over a diff that also adds a flag, option, rename pass, or rewrite of a file the plan never names. And the less-than direction: a description claiming docs or tests the diff doesn't contain. An honestly disclosed shortfall is fine.

## Test evidence (harness category: not-tested)

**Where it lives.** In an eval bundle: the "Test evidence" section, read against the plan's test plan and the issue's failing case. In my own PR: `test_evidence.md` and the Testing section of `pr_draft.md`, read against `plan.md`'s test plan and my Unit 2 repro.

**What good looks like.** The failing case from the issue re-run on the branch, with pasted output for the after and the before shown or quoted from the repro, with a result reported for every other item the plan's test plan names (pasted output, or the observed result in words). The repo's own checks with their output are a bonus.

**Traps.** "Tested locally", "works on my machine", "tests pass" with nothing shown; evidence that runs only the control or a path the fix never touched; a test-plan item that is silently missing from the evidence.

## Diff quality (harness category: unreviewable)

**Where it lives.** In an eval bundle: the "Diff" and "Commits" sections. In my own PR: `git diff main...HEAD` and `git log main..HEAD`.

**What good looks like.** Every line in the diff is part of the fix or its test. Commits say what they change.

**Traps.** Debug prints, commented-out first attempts or debug lines, dead helper functions, new TODOs, and re-indent or import churn on untouched lines. These fail the PR even when the fix itself is right.

## Standards and comms (harness category: standards-wall)

**Where it lives.** In an eval bundle: the repo-facts "pull requests" line (template sections, required checklists, required changelog or whatsnew entries, issue-reference asks) and the "contribution policy" line, read against the title, description, and diff. In my own PR: Path Review's `.github/PULL_REQUEST_TEMPLATE.md` (Summary, Issue with `Closes #`, Changes, Testing checklist, Screenshots / Demo, Notes for Reviewers) and `docs/CONTRIBUTING.md`, read against `pr_draft.md`. The course always requires an AI-use disclosure, which goes in Notes for Reviewers.

**What good looks like.** Every required section present and filled, required artifacts in the diff, the issue referenced, and the AI use stated when the policy requires it. A repo with no template and no AI rule only needs the issue reference.

**Reading an AI policy.** Treat every eval package as AI-assisted work. No AI rule, or rules to understand, review, or write comments in your own words: no disclosure is owed. A policy requiring disclosure of all AI use: the description must state it.

**Traps.** An excellent PR whose description skips the required checklist or the AI disclosure, or a bug fix missing the changelog or whatsnew entry the template requires.
