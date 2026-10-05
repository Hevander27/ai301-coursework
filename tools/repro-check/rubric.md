# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment-recorded | The repro report's environment record (versions, OS, install method, relevant settings), read against the issue's stated environment, against any statement in the issue body or thread that a factor changes whether or how the bug appears, and against the repo-facts bug-report template's version asks (Environment in the evidence guide) | Pass if the report names the version of the software under test, the operating system, and every other factor the issue or a maintainer says changes the behavior (for example the driver, shell, browser language, or build profile), AND the tested version is the issue's version, a newer one, or the current release or main, OR the report itself states how its version or platform differs from the issue's. Fail if there is no environment record, if a factor the issue says matters is missing, or if the report tests a version older than the one the issue targets without saying so. A different OS or a newer version that the report calls out does not fail. | required |
| steps-rerunnable | The repro report's steps and command transcripts (commands shown inline in output blocks count as steps), read against the issue's own reproduction steps and the trigger elements the issue and thread name (Steps in the evidence guide) | Pass if a stranger holding only the report, the issue, and publicly available software could repeat the attempt from a stated starting state to the trigger. Each step gives the exact command, input, file content, or setting; or points to an input the issue itself supplies (for example "the input file exactly as given in the issue"); or describes an input by every element the issue says matters. Fail if there are no steps; if any step depends on private code, private configuration, or anything else a stranger cannot obtain; if a step only gestures at work ("set up the project") with no command or input; or if the steps leave out an element the issue names as part of the trigger (a required flag, driver, or setting). | required |
| behavior-matches-issue | The repro report's artifacts (pasted output, log excerpts, exit codes, rendered results), read against the exact symptom the issue describes (its error message or error class, exit code, wrong value, or missing output) and against the trigger the issue and any maintainer comment say is required (Behavior shown in the evidence guide) | Pass if at least one artifact came from running the issue's trigger (the same command, syntax, operator, expression, input, and settings, or a minimal equivalent that keeps every element the issue or a maintainer names as required), AND either that artifact shows the issue's specific symptom, or the report says the bug did not reproduce and the artifact shows the faithful attempt's actual result. Fail if there is no artifact; if the artifacts show only setup, a version banner, or that the software runs; if the input was changed in a way that removes the trigger (a different syntax, a different operator, a different expression or variable); or if the artifact shows a different symptom from the issue's (a graceful validation error or compile error where the issue reports a panic or a runtime error; garbled output with the program still running where the issue reports a crash). Read what the output shows, never what the narration says it shows. | required |
| claims-match-evidence | Every sentence in the claim comment and the repro report that asserts reproduction, confirmation, a root cause, or existing understanding of the code, each read against the artifact in the report that would back it (Honesty in the evidence guide) | Pass if every such assertion is backed by an artifact shown in the report, or is worded as a hypothesis ("seems", "looks plausible", "my guess"). An honest cannot-reproduce that says it did not reproduce, shows its attempt, and names what differed passes. Statements about how the author worked (ran every step myself, used an AI assistant) are not assertions this check grades. Fail if the words claim more than the artifacts show: "reproduced" or "confirmed" over an artifact that shows a different behavior or no behavior; a root cause stated as verified with no transcript, log, or measurement; a claim to already understand the code path, internals, or fix with nothing in the report showing it; repetition or certainty language ("100%", "guaranteed", "every single time", "on several machines") offered as proof when the artifact itself does not show the issue's behavior; or a claim that the bug exists on a build or platform the report did not show it on. | required |
| claim-comment-specific | The candidate claim comment, read against the issue's title, body, and thread highlights (Comms in the evidence guide) | Pass if the claim comment refers to something particular to this issue (its symptom, trigger, component, version, or a pointer from the thread), so it could not be pasted unchanged onto a different issue, AND names a concrete next action the author will take on this issue (what they will investigate, trace, test, read, or set up next). Wanting the issue, claiming it, or a general offer to help is not by itself a next action. Fail if it names no next action; if it is a +1, a me-too, or a request for updates; if it is interchangeable boilerplate (praise for the project, "please assign this to me", "reserve it for me") that would fit any issue; if it promises an outcome or a deadline ("a fix guaranteed by Friday"); or if it self-assigns. | required |
| ai-disclosure | The repo-facts contribution-policy line (CONTRIBUTING.md, AI_POLICY.md, or similar as quoted there), read against the full text of the claim comment and the repro report. Treat every eval package as AI-assisted work, as course packages are (Comms in the evidence guide) | Pass if the policy states no AI rule; or its AI rules put no disclosure duty on issue comments (rules to understand, test, review, or take responsibility for AI output; rules that comments be in the contributor's own words; and disclosure rules scoped only to pull requests all count as no comment-disclosure duty); or either comment contains an explicit statement of AI use. A rule that comments be in the contributor's own words is met unless the claim comment fails claim-comment-specific as boilerplate; being AI-assisted work does not break it on its own. Fail only when the policy requires disclosure that covers issue comments or all AI usage in any form, and neither comment states AI use. | required |
| control-run | The repro report's artifacts (Behavior shown in the evidence guide) | Pass if the report includes a contrast run that changes only the suspected trigger (for example the same command with and without the suspected flag, input, or setting) and shows the behavior appearing in one run and not the other. | preferred |

## Verdict rule

Accept if every required check passes. Reject if any required check fails or is unclear. Preferred checks never change the verdict; they only describe how strong an accepted package is. Evidence a check needs that the package simply does not contain (no environment record, no steps, no artifact) is a fail, not unclear; reserve unclear for evidence that is genuinely ambiguous. Grade every required check on its own evidence: a strong repro report does not rescue a failing claim comment or a missing disclosure, and an honest cannot-reproduce is graded on the same checks as a reproduction, never failed for its outcome alone.
