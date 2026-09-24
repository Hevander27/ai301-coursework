# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Maintainer activity | Repo facts: last 5 default-branch commits, maintainer first-response sample, and maintainer activity in the issue Comments section | Pass if at least one non-bot default-branch commit occurred within 90 days of the capture date, OR the maintainer first-response sample shows an Owner, Member, or Collaborator responding within 30 days. A bot commit alone does not satisfy this check unless it merged a human PR. | required |
| Repository in use | Repo facts: archived flag, latest release, last push to any branch, and stars | Pass if the repository is not archived AND either its latest release or last push to any branch occurred within 180 days of the capture date. | required |
| Newcomer-sized scope | Issue body and Comments section, including maintainer guidance about implementation difficulty and evidence of prior implementation attempts | Pass if the issue describes a specific bug, feature, or bounded outcome that a contributor can work toward. Multiple closely related fixes, causes, checklist items, or implementation suggestions may still constitute one bounded contribution. A short or terse issue does not fail merely for lacking detailed reproduction steps. Fail if it is an umbrella/tracking issue spanning independent work, a pure usage/support question, an unresolved design discussion with no maintainer-set direction, or a maintainer explicitly says the work requires core internals or is unsuitable for a newcomer. Also fail when an old issue shows a repeated pattern of abandoned contributor attempts or multiple closed unmerged implementation PRs that provides evidence the task is substantially harder or less settled than its newcomer-facing label suggests. | required |
| Issue unclaimed | Repo facts: this issue's assignees and linked PRs; issue Comments section for claim comments and PR mentions | Pass if there is no current assignee, no open linked or comment-mentioned PR implementing the issue, and no unresolved active claim to work on it. Closed unmerged PRs are abandoned attempts rather than active claims. | required |
| AI contribution policy | Repo facts: contribution policy, including CONTRIBUTING.md, dedicated AI policy files, linked contributor documentation, and PR/issue template requirements | Pass if the contribution policy is silent about AI use or permits AI-assisted contributions outright or with conditions. Fail only if the policy explicitly prohibits AI-generated or AI-assisted contributions. | required |
| First-issue signal | Issue body and labels represented in the snapshot bundle | Pass if the issue has an explicit good-first-issue or equivalent newcomer-friendly label or a maintainer explicitly describes it as appropriate for a new contributor. | preferred |

## Verdict rule

Accept if every required check passes. Reject if any required check fails. An unclear required check counts as a fail. Preferred checks never change the accept/reject verdict; they are used only to rank issues that pass all required checks.

