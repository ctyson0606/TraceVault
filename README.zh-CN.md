<!-- Hero -->
<div align="center">
  <h1>TraceVault</h1>
  <p><b>给 AI agent 用的 vault 模板</b>：每个主题一个文件夹，里面放你的资料，以及查到了什么、决定了什么、做了什么的记录。全部是纯文本文件，任何 agent 都能读、搜索和分析。</p>
  <p>
    <img alt="agents: Claude Code | OpenCode | DeepSeek Harness" src="https://img.shields.io/badge/agents-Claude%20Code%20%7C%20OpenCode%20%7C%20DeepSeek%20Harness-4d6bfe">
    <img alt="content: Markdown only" src="https://img.shields.io/badge/content-Markdown%20only-brightgreen">
  </p>
  <p>🌏 简体中文 · <a href="./README.zh-TW.md">繁體中文</a> · <a href="./README.md">English</a></p>
</div>

<!-- Screenshot placeholder: commit the image at docs/screenshot.png, then remove this comment pair.
<p align="center"><img src="docs/screenshot.png" alt="在 VS Code 中打开的 vault，显示记录和摘要" width="720"></p>
-->

你在一个主题上做的决定和查到的东西，通常只留在某个 agent 的聊天记录里，其他 agent 看不到，过一个月你自己也忘了。vault 改成把这些写进文件夹里的文件。agent 会边做边写，不用你开口，所以你可以随时换 agent；以后问「我们对 X 决定了什么？」，也能得到答案和出处文件。

## ✨ 功能

- **用对话完成设置**：复制模板、启动 agent、回答几个问题；你确认摘要之前，什么都不会写入。
- **边做边记**：决定、事实、做法、事件和行动项，各自写成一个文件，不用你提醒。
- **随时换 agent**：关掉 Claude Code，在同一个文件夹打开 OpenCode 或 DeepSeek Harness，它会告诉你目前做到哪里。
- **自动整理资料**：把文件放进 `materials/sources/`，下一个启动的 agent 会为它写一份简短的英文摘要，附上页码或章节位置。
- **可以查**：问任何之前记过的事，答案会列出出处文件；没记过的就直说没有。
- **方便分析**：每份记录和摘要开头都有相同的字段（`date`、`type`、`tags`、`summary`、`sources`），一行 `grep` 就能筛选。
- **原始文件不会被改动**：`materials/sources/` 里的东西永远不会被修改。

## 🚀 安装

需要先有：git，以及至少一个 agent：Claude Code、OpenCode 或 DeepSeek Harness。

**Option 1 · Windows (PowerShell)**

在 `TraceVault` 所在的文件夹里运行：

```powershell
mkdir D:\vaults\my-topic
Copy-Item -Recurse TraceVault\template\* D:\vaults\my-topic\
cd D:\vaults\my-topic
git init
```

**Option 2 · macOS 或 Linux**

<details>
<summary>同样的步骤，用 POSIX shell</summary>

```bash
mkdir -p ~/vaults/my-topic
cp -r TraceVault/template/. ~/vaults/my-topic/
cd ~/vaults/my-topic
git init
```

</details>

新文件夹要建在这个 repo **外面**：放在里面的话，模板的防护规则会让 agent 忽略所有规则。手上已经有的资料，放进新文件夹的 `materials/sources/`。

**Verify**

在新文件夹里启动 agent，随便说一句话，例如「你好」。它的第一个回复只会问你想用哪种语言。

## 🕹️ 使用

### 第一次对话

1. agent 先问你的语言，其他什么都不问。
2. 接着问主题是什么、vault 用来做什么、要记哪些类型的记录（也可以加你自己的类型，例如 `action`），以及最初的标签列表。
3. 它会给你看一份摘要，说明它在这里要遵守的规则。你同意后，它会把规则写进 vault 的 `AGENTS.md`，整理 `materials/sources/` 里已有的资料，并记下找到的内容。

### 日常使用

照平常一样工作和对话就好。当你说了值得保留的事（「我们选 PostgreSQL 是因为……」、「Chen 会在 2026-11-10 前做 X」），它会写一条记录、更新 `memory/handoff.md` 并 commit。你永远不用说「保存」。

### 加入资料

把文件放进 `materials/sources/`。下一个启动的 agent 会先找到它、读完，在 `materials/generated/` 写好摘要，再做其他事。读不了某种格式的 agent 会直接说明，留给读得了的 agent。

### 查东西

用你自己的话问：「billing 数据库我们决定用什么？」、「billing API 要怎么重启？」。答案会列出出自哪份记录或摘要。

### 用 harness 分析

`records/` 和 `materials/generated/` 里的每个文件开头都有相同的 front matter，一行一个字段：

```bash
grep -l '^type: decision' records/*.md
grep -l '^tags:.*budget' records/*.md materials/generated/*.md
```

### 换到另一个 agent

关掉旧的 agent，在同一个文件夹打开新的。如果旧的还能用，先说一声「handoff」。同一个文件夹一次只开一个 agent：如果看起来有另一个 session 还开着，新的 agent 会先问你再继续。

### 你可以说的话

| 说 | 会发生什么 |
| --- | --- |
| 「Handoff」 | 立刻保存所有东西，方便你切换 |
| 「我们对 X 决定了什么？」 | 搜索记录和摘要，回答并附上文件 |
| 「我加了新文件」 | 为 `materials/sources/` 里还没整理的文件写摘要 |

## 📁 你的 vault 里有什么

| 路径 | 是什么 | 谁写的 |
| --- | --- | --- |
| `AGENTS.md` | 每个 agent 都要遵守的规则，结尾是这个 vault 自己的指令 | 设置时由 agent 写；之后只在你要求时修改 |
| `CLAUDE.md` | 让 Claude Code 去读 `AGENTS.md` | 没人；保持原样 |
| `materials/sources/` | 你的原始资料 | 你；agent 只读不改 |
| `materials/generated/` | 你的资料的英文摘要 | agent |
| `records/` | 每个决定、事实、做法、事件或行动各一个文件 | agent，事情发生时写 |
| `memory/handoff.md` | 目前的状态和下一步 | agent，每次整份重写 |
| `memory/todo.md` | 待办 | agent |
| `memory/sessions/` | 每次对话的摘要 | agent |

## 📦 项目结构

```text
template/AGENTS.md
template/CLAUDE.md
template/.gitignore
template/materials/sources/
template/materials/generated/
template/records/
template/memory/
```

- `template/AGENTS.md`：每个新 vault 一开始的规则；
- `template/CLAUDE.md`：Claude Code 读的指针文件；
- `template/.gitignore`：不让 macOS 和 Windows 的系统文件进入 git；
- `template/materials/sources/`：空的，放你的资料；
- `template/materials/generated/`：空的，放摘要；
- `template/records/`：空的，放记录；
- `template/memory/`：记忆文件的初始样子。

只复制 `template/`。这个 repo 的其他部分（`kit/`、`scripts/`、最上层的 `AGENTS.md`、`SPEC.md`、`STATE.md`）是用来打造模板的工具。

## ⚠️ 兼容性与限制

- 没有数据库引擎，也没有搜索索引：vault 就是一堆 Markdown 文件，用 `grep` 搜索，由 agent 阅读。
- 不管你用什么语言对话，记录、摘要和记忆都用英文写，让每个 agent 和每次分析读的都是同一种格式。
- DeepSeek Harness 还没用这个模板测试过；Claude Code 和 OpenCode 测过了。
- DeepSeek Harness 最多只加载 64 KiB 的指令，所以 `AGENTS.md` 超过 60,000 bytes 时，agent 会提议把一部分移到 `memory/`。
- 不管 agent 在哪台电脑上跑，日期一律用英国日期，也不写时间。
- 不会导入各 agent 自己的聊天记录，也不会从 claude.ai Project 或其他工具搬资料进来。

## ❓ 常见问题

| 症状 | 怎么做 |
| --- | --- |
| agent 做到一半额度用完了 | 在同一个文件夹打开另一个 agent；它会从记忆接着做 |
| 新的 agent 问另一个 session 是不是还开着 | 关掉另一个，或告诉它已经关了 |
| Windows 上的 Claude Code 一直要求批准 `git` 或日期命令 | 批准就好；它每写一条记录就 commit，每次都要读英国日期 |
| `opencode run` 出现 "has been deprecated" 错误 | 换一个现行的模型，例如 `-m opencode/deepseek-v4.1-flash-free` |
