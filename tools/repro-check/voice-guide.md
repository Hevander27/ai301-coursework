# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

I'm a student in CodePath's AI301, making my first open-source
contributions, starting in the course's Path Review repo. I work with
Claude Code as a tool, and I run every command I report myself.
Readers can expect me to show my output, say plainly what I know and
what I'm guessing, and come back to the thread when I said I would.

## Rules I write by

### Rule: Paste it, don't promise it

Every "reproduced" or "confirmed" sits directly above the output that
shows it. If I can't paste it, I don't claim it.

- Wrong: "Confirmed, this is definitely broken on my machine too."
- Right: "Reproduced on `main` at `a1b2c3d` (Python 3.12, macOS 15): `pytest tests/unit/test_structural_chunker.py --runxfail -k no_headings` fails with the assertion below."

### Rule: Label my guesses

A cause I haven't tested is a guess, and I say so in the same
sentence. Only a log line, a failing test, or a diff turns it into a
finding.

- Wrong: "The bug is in the heading regex in the structural chunker."
- Right: "My guess is the heading regex in the structural chunker; I haven't confirmed that yet, and I'll post what I find."

### Rule: No dates, no guarantees

I say what I'm doing next, never when it will be done or that it will
work. Course deadlines are mine to manage, not the maintainer's.

- Wrong: "I'll have a PR up by tomorrow, guaranteed."
- Right: "Next I'm going to read how the parser handles empty sections, and I'll report back here."

### Rule: Write it so it only fits this issue

If my comment could be pasted onto any other issue unchanged, it's
filler. It has to name this issue's symptom, file, or test.

- Wrong: "Hi! This looks like a great first issue for me. Can I be assigned?"
- Right: "I'd like to take this. The `xfail` test for heading-less documents fails for me on `main`, and I want to start from `ingestion/chunking/`."

### Rule: My own repro, in my own words, owned by me

On a shared issue I post my own reproduction from my own environment,
not a "same here". When a repo asks for AI disclosure, I state the
tool and what it did. Everywhere, I've read and can defend every line
before it goes up.

- Wrong: "Same as above, can confirm."
- Right: "Posting my own run since my environment differs from the one above (Docker instead of a local venv); same failing assertion, output below."

## Things I never post

- A "+1", "same here", or "any updates?" with nothing of my own in it.
- A deadline, a guarantee, or "please keep this reserved for me".
- A root cause stated as fact without a log, test, or diff behind it.
- Text an assistant drafted that I haven't rewritten and checked line by line.
- Over-thanking or "sir/ma'am" padding in place of content.
- Disagreement with a classmate's claim or repro. Their claim doesn't block mine, and I don't need to argue with it.
