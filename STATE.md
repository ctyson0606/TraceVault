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
2026-10-26. Every criterion except AC4 has evidence from one-shot runs on a
scratchpad vault. G3 waits on AC4 (DeepSeek Harness).

## Next Steps
1. AC4: install DeepSeek Harness (`dsh`) with DEEPSEEK_API_KEY in the
   environment, and run the AC3 scenario with it on a fresh copy of the
   template. Then write G3's How and Parts lines.
2. Run the criteria once in the interactive UIs (Claude Code in VS Code,
   OpenCode TUI): every run so far was headless (`claude -p`,
   `opencode run`), and Claude Code there used Sonnet 5.5.
3. Use the template for a first real vault; watch whether agents write
   howto records for procedures found inside sources (the on-call runbook
   test got a digest and no howto record, which the spec allows).
4. A README for this repo (kit/skills/readme.md) once the user wants one.

## Open Questions
- The 30-minute open-session check fires on back-to-back sessions, which is
  correct by the rule but is the false-positive pattern LearningProject saw.
  Left as in LearningProject until a real vault shows it matters.

## Acceptance status

- AC1: evidenced — `claude -p "你好"` (Sonnet 5.5) on a fresh copy of the template: the first reply asked only the language; then topic, types and tags; then a summary; Vault instructions written only after "確認". The first run, before the Setup step 1 fix, bundled four questions and failed (2026-10-26)
- AC2: evidenced — `opencode run` (DeepSeek V4.1 Flash) told only of a meeting, no mention of memory: it wrote three records (event, decision, action), updated handoff.md, todo.md and the session file, and committed b7be66d itself (2026-10-26)
- AC3: evidenced — `opencode run "我回來了…"` after two Claude Code sessions closed without a handoff: it stated the topic, the state, what session 02 did and the next step (2026-10-26)
- AC4: not started — `dsh` is not installed on this machine (2026-10-26)
- AC5: evidenced — `claude -p "我回來了"` after the OpenCode session: it stated topic, state, what session 03 did and the next step. Its commit was blocked by the headless permission setup, not by the template (2026-10-26)
- AC6: evidenced — OpenCode answered "billing 用哪個資料庫？為什麼？" with PostgreSQL and both reasons, naming records/2026-10-26-01-billing-db-postgresql.md and the digest; Claude Code answered the restart steps naming the runbook and its digest (2026-10-26)
- AC7: evidenced — `grep -l '^type: decision' records/*.md` listed exactly the two decisions; a loop over the five keys in every record and digest reported none missing, and reported `MISSING tags` when one record's tags line was deleted on a copy (2026-10-26)
- AC8: evidenced — oncall-runbook.md added to sources/, then `claude -p "我回來了"`: unasked, it wrote materials/generated/oncall-runbook.md, `type: digest`, `sources: [materials/sources/oncall-runbook.md]`, in English (2026-10-26)
- AC9: evidenced — `git log --stat -- materials/sources/` on the test vault after all eight sessions shows only the two commits that added the user's files (2026-10-26)

## Known Annoyances
- Headless test runs need an allowlist in the test vault's
  .claude/settings.local.json; see GOTCHAS.md.

## Recent Decisions
- 2026-10-26 user: digests are in English; source-to-digest is as needed;
  new sources are checked for at every start; LearningProject's UK date,
  own-commit, one-session and size-limit rules carry over; records are one
  file per entry with fixed front matter; no migrate path.
- 2026-10-26: Setup step 1 now forbids any other question in the language
  message, after the first AC1 run bundled four.

## Upstream
