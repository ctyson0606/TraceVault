# SPEC

The contract this project is built against: what the spec gate produced and a
person approved. It stays true for as long as the code does, so it is kept
apart from STATE.md, which is replaced as the work moves.

Change it only through the spec gate, never as you go, except a goal's How and
Parts lines, written once its criteria all have evidence. What no longer holds
is rewritten in place, not appended to; git holds the history.

## Problem

What happens on a topic (what was found, decided, done and why) is scattered
across each agent's own history, where the other agents cannot read it. A
new agent starts with no idea where the work stands, and after a while the
user forgets too. The user needs one local folder per topic that holds the
material and a record of what happened, in plain files every agent can read
and write, kept up to date as the work goes, so an agent harness can analyse
it and the user can look up what they forgot.

## Milestones

- M1: v1 — the vault template

## Goals

Build `template/`: a copy-to-adopt vault template, modelled on
D:\codingProject\Gen\LearningProject's template without the teaching,
readable and writable by Claude Code, OpenCode and DeepSeek Harness. Spec
approved 2026-10-26.

- G1 (M1): Start a new vault: copy the template into a folder and start an
  agent; it asks the language, then what the topic is and what to record,
  and writes the vault's instructions only after the user approves a
  summary of them.
  - How: The template's AGENTS.md has a Setup that asks only the language in
    its first message, then the topic, what to record and the tags, and
    writes Vault instructions after the user confirms a summary.
  - Parts: C1, C2
- G2 (M1): The agent records as it works, without being asked: where things
  stand, what happened in each session, and one record per decision, fact,
  how-to or event, committing after every memory write.
  - How: The memory table in AGENTS.md tells the agent to rewrite handoff.md,
    append to the session file and write a record the moment something
    happens, then commit with the UK TZ prefix.
  - Parts: C1, C3, C4
- G3 (M1): Switch agents at any time — Claude Code, OpenCode, DeepSeek
  Harness — and the new one picks up where the last one stopped.
  - How: Every agent reads the same plain files: the start routine reads
    handoff.md, the to-do list and the end of the last session file, then
    states where things stand. The rules live in AGENTS.md, which all three
    agents load by themselves, and the UK date has a form each one can run.
  - Parts: C1, C2, C3
- G4 (M1): Look things up: ask about something recorded earlier and the
  agent answers from the vault, naming the file it came from.
  - How: "Looking things up" in AGENTS.md sends the agent through the front
    matter of records and digests, then sessions, then the sources, and
    requires every answer to name its files.
  - Parts: C1, C4, C5
- G5 (M1): Analyse: every record and digest carries the same fixed fields,
  so a harness can filter and count them with a single command.
  - How: Every record and digest starts with the same five front matter keys,
    each on one line, so `grep '^type: decision'` or `grep '^tags:.*budget'`
    selects them.
  - Parts: C1, C4, C5
- G6 (M1): The user's own material is never changed.
  - How: AGENTS.md makes materials/sources/ read-only; agents write digests
    to materials/generated/ instead.
  - Parts: C1, C5
- G7 (M1): New material is digested automatically: at every start the agent
  finds sources no digest covers yet, analyses them, and writes an English
  digest into materials/generated/.
  - How: Step 8 of the start routine runs a one-line check for sources no
    digest lists in its `sources` field, and "Digesting sources" fixes the
    digest's shape: Key points, Outline, and Facts with locations.
  - Parts: C1, C5

## Non-goals

Teaching of any kind (blackboard, learning modes, progress tracking of what
the user knows); migrating from claude.ai Projects or any other tool; a
database engine (SQLite or a server); vector search or embeddings; importing
the agents' own histories; a UI; a manager for several vaults; quota
detection or automatic agent switching.

## Acceptance criteria

Each is run with the real agent; nothing counts unrun.

- AC1 (G1): Copy the template to an empty folder and start Claude Code: it
  asks the language first, then the topic and what to record, and writes
  Vault instructions only after showing a summary and getting a yes.
  - Means: a new vault is set up by conversation, and nothing is written
    that the user did not approve.
  - Try: copy `template/` to a new folder, open Claude Code there and say hello.
- AC2 (G2): Work in the vault without mentioning memory: memory/handoff.md,
  the session file and records/ are updated, and git log shows the commits.
  - Means: the user never has to ask for anything to be saved.
  - Try: work for a while, then look at memory/, records/ and `git log`.
- AC3 (G3): Close Claude Code without saying "handoff", open OpenCode: it
  states the topic, where things stand, what was done last time and the
  next step.
  - Means: running out of quota in one agent loses nothing.
  - Try: close one agent mid-task and open another in the same folder.
- AC4 (G3): The same as AC3 with DeepSeek Harness.
  - Means: the third agent picks up as well.
  - Try: as AC3, opening DeepSeek Harness instead.
- AC5 (G3): The reverse direction, OpenCode to Claude Code, passes.
  - Means: switching works both ways.
  - Try: as AC3, from OpenCode back to Claude Code.
- AC6 (G4): Ask about something recorded in an earlier session: the answer
  is right and names the record or digest it came from.
  - Means: the vault answers "what did I decide / how did I do X" and can be
    checked.
  - Try: in a new session ask "what did we decide about <thing>?".
- AC7 (G5): One grep lists every record of a given type or tag, and every
  file in records/ and every digest has all its fixed fields.
  - Means: a harness can analyse the vault without guessing its format.
  - Try: `grep -l '^type: decision' records/*.md`.
- AC8 (G7): Put a file into materials/sources/ and start an agent: it writes
  an English digest into materials/generated/ whose `sources` field names
  that file, without being asked.
  - Means: material dropped in is summarised and becomes searchable.
  - Try: drop a PDF into materials/sources/ and start any agent.
- AC9 (G6): Nothing in materials/sources/ is modified at any point, checked
  with git.
  - Means: the originals stay exactly as the user put them.
  - Try: `git log --stat -- materials/sources/` shows only additions the user made.

## Components

- C1: Vault rules (`template/AGENTS.md`) — the start routine, setup, source digesting, recording, lookup and the rules every agent follows
- C2: Claude Code pointer (`template/CLAUDE.md`) — sends Claude Code to the template's AGENTS.md; uses C1
- C3: Memory (`template/memory/`) — handoff, to-dos and session files every agent reads and writes
- C4: Records (`template/records/`) — one file per decision, fact, how-to or event, with fixed front matter
- C5: Materials (`template/materials/`) — the user's sources, never modified, and the agent's digests of them

## Data and API contract

```
template/
  AGENTS.md            Guard; framework (Status, start routine, Setup,
                       digesting, recording, lookup, files, time, git,
                       language); then "Vault instructions", this vault's own
  CLAUDE.md            "@AGENTS.md" only (Claude Code). OpenCode and DeepSeek
                       Harness read AGENTS.md directly.
  .gitignore           OS metadata (._*, .DS_Store, .smbdelete*, desktop.ini,
                       Thumbs.db) never enters git
  materials/
    sources/           The user's originals (read-only)
    generated/         Digests: <slug>.md, English, one per need (one source
                       to one digest, one to many, or many to one)
  records/             YYYY-MM-DD-NN-<slug>.md, one entry each, English
  memory/
    handoff.md         Current state only, rewritten whole, at most 40
                       lines: Updated, Agent; Now, Next step, Paused
    todo.md            To-dos
    sessions/          One summary per conversation:
                       YYYY-MM-DD-NN-<agent>.md (NN = order within the day)
```

Front matter, the same keys in every record and digest, in this order:

```
---
date: YYYY-MM-DD           UK date it was written
type: decision | fact | howto | event | digest
tags: [lowercase-words]    may be empty: []
summary: one sentence
sources: [paths]           digests: the source files covered; records: files
                           it came from, or []
---
```

A digest's body has `## Key points` (3-7, most important first), `## Outline`
(the source's structure, one line per part) and `## Facts` (numbers, names,
dates, definitions, each with its location in the source such as `p.12` or
`§3.2`). A record's body is free text, short, with the reason for a decision.

A source is new when no digest lists it in `sources`. Vault instructions may
add types and body sections; they never remove a fixed key.

Rules carried over from LearningProject unchanged in intent: UK dates only
and no clock times; the agent commits after every memory write and never
pushes; one project, one session; AGENTS.md checked against 60,000 bytes at
every start; AGENTS.md, memory/, records/ and digests in English, the
conversation in the user's chosen language. Vault instructions never switch
these off.

## Assumptions

- Records rather than a decisions file hold decisions, so memory/ has no
  decisions.md.
- OpenCode and DeepSeek Harness may not read PDFs; an agent that cannot read
  a source says so and leaves it for an agent that can, rather than writing
  a digest it did not read.
