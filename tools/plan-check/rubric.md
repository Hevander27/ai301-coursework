# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-grounded | The plan's stated cause (its diagnosis line or the first paragraph that names why the bug happens), read against every step, control run, and observation in the repro-evidence block, and against maintainer statements in the thread highlights (Diagnosis and grounding in the evidence guide) | Pass if the plan names a cause, and that cause is consistent with every observation in the repro evidence: it explains the failing run AND every control run (each control's result is what the stated cause predicts). Fail if any control or step in the repro evidence rules the stated cause out (the symptom appears with the blamed component absent or unchanged, the blamed component is shown working in the same build, or the data is already wrong before the blamed step runs); if the plan adopts a diagnosis from the thread or its own intuition that the package's own evidence contradicts; or if no cause is stated at all. A cause that is stated as a hypothesis but fits all the evidence passes. | required |
| scope-bounded | The plan's change list, in-scope and not-in-scope statements, and files or areas named, read against what the issue asks for (Scope in the evidence guide) | Pass if the plan commits to one bounded change that resolves the issue as reported, plus only what that change needs (its regression test, an audit of the same pattern at sibling sites in the same function or file, a note to reviewers). Explicit deferrals with reasons (leaving a variant or a larger rework for later) are fine and do not fail. Fail if the plan also commits to work the issue did not ask for: a dependency or framework migration, a rewrite or restructure of a module, a new user-facing option or settings UI, a redesign, a test-harness migration, or "while I'm in the area" fixes to other behavior, even when the core fix inside it is correct. | required |
| executable | The plan's files or code areas, its chosen approach, and its order of work (Executability in the evidence guide) | Pass if a stranger could start the work without asking the author anything: the plan names the files or the specific code area where the change goes, AND commits to one approach (what the code will do differently). Pinning exact function names during the build is fine once the area and the approach are chosen. Fail if no files or code area are named; if the approach is left open ("somewhere", "X or Y, whichever is easier", "not sure which layer"); or if the plan is an investigation with no chosen change ("profile and optimize", "poke around", "investigate the input stack"). | required |
| test-plan-decisive | The plan's test plan, read against the repro evidence's steps and the actual result they produced (Test plan in the evidence guide) | Pass if the test plan names at least one observable outcome specific to this fix: re-running the repro steps (or a named regression test built from them) with a stated expected result that differs from the bug's actual result (an exit code, an output, a color, a count, a passing named test). Fail if the only test is generic ("run the full test suite", "CI passes"), subjective ("should feel fast", "nothing else should feel broken"), or names no outcome a reader could check against the repro. | required |
| comment-respects-thread-and-policy | The candidate plan comment and the plan, read against explicit direction from maintainers (OWNER, MEMBER, or COLLABORATOR comments) in the thread highlights, and the repo-facts contribution-policy line. Treat every eval package as AI-assisted work (Comms in the evidence guide) | Pass if BOTH hold. (1) Thread: where a maintainer in the thread has given explicit direction (named the culprit or fix site, chose or proposed a fix approach, posted a patch or test build and asked for testing), the plan and comment either follow that direction or say why they depart from it; a plan that silently goes another way fails. Threads with no maintainer direction pass this half. (2) Policy: pass if the policy states no AI rule, or its AI rules put no disclosure duty on comments (understand or review the work, comments in your own words, disclosure in pull requests only), or the comment explicitly states AI use. Fail this half only when the policy requires disclosing all AI usage or AI use in comments, and the comment does not state it. Timing words in the comment ("PR shortly", "this week") do not fail this check. | required |
| unknowns-stated | The plan's risks, unknowns, and confidence language (Honesty in the evidence guide) | Pass if the plan names at least one real risk or unknown of its change (a case it might break, a variant it cannot test, a decision left for review) or the change is small enough that none applies, and does not present an untested guess as certain. | preferred |

## Verdict rule

Accept if every required check passes. Reject if any required check fails or is unclear. Preferred checks never change the verdict. Evidence a check needs that the package does not contain (no stated cause, no files, no test plan) is a fail, not unclear; reserve unclear for evidence that is genuinely ambiguous. Grade each required check on its own evidence: an excellent plan does not rescue a comment that ignores the maintainer's direction or a missing required disclosure, and a correct core fix does not rescue a plan that bundles unrequested work.
