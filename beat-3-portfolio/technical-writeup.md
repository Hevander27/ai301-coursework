# Building Issue Scout: Learning to Evaluate an AI Tool Against Real Open-Source Projects

## Overview

Finding an open-source issue that matches a developer's skills is not always straightforward. GitHub contains thousands of open issues, but many are already claimed, poorly scoped, inactive, or considerably more complicated than their labels suggest. I built **Issue Scout**, a Claude Code skill designed to identify and evaluate suitable first-contribution opportunities. I developed its search strategy, evaluation rubric, and contributor fit profile, using GitHub search results and repository activity to distinguish promising issues from unsuitable ones. My first real scan exposed weaknesses that were not apparent from the tool's initial configuration. A filtering rule incorrectly excluded 19 potentially relevant issues, while other rules overlooked active pull requests and misjudged repository participation. After revising and replaying the search, I reduced the incorrect exclusions from 19 to zero in the saved results. I subsequently used Issue Scout to evaluate ChurchCRM documentation issues, leading to [pull request #252](https://github.com/ChurchCRM/docs.churchcrm.io/pull/252), which remains open and awaiting maintainer review. The tool's latest evaluation passed with an agreement score of 18 out of 20.

## What happened

### Building the initial search strategy

I initially designed Issue Scout around a practical problem: identifying an issue that would allow me to make a meaningful contribution without first learning an enormous codebase.

The search strategy contained four GitHub queries and eight ranking rules. The evaluation rubric included five required checks and one preferred check. Together, these were meant to consider whether an issue was available, adequately scoped, technically relevant, and supported by an active project.

On November 11, 2026, I conducted my first manual searches. Instead of consistently finding manageable contributions, the queries returned cryptocurrency bounty promotions, heavily commented issues, and large projects such as Kibana and OpenHands.

I responded by introducing more restrictive search filters:

```bash
-label:bounty
--comments "<=8"
--updated ">2026-09-11"
--no-assignee
```

These filters helped narrow the search toward recent, unassigned issues with relatively little discussion.

The first full scan returned 380 unique issues. Applying ranking filters reduced the set to 356, and retaining one candidate per repository left 164. Of the five highest-ranked issues evaluated in detail, only two were accepted.

The result suggested that retrieving relevant issues was only one part of the problem. The tool also needed to interpret the available information correctly.

### Discovering the filtering mistake

The most revealing problem involved ranking rule 5, which was intended to exclude visual-design and frontend-only work.

My original rule looked for certain character sequences inside issue labels. In particular, it treated the letters `ui` as evidence that an issue was related to user-interface work.

However, those letters also occur inside unrelated words.

As a result, the tool excluded labels containing expressions such as `quick fix`, `low-hanging fruit`, and `trade-suite`, even though they did not indicate frontend work.

My scan notes revealed that the rule had excluded 21 issues, but only two had genuinely relevant frontend labels. **Nineteen potentially suitable issues had been removed incorrectly.**

This was not a failure of GitHub search itself. It was a failure of the interpretation logic I had written around the search results.

### Revising the rules

I changed rule 5 to match complete label words rather than arbitrary substrings.

The revised rule lowercases each label, separates its words using non-alphanumeric characters, and checks whether any complete word matches the excluded categories.

**Evidence — Rule 5 before and after**

```text
Before:
Drop a result if a label contains "ui" as a substring.

After:
Lowercase the label and split it into words.
Exclude only labels containing complete words such as:
ui, frontend, design, css, visual.
```

From `search-strategy.md`, rule 5 as it stands now:

> Drop any result with a label that contains one of these whole words: "ui", "frontend", "design", "css", "visual". (Profile: avoid "UI/visual-only work".) To find a label's words, lowercase it and split it on any character that is not a letter or digit: "area: frontend" becomes [area, frontend] and is dropped; "UI/UX" becomes [ui, ux] and is dropped. Never match inside a word: "quick fix", "low-hanging fruit", "trade-suite", "build" and "guide" contain the letters "ui" but not the word, so they stay.

I replayed the saved results from the first scan without conducting a new search. This isolated the effect of the revised rule from any changes in GitHub's search results.

**Evidence — Saved scan replay**

```text
Original rule:
21 issues excluded
2 genuine frontend exclusions
19 incorrect exclusions

Revised rule:
2 issues excluded
2 genuine frontend exclusions
0 incorrect exclusions in the replay
```

From `scan-notes.md`, November 11, 2026:

> Ranking rule 5 matches "ui" as a substring, so it dropped "low-hanging fruit" (17), "quick fix" (6) and "trade-suite" (2). Only 2 of the 21 drops were real frontend labels.

> Replaying the saved scan-1 results (no new search) cut rule 5's drops from 21 to 2, and both of those were real frontend labels.

The correction demonstrated that a small ambiguity in a filtering rule could materially affect the opportunities presented by an AI-assisted search tool.

### Evaluating repository participation

A second weakness concerned how Issue Scout determined whether a repository had multiple contributors.

My first preferred-check wording looked for evidence that a non-owner had committed to the repository. This failed on an organization-owned repository called `agent-crew-harness`, where the only human contributor technically counted as a non-owner.

I rewrote the condition to require two distinct human contributors.

Using GitHub's contributors API, I verified that `agent-crew-harness` had one human contributor, whereas `flisk` had 19. Under the corrected rule, `flisk` would rank above `agent-crew-harness`, reversing their original relative order.

Another weakness emerged when two of the five leading issues already had pull requests proposing fixes. The search results alone did not expose that information. I therefore updated the issue-availability check to inspect pull-request activity through the issue timeline.

These changes emphasized the difference between finding a seemingly suitable issue and determining whether it represented an actual contribution opportunity.

### Testing the revised evaluation rubric

The November 11 regression evaluation achieved agreement on all 20 scored examples, exceeding the passing threshold of 18.

However, I continued revising the rubric rather than treating that result as final.

On November 22, I changed the scope criterion to avoid automatically rejecting issues merely because they included unresolved design details or involved several files. Instead, the revised rule focused on whether the issue contained a clearly identifiable piece of work.

This change produced a tradeoff.

**Evidence — Evaluation results**

```text
November 11:
Agreement: 20/20
Scope: 4/4
Result: PASS

November 22:
Agreement: 18/20
Scope: 2/4
Result: PASS

Passing threshold: 18/20
```

Two evaluation cases changed from reject to accept, reducing agreement with the reference labels.

I retained the new behavior rather than modifying the rubric simply to recover the earlier perfect score. The disagreement exposed a question about how the tool should balance ambiguity against potentially worthwhile contributions.

### Using Issue Scout in a real contribution

I next applied Issue Scout to three ChurchCRM issue URLs that I supplied directly for evaluation. The tool helped me select an email-documentation issue, originally tracked as ChurchCRM/CRM #10837 and subsequently moved to the documentation repository as issue #203.

On November 11, I claimed the issue and began preparing documentation changes.

The existing manual did not fully describe how ChurchCRM generated configurable email closings or displayed long links beneath buttons in automated messages.

My first implementation covered email closings, link wrapping, and subject-line behavior. It changed two documentation files.

During development, ChurchCRM maintainer DawoudIO merged pull request #228, which already addressed the subject-line documentation. Continuing with my original patch would have duplicated work that was now part of the upstream project.

I therefore rebuilt my branch against the updated repository and narrowed the contribution to the remaining documentation gaps.

The revised change affected only `docs/user-guide/email.md`, with eight lines added and one removed.

**Evidence — Documentation correction**

```text
Original documentation:
"The closing starts with 'Sincerely,' followed by
your name and the church name..."

Revised documentation:
"The closing is filled in for you: the Closing from
Reports → Admin → Report Settings..."

Additional documented behavior:
A long link beneath an email button wraps onto
several lines to fit a phone screen.
```

I checked the relevant behavior against ChurchCRM 7.8.6 source files, including `EmailComposerService.php` and `BaseEmail.html.twig`.

The documentation build and associated validation checks passed. My Ship Gate also verified that the changed files matched the intended scope.

After revising the pull-request description to satisfy my Voice Guard rules, I opened [ChurchCRM PR #252](https://github.com/ChurchCRM/docs.churchcrm.io/pull/252) on November 23.

The pull request remains open and has not yet received human maintainer review. The successful outcome so far is a tested, publicly submitted contribution—not a merged change.

## Limitations and what's next

Issue Scout improved substantially through real-world testing, but several limitations remain.

First, issue-availability checks can still miss work already completed in merged pull requests. One ChurchCRM issue was accepted even though a previously merged documentation PR had already addressed it. The tool needs to inspect historical pull requests as well as currently open ones.

Second, parts of the search configuration require manual maintenance. The recency filter uses a fixed date, while the search strategy cannot directly filter repositories by size or star count. These limitations can introduce outdated or unsuitable candidates.

Third, the latest rubric evaluation stands at 18/20, exactly the passing threshold. The two disagreements following the scope-rule revision deserve further investigation rather than being treated as irrelevant.

My next improvements would focus on checking completed pull requests, introducing relative date filters, and developing more reliable measures of repository suitability.

I would also like to test Issue Scout against language-processing projects relevant to my work on Syntext, including spaCy, Stanza, and Universal Dependencies. Those repositories offer an opportunity to evaluate whether the current selection rules remain effective across larger and more specialized codebases.

The broader lesson from this project is that an AI-assisted tool becomes more useful when its recommendations are treated as hypotheses to verify rather than conclusions to accept. Its value came not merely from returning GitHub issues, but from the cycle of examining mistakes, changing the underlying rules, and testing whether those changes actually improved the results.