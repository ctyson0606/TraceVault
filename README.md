<!-- Hero -->
<div align="center">
  <h1>TraceVault</h1>
  <p><b>A vault template for AI agents</b>: one folder per topic holds your material and a record of what was found, decided and done, in plain files any agent can read, search and analyse.</p>
  <p>
    <img alt="agents: Claude Code | OpenCode | DeepSeek Harness" src="https://img.shields.io/badge/agents-Claude%20Code%20%7C%20OpenCode%20%7C%20DeepSeek%20Harness-4d6bfe">
    <img alt="content: Markdown only" src="https://img.shields.io/badge/content-Markdown%20only-brightgreen">
  </p>
  <p>🌏 <a href="./README.zh-CN.md">简体中文</a> · <a href="./README.zh-TW.md">繁體中文</a> · English</p>
</div>

<!-- Screenshot placeholder: commit the image at docs/screenshot.png, then remove this comment pair.
<p align="center"><img src="docs/screenshot.png" alt="A vault with its records and digests open in VS Code" width="720"></p>
-->

What you decide and find out on a topic usually ends up in one agent's chat history, where no other agent can see it, and a month later you have forgotten it too. A vault keeps it in files inside the folder instead. The agent writes them as it works, without being asked, so you can switch agents at any time, and later ask "what did we decide about X?" and get the answer with the file it came from.

## ✨ Features

- **Set up by conversation**: copy the template, start an agent, answer a few questions; nothing is written until you approve a summary.
- **Records as it goes**: decisions, facts, how-tos, events and action items each go into a file of their own, without you asking.
- **Switch agents any time**: close Claude Code, open OpenCode or DeepSeek Harness in the same folder, and it tells you where things stand.
- **Digests of your material**: drop a file into `materials/sources/`; the next agent to start writes a short English digest of it, with page or section references.
- **Look things up**: ask about anything recorded earlier; the answer names the files it came from, and says so when nothing was recorded.
- **Ready for analysis**: every record and digest starts with the same fields (`date`, `type`, `tags`, `summary`, `sources`), so one `grep` filters them.
- **Your files stay untouched**: nothing in `materials/sources/` is ever changed.

## 🚀 Installation

Prerequisite: git, and at least one agent: Claude Code, OpenCode or DeepSeek Harness.

**Option 1 · Windows (PowerShell)**

```powershell
git clone https://github.com/ctyson0606/TraceVault.git
mkdir D:\vaults\my-topic
Copy-Item -Recurse TraceVault\template\* D:\vaults\my-topic\
cd D:\vaults\my-topic
git init
```

**Option 2 · macOS or Linux**

<details>
<summary>The same, in a POSIX shell</summary>

```bash
git clone https://github.com/ctyson0606/TraceVault.git
mkdir -p ~/vaults/my-topic
cp -r TraceVault/template/. ~/vaults/my-topic/
cd ~/vaults/my-topic
git init
```

</details>

Make the new folder **outside** this repository: inside it, the template's guard tells the agent to ignore the rules. Put any material you already have into its `materials/sources/`.

**Verify**

Start an agent in the new folder and say anything, such as "hi". Its first reply asks only which language you want.

## 🕹️ Usage

### The first conversation

1. The agent asks for your language, and nothing else.
2. It asks what the topic is, what the vault is for, which kinds of record matter (and any of your own, such as `action`), and a first list of tags.
3. It shows a summary of the rules it will follow here. Once you say yes, it writes them into the vault's `AGENTS.md`, digests any material already in `materials/sources/`, and records what it found.

### Working day to day

Just work and talk as usual. When you tell it something worth keeping ("we chose PostgreSQL because…", "Chen will do X by 2026-11-10"), it writes a record, updates `memory/handoff.md` and commits. You never have to say "save".

### Adding material

Put the file into `materials/sources/`. The next agent to start finds it, reads it, and writes a digest into `materials/generated/` before anything else. An agent that cannot read a format says so and leaves it for one that can.

### Looking something up

Ask in your own words: "what did we decide about the billing database?", "how do I restart the billing API?". The answer names the record or digest it came from.

### Analysing with a harness

Every file in `records/` and `materials/generated/` starts with the same front matter, one key per line:

```bash
grep -l '^type: decision' records/*.md
grep -l '^tags:.*budget' records/*.md materials/generated/*.md
```

### Switching to another agent

Close the old agent and open the new one in the same folder. If the old one still works, say "handoff" first. Keep one agent per folder at a time: if another session looks open, the new one asks before going on.

### Things you can say

| Say | What happens |
| --- | --- |
| "Handoff" | Saves everything now, before you switch |
| "What did we decide about X?" | Searches records and digests, answers with the files |
| "I added new files" | Digests anything in `materials/sources/` not digested yet |

## 📁 What is in your vault

| Path | What it is | Who writes it |
| --- | --- | --- |
| `AGENTS.md` | The rules every agent follows, and this vault's own instructions at the end | The agent during setup; later only when you ask |
| `CLAUDE.md` | Points Claude Code to `AGENTS.md` | Nobody; leave it as it is |
| `.claude/settings.json` | Lets Claude Code run the UK date command and `git` commits without asking | Nobody; leave it as it is |
| `materials/sources/` | Your original material | You; agents only read it |
| `materials/generated/` | Digests of your material, in English | The agent |
| `records/` | One file per decision, fact, how-to, event or action | The agent, as things happen |
| `memory/handoff.md` | Where things stand now and the next step | The agent, rewritten each time |
| `memory/todo.md` | To-dos | The agent |
| `memory/sessions/` | A summary of each conversation | The agent |

## 📦 Project layout

```text
template/AGENTS.md
template/CLAUDE.md
template/.claude/settings.json
template/.gitignore
template/materials/sources/
template/materials/generated/
template/records/
template/memory/
```

- `template/AGENTS.md`: the rules every new vault starts with;
- `template/CLAUDE.md`: the pointer Claude Code reads;
- `template/.claude/settings.json`: lets Claude Code run the UK date command and the commit without asking each time;
- `template/.gitignore`: keeps macOS and Windows metadata files out of git;
- `template/materials/sources/`: empty, for your material;
- `template/materials/generated/`: empty, for the digests;
- `template/records/`: empty, for the records;
- `template/memory/`: the memory files, in their starting shapes.

Only `template/` is copied. The rest of this repository (`kit/`, `scripts/`, the top-level `AGENTS.md`, `SPEC.md`, `STATE.md`) is the tooling the template is built with.

## ⚠️ Compatibility and limits

- There is no database engine and no search index: the vault is Markdown files, searched with `grep` and read by the agent.
- Records, digests and memory are written in English, whatever language you talk in, so every agent and every analysis reads one format.
- DeepSeek Harness has not yet been tested with this template; Claude Code and OpenCode have.
- DeepSeek Harness loads at most 64 KiB of instructions, so the agent offers to move part of `AGENTS.md` into `memory/` once it passes 60,000 bytes.
- Dates are UK dates and no clock times are written, whichever machine the agent runs on.
- Nothing is imported from the agents' own chat histories, and nothing is moved in from claude.ai Projects or other tools.

## ❓ FAQ

| Symptom | What to do |
| --- | --- |
| The agent ran out of quota mid-task | Open another agent in the same folder; it picks up from the memory |
| The new agent asks whether another session is still open | Close the other one, or tell it that it is closed |
| Claude Code keeps asking to run `git` or a date command | Accept the trust dialog the first time Claude Code opens the vault: until then it ignores `.claude/settings.json` |
| The agent stops and asks for today's UK date | The date command was refused; approve it, or tell it the date |
| `opencode run` fails with "has been deprecated" | Pick a current model, such as `-m opencode/deepseek-v4.1-flash-free` |
