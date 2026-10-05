# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

A package has two sides. The issue side (what the bug is, where it
was seen, what triggers it) is the yardstick. The candidate side (the
claim comment and the repro report) is what gets measured. Read the
issue side first, every time, and write down three things before
opening the report: the exact symptom, the trigger elements the issue
or a maintainer calls required, and the environment factors the issue
says matter.

## Environment

**Where it lives.**

- Eval bundle, candidate side: the line or table in the repro report
  that names versions, OS, install method, and settings. It is usually
  labeled "Environment", but a version printed in a transcript
  (`tool --version`) counts too.
- Eval bundle, issue side: the issue body's version and OS lines, the
  reporter's `show_versions` or `conda info` summary, and thread
  comments that say a factor matters ("only in Release builds", "only
  with driver X"). The repo-facts
  "bug reports" line says which versions the repo expects tested (for
  example "confirm on the latest version and on main").
- Live mode: the draft repro comment is the candidate side. For a Path
  Review issue the factors that usually matter are the commit of
  `main` tested (short SHA), the Python version, the OS, and whether
  the run used the local virtualenv or Docker (`docs/SETUP.md` names
  both). The issue thread on GitHub is the issue side.

**What good looks like.** The report names the software version and
the OS, plus every factor the issue says changes the behavior. The
version is the issue's, newer, or current, or the report states the
difference in plain words ("filed against 2.1; tested 2.4").

**Traps.**

- No environment record at all is a fail even when the artifact looks
  right. Without it, nobody can place the attempt, and on issues where
  the build profile, driver, or shell changes the failure mode the
  artifact cannot be interpreted.
- An older version than the issue targets, tested silently, is a
  wrong-target environment. Its output describes that old version, not
  the reported bug.
- A different OS, shell, or newer version that the report calls out
  is fine. Calling out the difference is the whole requirement.

## Steps

**Where it lives.**

- Eval bundle: the repro report's numbered steps and its fenced
  command transcripts. A command shown after a `$` prompt in an output
  block is a step.
- Issue side: the issue's own reproduction steps and inputs, plus
  maintainer comments that add trigger requirements ("it only happens when a
  second flag is also passed").
- Live mode: the draft repro comment. In Path Review, many issues are
  seeded bugs with a failing unit test marked `xfail`; the clean steps
  are the clone or checkout, the setup command from `docs/SETUP.md`,
  and the exact `pytest` invocation (with `--runxfail` so the marked
  test actually reports its failure).

**What good looks like.** A stranger could start from the stated
starting state and reach the trigger without asking a question. Each
step is a command, an input, a file's content, or a setting. Pointing
to an input the issue already supplies ("the input file exactly as given
in the issue") is complete, and so is describing an input by every element
the issue says matters ("a config file with a valid section
plus the one unknown key the issue names").

**Traps.**

- Private code or private configuration (a company repository, an
  internal config file that is not shared) makes every later step unrepeatable, however
  convincing the output.
- Dropping a trigger element the issue names (the driver on a
  driver-specific bug, a required flag) is a steps failure even when
  the output looks similar.
- Short is not incomplete. Four exact steps beat a page of narration.

## Behavior shown

**Where it lives.**

- Eval bundle: every fenced block in the repro report (output, logs,
  exit codes, rendered results, parser errors), plus any plain-text
  description of an observation that a command produced ("the window
  remained open and the prompt returned").
- Issue side: the issue's quoted error text, exit code, wrong value,
  or missing output, and the exact command or input that produced it.
- Live mode: the output blocks pasted into the draft. For a Path
  Review seeded bug, the strongest artifact is the failing test's
  assertion line together with the test's node id.

**What good looks like.** Put the issue's trigger next to the report's
command and check that they match element by element: same syntax,
same operator, same expression, same flags. Then put the issue's
symptom next to the report's output and check that it is the same
kind of failure: same error class or message, same exit code, same
wrong value. A control run that changes only the trigger and makes the
symptom disappear is the strongest form.

**Traps.**

- Adjacent symptoms. A graceful argument-validation error is not a
  panic, and a different exit code is a different outcome. A syntax
  error from a mistyped input (the class activity's `:` where the
  format needs `=`) is not the issue's crash. A compile-time error is
  not the runtime error the issue reports. Corrupted output from a
  program that keeps running is not a crash.
- Setup shown as proof. A version banner, a list of running
  sessions, or "the window opened" shows the software runs; it shows
  nothing about the bug.
- Narration over artifact. When the prose says "exactly as the issue
  describes" and the output shows something else, the output wins.

## Honesty

**Where it lives.** Every assertion sentence in the claim comment and
the repro report ("I reproduced", "confirmed", "the root cause is",
"I understand the internals"), each matched to the artifact that
should back it. In live mode, the same sentences in the draft files.

**What good looks like.** Each assertion points at something in the
report: an output, a log line, a measurement. Guesses are labeled as
guesses ("seems", "my guess is"). An honest cannot-reproduce states
the result first, shows the attempt's actual output, and names what
differed from the reporter's setup and what a triggering setup might
need. That is a complete and useful package.

**Traps.**

- Confidence as a substitute for proof: "100% confirmed",
  "guaranteed", "every single time", "on several machines". Count the
  artifacts, not the adjectives.
- A root cause stated as fact or as "verified" with no transcript,
  log, measurement, or test behind it.
- Generalizing beyond what was shown, such as claiming the bug exists
  on a build or platform where the report's own artifact does not show
  it.
- Process statements are not overclaims. "I ran every step myself" or
  "I used an AI assistant to organize this" describes how the author
  worked and is not graded here.

## Comms

**Where it lives.**

- The claim comment: the candidate claim comment section of the bundle,
  or the student's `claim.md` draft in live mode, read against the
  issue title, body, and thread.
- The repo's rules: the repo-facts "contribution policy" line in a
  bundle (it quotes or summarizes CONTRIBUTING.md and any AI policy
  file). In live mode, the repo's CONTRIBUTING.md (Path Review keeps
  it at `docs/CONTRIBUTING.md`), any `AI_POLICY.md`, and the issue and
  pull request templates under `.github/`. Path Review's house rules in
  `scope.md` also apply in live mode: a classmate's claim does not
  block a claim, and a classmate's repro does not replace one.

**What good looks like.**

- A claim comment that could only belong to this issue: it names the
  symptom, the component, or a thread pointer, and says what the
  author will do next. Modest is good ("I'd like to look into this";
  "next I'll read the module the maintainer pointed to").
- Boilerplate fails: praise for the project, "please assign this to
  me", "reserve it for me", or a guaranteed deadline. So does a +1,
  an "any updates?", or a bare "claiming this one" that names no next
  action, and so does self-assignment. Wanting the issue is not a
  plan; the comment has to say what the author will do first.

**Reading an AI policy.** Treat every eval package as AI-assisted
work. Then sort the policy into one of these.

- **Silent or permissive.** "No stated AI policy", or "AI tools
  welcome, you are responsible for what you submit". No disclosure is
  needed. Pass.
- **Conditions that are not disclosure.** "Understand and test what
  you submit", "low-quality AI content is closed", "comments must be
  in your own words". None of these asks the comment to disclose
  anything. A specific, human-sounding claim comment meets an
  own-words rule. Pass.
- **Disclosure scoped to pull requests.** "State the tool and extent
  in the pull request", with no ask for issue comments. Issue comments
  owe nothing yet. Pass.
- **Disclosure that covers comments.** "All AI usage in any form must
  be disclosed", or a rule naming issues and comments. Pass only if
  the claim comment or the repro report states the AI use in words. An
  excellent repro does not excuse a missing disclosure.

Grade only what the policy line says. Do not invent a requirement the
line does not state, and do not drop one it does.
