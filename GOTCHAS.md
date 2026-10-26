# GOTCHAS

Version-bound traps. Every entry carries a date. Entries here may be deleted
without justification once the dependency they describe is gone.

Format:  ### <one-line trap>  `[YYYY-MM-DD]`

### Claude Code 2.1.218 on Windows runs commands through PowerShell, so Bash() allow rules do not apply  `[2026-10-26]`

Headless `claude -p` in a test vault reached for the PowerShell tool, and
`[TimeZoneInfo]::...` and `$env:TZ=...; git ...` compounds were denied even
with PowerShell() rules. Look at `permission_denials` in
`--output-format json` to see what it tried. `--dangerously-skip-permissions`
is refused by this session's auto mode; use `--permission-mode acceptEdits`
plus an allowlist in the test vault's .claude/settings.local.json, excluded
via .git/info/exclude.

### OpenCode 2.0.31's default model minimax-m3.1-flash-free is deprecated  `[2026-10-26]`

`opencode run` fails at once. Pass `-m opencode/deepseek-v4.1-flash-free`.

### Back-to-back test sessions trip the 30-minute open-session check  `[2026-10-26]`

Run `touch -d "2 hours ago" memory/sessions/*.md` in the test vault before
starting the next agent, or it stops to ask whether the last one is open.
