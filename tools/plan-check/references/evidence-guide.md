# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

## Diagnosis and grounding

**Where it lives.** In an eval bundle: the candidate plan's "Diagnosis" or "Cause" line (or the first sentence that says why the bug happens), checked against the repro-evidence block's steps, control runs, and "Actual" line, and against maintainer comments in the thread highlights. In live mode: the draft plan's diagnosis, checked against the student's posted repro comment on the issue and the live thread.

**What good looks like.** The stated cause explains the failing run and predicts every control's result. If a control changed one thing and the symptom vanished, the cause involves that thing. If a control kept the blamed component and the symptom still appeared (or the blamed component worked in the same build), the cause is ruled out.

**Traps.** A confident, detailed diagnosis copied from the thread or from intuition, when the package's own controls or intermediate steps contradict it. Watch for evidence that the data is already wrong before the step the plan blames.

## Scope

**Where it lives.** The plan's numbered change list, its "In scope" and "Not in scope" lines, any "while touching" or "also" items, and the files or areas list. Live: the same parts of the draft plan.md.

**What good looks like.** One change that resolves the issue as reported, its regression test, and at most an audit of the same pattern nearby. Deferrals with reasons ("leaving the Windows variant out, I cannot test it") are a sign of good scoping, not a gap.

**Traps.** A correct core fix bundled with a migration, a module rewrite, a new option or settings panel, a redesign, or other behavior fixed "while in the area". The bundle fails even when its first item is right.

## Executability

**Where it lives.** The plan's "Files" line, the code areas named in the change description, and the sentence that says what the code will do differently. Live: the draft plan's files-to-touch and approach sections; in Path Review these are paths like `ingestion/parsers/skill_extractor.py` and test files under `tests/unit/`.

**What good looks like.** A named file or specific code area, and one chosen approach. A plan may leave exact function names to the build if the layer and the approach are already chosen.

**Traps.** "Somewhere", "X or Y, whichever is easier", "not sure which layer", "profile and optimize", or a plan that is really an investigation with no decided change.

## Test plan

**Where it lives.** The plan's "Test plan" or "Test" section, read against the repro evidence's steps and its "Actual" result. Live: the draft plan's test plan against the student's posted repro steps and output; in Path Review, usually the issue's named `pytest` tests run with and without `--runxfail`.

**What good looks like.** The repro steps (or a named regression test made from them) re-run with an expected result a reader can check and that differs from the bug's actual result: exit 0 instead of a panic, the pushed color at step 3, the named tests passing.

**Traps.** "Run the full test suite", "CI green", "should feel fast", "nothing else breaks". None of these tells a reader whether this bug is fixed.

## Honesty

**Where it lives.** The plan's "Risk", "Unknowns", or "Open question" lines, its confidence words ("confirmed", "certainly"), and, after the build, its "Deviations" section. Live: the same parts of the draft plan.md.

**What good looks like.** At least one real risk or unknown named with how it will be handled (gated, flagged for review, tested). After a build, a Deviations section that says what changed from the plan and why, or says plainly that nothing changed.

**Traps.** Untested guesses written as certainties, and a deviation that exists only in the diff.

## Comms

**Where it lives.** The candidate plan comment, read against the thread highlights' OWNER, MEMBER, and COLLABORATOR comments, and against the repo-facts contribution-policy line. Live: the draft comment.md against the live thread and the repo's CONTRIBUTING.md and any AI policy file (Path Review's is `docs/CONTRIBUTING.md`, which states no AI rule).

**What good looks like.** Where a maintainer has given direction (named the culprit or fix site, proposed an approach, posted a test build and asked for testing), the comment follows it or says why it departs. It may mention open PRs and how it relates to them. Timing words like "PR shortly" are fine.

**Reading an AI policy.** Treat every eval package as AI-assisted work. No AI rule: pass. Rules to understand, review, or own the work, or to write comments in your own words: no disclosure is owed, pass. Disclosure scoped to pull requests: pass. Disclosure required for all AI usage or for comments: pass only if the comment states the AI use in words.

**Traps.** A comment that politely ignores a maintainer's posted patch and goes a different way, and an excellent plan whose comment omits a disclosure the policy requires.
