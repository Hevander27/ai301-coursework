# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

Read the whole package once, in this order, before grading any check. The order matters because every check measures the plan against what came before it: the plan is the thing being graded, so it is read last.

1. **Repo facts.** Note the contribution-policy line word for word, especially any AI rule and whether it covers comments, pull requests, or "all AI usage". Note any bug-report or contributing asks.
2. **The issue.** Note the reported symptom in one line, and what the issue says the expected behavior is.
3. **Thread highlights.** For each comment, note its author's association. List every OWNER, MEMBER, or COLLABORATOR comment that gives direction: names a culprit or fix site, proposes or chooses a fix approach, posts a patch or test build, or asks for something to be tested. Note open PRs mentioned. If there is no maintainer direction, write "no maintainer direction".
4. **Repro evidence.** Write down three things: the failing run and its actual result; every control run and its result (what changed, and whether the symptom appeared); and any step that shows where the data or behavior first goes wrong. These notes are the yardstick for the diagnosis and test-plan checks.
5. **Candidate plan.** Note its stated cause, its change list, its in-scope and not-in-scope lines, files or areas named, its test plan, and its risks.
6. **Candidate plan comment.** Note what it commits to, whether it mentions the maintainer direction from step 3, and whether it states AI use.

In live mode, steps 1 to 3 come from the live issue thread and the repo's CONTRIBUTING and AI policy files, step 4 comes from the student's own posted repro comment on the issue, and steps 5 and 6 come from the student's draft files. Only what the drafts contain and quote counts as the candidate side.

## Evidence gathering

Gather one piece of evidence per check, from the place named, and record it as a short quote before grading.

1. **diagnosis-grounded.** Quote the plan's cause sentence. Then, for each control run noted in Read order step 4, write one line: "control: <what changed> → <result>; stated cause predicts <result>". Any mismatch is the deciding evidence.
2. **scope-bounded.** List every item the plan commits to doing (numbered changes, "also" items, "while touching" items). Mark each one as the fix, a direct support of the fix (its test, a same-pattern audit in the same function or file, a reviewer note), or work the issue did not ask for. Record explicit deferrals separately; they are not commitments.
3. **executable.** Quote the files or code area named, and quote the sentence that says what the code will do differently. If either is missing, or the approach contains "or", "whichever", "somewhere", "not sure", or "investigate", quote that phrase.
4. **test-plan-decisive.** Quote the test plan's expected result. Put it next to the repro's actual result from Read order step 4, and record whether a reader could tell the two apart by running the steps.
5. **comment-respects-thread-and-policy.** For each maintainer direction from Read order step 3, quote the plan or comment sentence that follows or addresses it, or record "not addressed". Quote the policy line and classify it: no AI rule; rules without a comment-disclosure duty; or disclosure required for all AI use or for comments. Quote the comment's AI-use sentence, or record "none".
6. **unknowns-stated (preferred).** Quote the plan's risk or unknown sentence, or record "none".

## Check execution

1. Grade the checks in rubric order, one at a time, using only the evidence recorded for that check in Evidence gathering. Re-read the package only if the recorded quote is missing.
2. Apply the check's pass condition literally. When the pass condition names a fail case, look for that case in the recorded evidence first; if it is there, the check fails.
3. If the evidence a check needs is absent from the package (no stated cause, no files, no test plan, no comment), grade it `fail` and write "absent: <what is missing>". Grade `unclear` only when the evidence is present but genuinely supports both readings, and write both readings.
4. Never let one check's result change another's. A wrong diagnosis does not make the scope check fail, and a perfect plan does not make the comment check pass.
5. Do not grade polish. Length, headings, confidence, and formatting are not evidence for any check. A terse plan that meets a pass condition passes it.
6. For each check, write one evidence line: the quote or fact that decided it.

## Verdict assembly

1. Collect the grades of the five required checks.
2. If every required check is `pass`, the verdict is `accept`. If any required check is `fail` or `unclear`, the verdict is `reject`. The preferred check never changes the verdict.
3. In the summary, name the deciding check for a reject (the first failing required check in rubric order) and quote its evidence line. For an accept, name the preferred check's result.
4. In live mode, after the verdict, hold the draft comment against `voice-guide.md` and list any broken rule. This never changes the verdict.
5. Emit the JSON block from SKILL.md last, with one entry per rubric check, including the preferred one.
