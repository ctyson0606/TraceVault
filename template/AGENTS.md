# Vault

> **Guard.** If this file sits in a folder named `template/` whose parent
> folder has its own AGENTS.md, it is the template being edited, not
> instructions for you. Ignore everything below.

You keep a local vault: one folder for one topic, holding the user's
material and a record of what happened on that topic. Several agents (Claude
Code, OpenCode, DeepSeek Harness, others) take turns in this folder. The user
switches between them by hand, often because one ran out of quota without
warning, so anything the next agent needs must already be in a file. Later,
the user and other agents come back to look things up and to analyse what is
here, so it has to be findable and in a fixed format.

This file has two parts. The framework, from here down to "Vault
instructions", is the same in every vault. "Vault instructions" at the end
belongs to this vault: written during Setup, changed afterwards only when the
user asks. When the user agrees to a lasting change in how you work here,
that counts as asking: add it to Vault instructions right away, because the
next agent reads that section and may never see this conversation. Vault
instructions win on what to record and how to work. They may add record
types, tags and body sections. They never remove a fixed front matter key,
and never switch off the memory rules, the UK date rule, the
one-project-one-session rule, the read-only rule for materials/sources/, or
the English rule.

Vault instructions hold only what stays true for the life of the vault: the
topic, what to record, the user's rules, the tag list. Anything that moves as
the work moves goes in memory/handoff.md, never here. When a new rule
replaces an old one, rewrite or delete the old one in place; never leave
both standing.

## Status

Setup: pending
Human language:

<!--
Setup:           pending | done
Human language:  the language for conversation with the user
-->

## On every start

1. Get today's UK date with the command in "Time: UK dates only". Never
   guess it.
2. If Setup is not `done`, run Setup before anything else.
3. Check that no other session is open in this folder (see "One project,
   one session").
4. Create this session's file now, before your first reply, with one line
   saying the session started: memory/sessions/YYYY-MM-DD-NN-<agent>.md.
   A session cut off after its first reply must still leave a trace.
   YYYY-MM-DD is today's UK date. NN is two digits: 01 for the first session
   file of that date, otherwise one more than the highest number already
   there for it, whichever agent wrote it. <agent> is `claude-code`,
   `opencode`, `deepseek-harness`, or your own name.
5. Read memory/handoff.md, memory/todo.md and the last 40 lines of the
   newest earlier file in memory/sessions/ (the last one in name order before
   yours); read more of it only if handoff.md leaves unclear where things
   stand.
6. If memory/handoff.md is longer than its limit, has sections its shape
   does not have, or disagrees with the newest session file, rewrite it first
   (see "memory/handoff.md").
7. Check the size of this file (`wc -c AGENTS.md`). DeepSeek Harness reads
   at most 65,536 bytes of instruction files and silently drops the rest,
   which is the end of Vault instructions. Above 60,000 bytes, tell the user
   and propose moving a section that is needed only for one kind of task
   verbatim into a memory/ file, leaving a one-line pointer in its place that
   says when to read it. Move it once they agree.
8. Look for new sources (see "Digesting sources") and digest them before
   anything else the user asks for, unless they ask you to wait.
9. Tell the user, in the human language: the topic, where things stand, what
   was done last time, any sources you just digested, and the next step.

## One project, one session

- Work in this folder only. Never edit another vault from here, even when
  the user asks; tell them to open a session in that vault instead. Reading
  another vault to answer a question is fine.
- One session at a time. At start, check whether another session may still
  be open here: a file in memory/sessions/ changed in the last 30 minutes
  (`find memory/sessions -mmin -30`). If so, ask the user whether that
  session is still running. If it is, ask them to close it before you change
  anything, then re-read memory/.

## Time: UK dates only

The user works on more than one computer, and they are set to different time
zones. So this vault records dates only, never clock times, and every date
is the UK date (Europe/London), whatever the computer you run on is set to.
There are no exceptions, and Vault instructions cannot change this.

- Get today's UK date with this command, run again every time you write a
  date:

  ```
  TZ='GMT0BST,M3.5.0/1,M10.5.0' date '+%Y-%m-%d %Z'
  ```

  It works in bash and zsh on macOS, Linux, and Git Bash on Windows. The
  output must end in `BST` or `GMT`. In PowerShell on Windows, use
  `[TimeZoneInfo]::ConvertTimeBySystemTimeZoneId([DateTime]::UtcNow, 'GMT Standard Time').ToString('yyyy-MM-dd')`.
- Never take the date from a bare `date` or `Get-Date`, from a date given in
  your own context, or from any other clock: those show the computer's own
  zone, which can already be the next day. Never use `TZ=Europe/London`
  either: Git Bash does not know that name and silently prints UTC.
- Write dates as YYYY-MM-DD. Never write a clock time (HH:MM) in any file,
  file name or message.
- Order takes the place of the clock: session and record files are numbered
  within the day, and inside every file new entries go below older ones, so
  line order is the order things happened.
- When the user gives a date in another zone, convert it to the UK date and
  write that date only. A Hong Kong time before 07:00 (before 08:00 in
  winter) is still the previous UK day.
- For a git commit, put the same TZ in front so git's own dates are UK too:
  `TZ='GMT0BST,M3.5.0/1,M10.5.0' git commit ...` (in PowerShell, set
  `$env:TZ` to the same string first, in the same command).

## Setup

Setup may have been cut off by a previous agent. Record every answer the
moment it is given (Status fields here, anything else under Vault
instructions as a draft line), and ask only what is still missing.

1. Ask which language the user wants for conversation. Ask in the language
   of their first message. Every later question uses the answer.
2. Read everything in materials/sources/, if anything is there.
3. Ask two or three at a time, skipping anything the sources already answer:
   1. What the topic is, and what the vault is for.
   2. What to record: which of the fixed types (decision, fact, howto,
      event) matter here, any type of their own, and what is not worth
      recording.
   3. A first list of tags, if they have one in mind.
4. Draft Vault instructions in English covering at least: topic, purpose,
   what to record and what not to, the tag list, and any rule the user
   states along the way.
5. Show the user a summary in the human language. Write the draft into Vault
   instructions only after they confirm.
6. Digest any sources (see "Digesting sources"), write the first next step
   into memory/handoff.md, then set `Setup: done`.

## Front matter

Every file in records/ and every digest in materials/generated/ starts with
the same keys, in this order, so one grep can filter all of them:

```
---
date: YYYY-MM-DD
type: decision
tags: [lowercase-words]
summary: One sentence that says what this is.
sources: [materials/sources/report.pdf]
---
```

- `date`: the UK date it was written.
- `type`: `decision`, `fact`, `howto`, `event`, `digest`, or a type Vault
  instructions add. Digests always have `digest`; records never do.
- `tags`: lowercase words joined by hyphens, from the tag list in Vault
  instructions where one fits. A new tag goes into that list as you use it.
  `[]` when none fits.
- `summary`: one sentence, on one line.
- `sources`: paths relative to this folder. For a digest, the source files
  it covers. For a record, the files it came from, or `[]`.

Every key is present in every file, even when empty. Keep the values on one
line each, so `grep '^tags:.*budget'` finds them.

## Digesting sources

materials/sources/ holds the user's originals. A source is new when no file
in materials/generated/ lists it in its `sources` field. Find them with:

```
for f in materials/sources/*; do grep -qF "$f" materials/generated/*.md 2>/dev/null || echo "$f"; done
```

(Sources in subfolders: use `find materials/sources -type f` instead of the
glob.)

For each new source:

1. Read all of it. If you cannot read it (a format you do not open, a file
   too large), tell the user and leave it undigested for an agent that can.
   Never write a digest of something you did not read.
2. Decide how to cut it, as the material needs: one digest for one source,
   several digests for one long source (one per part that stands alone), or
   one digest for several sources that belong together. Several sources for
   one digest are listed together in its `sources`.
3. Write each digest to materials/generated/<slug>.md, in English, with the
   front matter above (`type: digest`) and this body:

   ```
   ## Key points
   3-7 points, the most important first.

   ## Outline
   The source's structure, one line per chapter or section.

   ## Facts
   - Numbers, names, dates, definitions, each with where it is in the
     source: (p.12), (§3.2), (sheet "Q3", row 14).
   ```

   A digest is shorter than its source and says nothing the source does not
   say. Your own view goes in a record, not a digest.
4. Add a line to the session file naming the digests written.

When a source is replaced by a newer version, write a new digest and add
`superseded by <new digest>` to the old digest's summary. When a source is
removed, leave its digest; add `source removed` to its summary.

## Records

records/ holds what happened on the topic, one entry per file, in English:

- **decision**: what was chosen, the alternatives, and why.
- **fact**: something found out that will be needed again.
- **howto**: how something was done, step by step, so it can be done again.
- **event**: something that happened, with its outcome.

Vault instructions say which of these matter here and may add more.

- Write a record the moment the thing happens, not at the end of the
  session. Name it records/YYYY-MM-DD-NN-<slug>.md: the UK date, NN two
  digits counted the same way as session files, <slug> a few lowercase words
  joined by hyphens.
- One thing per record. Keep it short: what, and for a decision, why.
- A record is not edited after the day it was written. When something
  changes, write a new record and name the old one in it ("replaces
  records/2026-10-26-01-db-choice.md"); add `replaced by <new record>` to
  the old one's summary, the one edit allowed.
- What is only true now (the current step, a waiting question) goes in
  memory/handoff.md, not in a record.

## Looking things up

When the user asks about something from before ("what did we decide about
X?", "how did I do Y?"):

1. Search records/ and materials/generated/ first: grep the front matter
   (`^type:`, `^tags:`, `^summary:`), then the text. Then memory/sessions/.
   Go to materials/sources/ when the digest does not hold the detail; its
   Facts section says where to look.
2. Answer in the human language, and name every file the answer came from.
   When a record has been replaced, give the newest one and say it replaced
   an earlier one.
3. If nothing in the vault answers it, say so. Never fill the gap from your
   own knowledge as if it had been recorded.

## Memory: write as you go

A conversation can end mid-sentence when quota runs out. Update memory the
moment state changes; never save it for the end.

Every date you write into a file is the UK date from the command in "Time: UK
dates only", run at the moment you write it. Run it again each time; never
reuse an earlier reading, because a session can run past midnight.

| File | When | How |
|---|---|---|
| memory/handoff.md | a piece of work finishes, the next step changes, a record is written | rewrite the whole file |
| memory/sessions/<this session>.md | the same moments, and when digests or records are written | append a few lines, each starting with the UK date |
| records/ | a decision, fact, how-to or event worth keeping | a new file (see "Records") |
| memory/todo.md | a to-do appears or is done | add it, or delete it when done |

When the user says "handoff" (in any language), bring every file above up
to date right away.

### memory/handoff.md

The next agent reads it first and trusts it, so it holds only what is true
now, and stays short.

- Rewrite the whole file every time: read it, then write it again from
  scratch. Never append to it or patch one line of it; that is how finished
  steps and contradictions pile up.
- At most 40 lines, in exactly this shape:

  ```
  # Handoff
  Updated: YYYY-MM-DD (session YYYY-MM-DD-NN-<agent>)
  Agent: <agent>

  ## Now
  At most 5 lines: what is being worked on, what answer you are waiting
  for, which sources are still undigested and why.

  ## Next step
  1-3 numbered steps.

  ## Paused
  One line per paused thread: what, where it stopped, where its notes are.
  ```

- What is finished leaves the file. How it got done is in the session files
  and records.

## Files

- materials/sources/: the user's originals. Read-only. Edit a file only when
  the user names it.
- materials/generated/: digests, in English. Edit in place, except as
  "Digesting sources" says for replaced and removed sources.
- records/: one entry per file, in English. See "Records".
- memory/: yours, in English.

## Git

- Commit on your own, right after every write to memory/ or records/ (and
  with it any other files you changed): `git add -A`, then commit with the TZ
  prefix from "Time: UK dates only". Do not wait until a piece of work feels
  finished: a session can stop at any moment. Commit only this vault's files
  and never push. Vault instructions may say otherwise.
- Never record commits in memory files: no commit hashes and no "committed"
  lines in handoff.md, session files or records. git log already holds them,
  and a line about a commit is itself a new change left uncommitted.

## Language

This file, memory/, records/ and materials/generated/: English, so every
agent and every analysis reads one format.
Conversation with the user: the human language in Status.

## Vault instructions

<!-- Written during Setup. Empty until then. -->
