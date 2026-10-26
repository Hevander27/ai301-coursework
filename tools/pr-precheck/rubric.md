# Rubric: is this pull request ready to submit?

<!--
THIS IS THE PART YOU WRITE (fourth week running; this is the rubric's
final form in the sandbox). Your frame in SKILL.md executes whatever
checks you define here, via your procedure.md. It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the diff read against the plan's scope, the test
     evidence read against the plan's test plan, the description read
     against the diff, the repo-facts block's template asks) or a
     location from your references/evidence-guide.md. "The PR" is not
     a source; "the diff's changed files read against the plan's
     stated boundary" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself
     (does the diff fall inside the plan plus its deviation notes? is
     the claimed evidence observable?), never the write-up's shape
     (how long the description is, how many commits there are).
     Structure-shaped checks are what make graders disagree with
     themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (submit) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. State the `unclear`
   treatment explicitly: the frame here is YOUR SKILL.md, so a rubric
   that stays silent is only covered if your frame's grading
   discipline says what happens (the contract's own default is that
   an unverifiable claim fails).

Cover what actually gets bad PRs submitted. The failure families the
lecture named ARE the harness's scoring categories, same names as the
eval README: silent drift (the diff silently does more or less than
the posted plan, or the description claims fidelity the diff
contradicts), not tested (the evidence proves nothing observable, or
the repo's own checks were never run), unreviewable (debris or
unrelated hunks bury the change), and standards wall (the repo's
stated template and disclosure asks are ignored). Your evidence
guide's four headings map onto these one to one (plan fidelity =
silent drift, test evidence = not tested, diff quality =
unreviewable, standards and comms = standards wall), and the category
floor is scored on exactly these names plus clear accept. A rubric
that ignores a category will fail the eval packages built around
that category. And remember the honest-outcome
rule, fourth week running: a PR that honestly discloses a shortfall
can be ready; a rubric that equates "less than everything" with
"hold" fails the set.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diff-matches-plan | Every changed file and hunk in the diff, read against the plan's stated changes, its in-scope and not-in-scope lines, and its deviation notes; then the description's claims about what the PR does, read against the same diff (Plan fidelity in the evidence guide) | Pass if every hunk in the diff implements something the plan or its deviation notes name, every change the plan promises is either in the diff or disclosed as left out (in the plan's deviation notes and restated in the description), and every claim the description makes about the change ("implements the plan exactly", "docs now document X", "no other changes") is true of the diff. A shortfall that is disclosed in both places passes. Fail if the diff does more than the plan (a new option, flag, setting, rename pass, or rewrite the plan does not name, or one its not-in-scope line excludes) without a deviation note; if the diff does less than the plan without disclosure; or if the description claims something the diff contradicts. | required |
| repro-before-after | The test evidence section, read against the plan's test plan and the issue's failing case (Test evidence in the evidence guide) | Pass if the evidence re-runs the plan's repro on the failing path the issue reports (the same command, input, or scenario that broke, not a control or a path the fix never touched) and shows the after result as pasted output or a transcript, with the before result shown or quoted from the repro, AND reports a result for every other item the plan's test plan names (pasted output, or a stated observed result such as "same-key redefine prints the one-time warning"). Fail if the issue's failing case is only claimed, not shown ("tested locally", "works on my machine", "cargo test passes" with no repro output); if the evidence exercises only a control or an unchanged path; or if an item the plan's test plan names is silently missing from the evidence. | required |
| diff-is-clean | The diff's hunks and the commit list (Diff quality in the evidence guide) | Pass if every hunk is part of the change: no debug prints or logging left in, no commented-out code (old attempts or debug lines), no dead functions or probes, no new TODO or FIXME, and no formatting, re-indent, or import-reorder churn on lines the change does not otherwise touch. Fail if any of those appear, even when the real fix in the same diff is correct. | required |
| meets-repo-standards | The repo-facts block's pull-request line (template sections, required checklist items, required artifacts such as a changelog or whatsnew entry, issue-reference asks) and contribution-policy line, read against the title, description, and diff. Treat every eval package as AI-assisted work (Standards and comms in the evidence guide) | Pass if every section or checklist the repo's PR template requires is present in the description, every artifact the template requires for this kind of change (for example a changelog or whatsnew entry for a bug fix) is in the diff, the issue is referenced where the repo asks for it, AND, when the contribution policy requires disclosing AI use, the description states the AI use. A repo with no PR template and no AI rule passes on the issue reference alone; "own words" and "understand your work" rules impose no disclosure. Fail if a required section or checklist is visibly missing, a required artifact is absent from the diff, the required issue reference is missing, or a required AI disclosure is absent. | required |
| commits-describe-change | The commit list (Diff quality in the evidence guide) | Pass if commit messages say what the change does (not "wip", "fix", "misc cleanups"). | preferred |

## Verdict rule

Accept if every required check passes. Reject if any required check fails or is unclear. The preferred check never changes the verdict. Evidence a check needs that the package does not contain (no test output, no description section the template requires) is a fail, not unclear; reserve unclear for evidence that is present but genuinely supports both readings. Grade each required check on its own evidence: a correct, well-evidenced fix does not rescue debris in its diff, a missing required disclosure, or a description that claims more than the diff does. A shortfall the plan's deviation notes and the description both disclose is not drift.
