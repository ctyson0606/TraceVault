<!-- Hero -->
<div align="center">
  <h1>TraceVault</h1>
  <p><b>給 AI agent 用的 vault 模板</b>：每個主題一個資料夾，裡面放你的資料，以及查到了什麼、決定了什麼、做了什麼的紀錄。全部是純文字檔，任何 agent 都能讀、搜尋和分析。</p>
  <p>
    <img alt="agents: Claude Code | OpenCode | DeepSeek Harness" src="https://img.shields.io/badge/agents-Claude%20Code%20%7C%20OpenCode%20%7C%20DeepSeek%20Harness-4d6bfe">
    <img alt="content: Markdown only" src="https://img.shields.io/badge/content-Markdown%20only-brightgreen">
  </p>
  <p>🌏 <a href="./README.zh-CN.md">简体中文</a> · 繁體中文 · <a href="./README.md">English</a></p>
</div>

<!-- Screenshot placeholder: commit the image at docs/screenshot.png, then remove this comment pair.
<p align="center"><img src="docs/screenshot.png" alt="在 VS Code 中打開的 vault，顯示紀錄和摘要" width="720"></p>
-->

你在一個主題上做的決定和查到的東西，通常只留在某個 agent 的聊天紀錄裡，其他 agent 看不到，過一個月你自己也忘了。vault 改成把這些寫進資料夾裡的檔案。agent 會邊做邊寫，不用你開口，所以你可以隨時換 agent；以後問「我們對 X 決定了什麼？」，也能得到答案和出處檔案。

## ✨ 功能

- **用對話完成設定**：複製模板、啟動 agent、回答幾個問題；你確認摘要之前，什麼都不會寫入。
- **邊做邊記**：決定、事實、做法、事件和行動項目，各自寫成一個檔案，不用你提醒。
- **隨時換 agent**：關掉 Claude Code，在同一個資料夾打開 OpenCode 或 DeepSeek Harness，它會告訴你目前做到哪裡。
- **自動整理資料**：把檔案放進 `materials/sources/`，下一個啟動的 agent 會為它寫一份簡短的英文摘要，附上頁碼或章節位置。
- **可以查**：問任何之前記過的事，答案會列出出處檔案；沒記過的就直說沒有。
- **方便分析**：每份紀錄和摘要開頭都有相同的欄位（`date`、`type`、`tags`、`summary`、`sources`），一行 `grep` 就能篩選。
- **原始檔案不會被改動**：`materials/sources/` 裡的東西永遠不會被修改。

## 🚀 安裝

需要先有：git，以及至少一個 agent：Claude Code、OpenCode 或 DeepSeek Harness。

**Option 1 · Windows (PowerShell)**

```powershell
git clone https://github.com/ctyson0606/TraceVault.git
mkdir D:\vaults\my-topic
Copy-Item -Recurse TraceVault\template\* D:\vaults\my-topic\
cd D:\vaults\my-topic
git init
```

**Option 2 · macOS 或 Linux**

<details>
<summary>同樣的步驟，用 POSIX shell</summary>

```bash
git clone https://github.com/ctyson0606/TraceVault.git
mkdir -p ~/vaults/my-topic
cp -r TraceVault/template/. ~/vaults/my-topic/
cd ~/vaults/my-topic
git init
```

</details>

新資料夾要建在這個 repo **外面**：放在裡面的話，模板的防護規則會讓 agent 忽略所有規則。手上已經有的資料，放進新資料夾的 `materials/sources/`。

**Verify**

在新資料夾裡啟動 agent，隨便說一句話，例如「你好」。它的第一個回覆只會問你想用哪種語言。

## 🕹️ 使用

### 第一次對話

1. agent 先問你的語言，其他什麼都不問。
2. 接著問主題是什麼、vault 用來做什麼、要記哪些類型的紀錄（也可以加你自己的類型，例如 `action`），以及最初的標籤清單。
3. 它會給你看一份摘要，說明它在這裡要遵守的規則。你同意後，它會把規則寫進 vault 的 `AGENTS.md`，整理 `materials/sources/` 裡已有的資料，並記下找到的內容。

### 日常使用

照平常一樣工作和對話就好。當你說了值得保留的事（「我們選 PostgreSQL 是因為……」、「Chen 會在 2026-11-10 前做 X」），它會寫一筆紀錄、更新 `memory/handoff.md` 並 commit。你永遠不用說「存檔」。

### 加入資料

把檔案放進 `materials/sources/`。下一個啟動的 agent 會先找到它、讀完，在 `materials/generated/` 寫好摘要，再做其他事。讀不了某種格式的 agent 會直接說明，留給讀得了的 agent。

### 查東西

用你自己的話問：「billing 資料庫我們決定用什麼？」、「billing API 要怎麼重啟？」。答案會列出出自哪份紀錄或摘要。

### 用 harness 分析

`records/` 和 `materials/generated/` 裡的每個檔案開頭都有相同的 front matter，一行一個欄位：

```bash
grep -l '^type: decision' records/*.md
grep -l '^tags:.*budget' records/*.md materials/generated/*.md
```

### 換到另一個 agent

關掉舊的 agent，在同一個資料夾打開新的。如果舊的還能用，先說一聲「handoff」。同一個資料夾一次只開一個 agent：如果看起來有另一個 session 還開著，新的 agent 會先問你再繼續。

### 你可以說的話

| 說 | 會發生什麼 |
| --- | --- |
| 「Handoff」 | 立刻存好所有東西，方便你切換 |
| 「我們對 X 決定了什麼？」 | 搜尋紀錄和摘要，回答並附上檔案 |
| 「我加了新檔案」 | 為 `materials/sources/` 裡還沒整理的檔案寫摘要 |

## 📁 你的 vault 裡有什麼

| 路徑 | 是什麼 | 誰寫的 |
| --- | --- | --- |
| `AGENTS.md` | 每個 agent 都要遵守的規則，結尾是這個 vault 自己的指令 | 設定時由 agent 寫；之後只在你要求時修改 |
| `CLAUDE.md` | 讓 Claude Code 去讀 `AGENTS.md` | 沒人；保持原樣 |
| `.claude/settings.json` | 讓 Claude Code 不用問就能執行英國日期指令和 `git` commit | 沒人；保持原樣 |
| `materials/sources/` | 你的原始資料 | 你；agent 只讀不改 |
| `materials/generated/` | 你的資料的英文摘要 | agent |
| `records/` | 每個決定、事實、做法、事件或行動各一個檔案 | agent，事情發生時寫 |
| `memory/handoff.md` | 目前的狀態和下一步 | agent，每次整份重寫 |
| `memory/todo.md` | 待辦 | agent |
| `memory/sessions/` | 每次對話的摘要 | agent |

## 📦 專案結構

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

- `template/AGENTS.md`：每個新 vault 一開始的規則；
- `template/CLAUDE.md`：Claude Code 讀的指標檔；
- `template/.claude/settings.json`：讓 Claude Code 不用每次都問，就能執行英國日期指令和 commit；
- `template/.gitignore`：不讓 macOS 和 Windows 的系統檔進入 git；
- `template/materials/sources/`：空的，放你的資料；
- `template/materials/generated/`：空的，放摘要；
- `template/records/`：空的，放紀錄；
- `template/memory/`：記憶檔的初始樣子。

只複製 `template/`。這個 repo 的其他部分（`kit/`、`scripts/`、最上層的 `AGENTS.md`、`SPEC.md`、`STATE.md`）是用來打造模板的工具。

## ⚠️ 相容性與限制

- 沒有資料庫引擎，也沒有搜尋索引：vault 就是一堆 Markdown 檔，用 `grep` 搜尋，由 agent 閱讀。
- 不管你用什麼語言對話，紀錄、摘要和記憶都用英文寫，讓每個 agent 和每次分析讀的都是同一種格式。
- DeepSeek Harness 最多只載入 64 KiB 的指令，所以 `AGENTS.md` 超過 60,000 bytes 時，agent 會提議把一部分移到 `memory/`。
- 不管 agent 在哪台電腦上跑，日期一律用英國日期，也不寫時間。
- 不會匯入各 agent 自己的聊天紀錄，也不會從 claude.ai Project 或其他工具搬資料進來。

## ❓ 常見問題

| 症狀 | 怎麼做 |
| --- | --- |
| agent 做到一半額度用完了 | 在同一個資料夾打開另一個 agent；它會從記憶接著做 |
| 新的 agent 問另一個 session 是不是還開著 | 關掉另一個，或告訴它已經關了 |
| Claude Code 一直要求批准 `git` 或日期指令 | Claude Code 第一次打開 vault 時，接受信任對話框：在那之前它會忽略 `.claude/settings.json` |
| agent 停下來問今天的英國日期 | 日期指令被擋了；批准它，或直接告訴它日期 |
| `opencode run` 出現 "has been deprecated" 錯誤 | 換一個現行的模型，例如 `-m opencode/deepseek-v4.1-flash-free` |
