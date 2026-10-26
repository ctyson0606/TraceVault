# STATE

> Last updated: 2026-10-26

Current working state. Superseded content is deleted, not archived.
For durable rules see METHOD.md.

<!--
Do not write into this file:
  - anything the repo already answers: file lists, route lists, branch,
    commit, whether something is pushed
  - any claim that the act of committing makes false, e.g. "not yet pushed"
-->

## Current Focus

M1, the vault template in `template/`, is built to the spec approved
2026-10-26. Every criterion has evidence from one-shot runs on a scratchpad
vault, so every M1 goal is done.

## Next Steps
1. Run the criteria once in the interactive UIs (Claude Code in VS Code,
   OpenCode TUI): every run so far was headless (`claude -p`,
   `opencode run`), and Claude Code there used Sonnet 5.5.
2. Use the template for a first real vault; watch whether agents write
   howto records for procedures found inside sources (the on-call runbook
   test got a digest and no howto record, which the spec allows), and
   whether answers stay inside what was recorded (DeepSeek Harness added
   "snapshot frequency unchanged", which no record says).
3. Still untested: a question nothing answers, a replaced source, an
   unreadable format, PDFs in OpenCode and DeepSeek Harness. Replacing a
   record was exercised 2026-10-26 (Claude Code wrote it, DeepSeek Harness
   answered from the newest one).
4. README.md, README.zh-TW.md and README.zh-CN.md written 2026-10-26, with
   git clone; the PowerShell copy step was run and copies every template
   file. Add a screenshot at docs/screenshot.png when there is one.

## Open Questions
- The 30-minute open-session check fires on back-to-back sessions, which is
  correct by the rule but is the false-positive pattern LearningProject saw.
  Left as in LearningProject until a real vault shows it matters.

## Acceptance status

- AC1: evidenced — `claude -p "你好"` (Sonnet 5.5) on a fresh copy of the template: the first reply asked only the language; then topic, types and tags; then a summary; Vault instructions written only after "確認". The first run, before the Setup step 1 fix, bundled four questions and failed (2026-10-26)
- AC2: evidenced — `opencode run` (DeepSeek V4.1 Flash) told only of a meeting, no mention of memory: it wrote three records (event, decision, action), updated handoff.md, todo.md and the session file, and committed b7be66d itself (2026-10-26)
- AC3: evidenced — `opencode run "我回來了…"` after two Claude Code sessions closed without a handoff: it stated the topic, the state, what session 02 did and the next step (2026-10-26)
- AC4: evidenced — `dsh --profile headless "我回來了"` (DeepSeek Harness 0.2.1-rc.2, DEEPSEEK_API_KEY in the environment) after a Claude Code session closed without a handoff: it stated the topic, the state, what the Claude Code session recorded and the next steps; it dated its session file with the PowerShell UK-zone form and committed at +0000. The first run, before that form was restored, stopped and asked for the date (2026-10-26)
- AC5: evidenced — `claude -p "我回來了"` after the OpenCode session: it stated topic, state, what session 03 did and the next step. Its commit was blocked by the headless permission setup, not by the template (2026-10-26)
- AC6: evidenced — OpenCode answered "billing 用哪個資料庫？為什麼？" with PostgreSQL and both reasons, naming records/2026-10-26-01-billing-db-postgresql.md and the digest; Claude Code answered the restart steps naming the runbook and its digest (2026-10-26)
- AC7: evidenced — `grep -l '^type: decision' records/*.md` listed exactly the two decisions; a loop over the five keys in every record and digest reported none missing, and reported `MISSING tags` when one record's tags line was deleted on a copy (2026-10-26)
- AC8: evidenced — oncall-runbook.md added to sources/, then `claude -p "我回來了"`: unasked, it wrote materials/generated/oncall-runbook.md, `type: digest`, `sources: [materials/sources/oncall-runbook.md]`, in English (2026-10-26)
- AC9: evidenced — `git log --stat -- materials/sources/` on the test vault after all eight sessions shows only the two commits that added the user's files (2026-10-26)

## Known Annoyances
- Headless test runs need an allowlist in the test vault's
  .claude/settings.local.json; see GOTCHAS.md.

## Recent Decisions
- 2026-10-26 user: OpenCode tests use the DeepSeek test key, not the free
  model. Run on the current template: `opencode run --standalone -m
  deepseek-test/deepseek-chat` answered "Q4 預算上限多少、為什麼" with
  41,000 USD and the Q3 overspend, naming three records, and committed at
  +0000.
- 2026-10-26: PowerShell keeps a UK-zone fallback for the date
  ([TimeZoneInfo], used when Git Bash cannot start, as in DeepSeek Harness)
  and commits with explicit GIT_AUTHOR_DATE/GIT_COMMITTER_DATE, never
  `$env:TZ`. LearningProject's template still uses `$env:TZ` in PowerShell
  and so has the summer-time-all-year commit bug; the user was told.
- 2026-10-26: Windows date and commit fixed. The template now runs both
  through `bash -c` from PowerShell, ships .claude/settings.json allowing
  exactly those commands, and forbids any fallback clock: a refused date
  command means stop and ask. Checked on a fresh copy: without trust it
  stopped and asked for the date; with `--settings` it dated the session
  file and committed with no denial (commit at +0000).
- 2026-10-26 user: digests are in English; source-to-digest is as needed;
  new sources are checked for at every start; LearningProject's UK date,
  own-commit, one-session and size-limit rules carry over; records are one
  file per entry with fixed front matter; no migrate path.
- 2026-10-26: Setup step 1 now forbids any other question in the language
  message, after the first AC1 run bundled four.

## Upstream
