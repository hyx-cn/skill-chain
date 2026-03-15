# SkillChain

面向 **Cursor**、**Codex**、**OpenClaw**、**OpenCode** 等智能体的**技能**，用于分析本地技能供应链与生态。安装后直接对智能体说话即可（如「分析我的技能」「技能生态报告」），无需自己跑命令。

A **skill** for **Cursor**, **Codex**, **OpenClaw**, and **OpenCode** agents: supply chain and ecosystem analysis for locally installed skills. Install the skill, then ask your agent in natural language (e.g. “analyze my skills”, “ecosystem report”)—no need to run Python yourself.

---

## 安装 | Install

按你使用的智能体选择一种方式安装本技能。  
*Install the skill according to the agent you use.*

| 智能体 Agent | 安装方式 How to install |
|--------------|--------------------------|
| **OpenClaw（小龙虾）** | 在 [Clawhub](https://clawhub.com) 搜索 **skill-chain** 安装；或直接对小龙虾说：「**安装 skill-chain 技能**」。<br>*Search for **skill-chain** on Clawhub to install, or tell 小龙虾: “安装 skill-chain 技能”.* |
| **Cursor** | 将本仓库克隆到 Cursor 技能目录：<br>`git clone https://github.com/hyx-cn/skill-chain.git ~/.cursor/skills-cursor/skill-chain`<br>*Clone this repo into Cursor’s skills folder (see Cursor docs for the path).* |
| **Codex** | 将本仓库克隆到 Codex 技能目录：<br>`git clone https://github.com/hyx-cn/skill-chain.git ~/.codex/skills/skill-chain`<br>若使用 `CODEX_HOME`：`git clone ... $CODEX_HOME/skills/skill-chain`<br>*Clone into `~/.codex/skills/skill-chain` or `$CODEX_HOME/skills/skill-chain`.* |
| **OpenCode** | 将本仓库克隆到 OpenCode 技能目录：<br>`git clone https://github.com/hyx-cn/skill-chain.git ~/.opencode/skills/skill-chain`<br>*Clone into `~/.opencode/skills/skill-chain` (or your OpenCode skills path).* |

安装完成后，在对话里用下面「怎么用」中的说法即可触发本技能。

---

## 怎么用 | How to use

安装技能后，对智能体说下面任意一种（或类似意思），智能体会自动扫描、建图并给出分析或报告。  
*After installing, ask your agent in natural language; it will run the skill and return analysis or reports.*

- **「分析我的技能」** / “Analyze my skills”  
- **「技能库存」「技能供应链」** / “Skill inventory”, “Skill supply chain”  
- **「我的技能用了哪些包/工具」** / “What tools or packages do my skills use?”  
- **「技能分类 / 生态 breakdown」** / “Skill categories”, “Ecosystem breakdown”  
- **「技能健康度 / 有没有重叠」** / “Skill health”, “Overlapping skills”  
- **「生成技能生态报告」** / “Ecosystem report”, “Full skill report”  

更多触发词与行为对应见 [SKILL.md](SKILL.md)。

---

## 功能 | Features

- **多平台：** 默认扫描路径涵盖 Cursor、Codex、OpenClaw、OpenCode，仅扫描实际存在的目录。  
  *Multi-platform: default scan paths include Cursor, Codex, OpenClaw, OpenCode; only existing directories are scanned.*
- **依赖图：** 从 `SKILL.md`、`requirements.txt`、`pyproject.toml`、`package.json` 及脚本 import 解析技能、包、工具、分类与技能间引用。  
  *Dependency graph: skills, packages, tools, categories, and skill-to-skill references from `SKILL.md`, `requirements.txt`, `pyproject.toml`, `package.json`, and script imports.*
- **分析：** 分类分布、健康分、重叠技能、热门包、单技能供应链、完整 Markdown 报告。  
  *Analysis: category distribution, health scores, overlapping skills, top packages, per-skill supply chain, full markdown reports.*
- **可选增强：** 有网络时可拉取 Clawhub 元数据（星标、下载量、审核状态）。  
  *Optional enrichment: Clawhub metadata (stars, downloads, moderation) when network is available.*

---

## 默认扫描路径 | Default scan paths

本技能会尝试扫描以下路径（不存在的会自动跳过）。  
*These paths are tried by default; missing paths are skipped.*

| 平台 Platform | 路径 Paths |
|---------------|------------|
| Cursor   | `~/.cursor/skills-cursor` |
| Codex    | `~/.codex/skills` |
| OpenClaw | `~/.openclaw/skills`, `~/.openclaw/extensions`, `/Applications/OpenClaw.app/Contents/Resources/skills` |
| OpenCode | `~/.opencode/skills` |

若存在，还会自动加入：项目内 `./skills`、以及 npm 全局目录下 `$(npm root -g)/<platform>/skills`（platform 为 openclaw、cursor、codex、opencode）。  
*Project-local `./skills` and npm global `$(npm root -g)/<platform>/skills` (openclaw, cursor, codex, opencode) are added when present.*

---

## 高级：直接运行脚本 | Advanced: run scripts directly

如需在本地自己跑（调试或二次开发），可在仓库根目录执行：  
*To run the pipeline yourself (e.g. for debugging or development):*

```bash
# 一键：重置 → 扫描 → 健康检查 → 重叠分析 → 报告
python3 scripts/ingest.py analyze-all

# 自定义扫描目录
python3 scripts/ingest.py analyze-all --dirs ~/.cursor/skills-cursor ~/.codex/skills
```

| 命令 Command | 说明 Description |
|--------------|------------------|
| `python3 scripts/ingest.py scan` | 发现技能并构建/更新图 |
| `python3 scripts/ingest.py status` | 查看图摘要 |
| `python3 scripts/ingest.py reset` | 清空图 |
| `python3 scripts/ingest.py analyze-all` | 重置 + 扫描 + 健康 + 重叠 + 报告 |
| `python3 scripts/analyze.py report` | 生成完整 Markdown 报告 |
| `python3 scripts/analyze.py health` | 各技能健康分 |
| `python3 scripts/analyze.py overlaps` | 重叠或重复技能 |
| `python3 scripts/analyze.py supply-chain --skill <slug>` | 某技能的完整供应链 |

更多子命令见 [SKILL.md](SKILL.md)。

---

## 数据与存储 | Data and storage

- **输入 Input:** `SKILL.md` 前置元数据，`requirements.txt`、`pyproject.toml`、`package.json`，`scripts/*.py`（AST），`_meta.json` / `.clawhub/origin.json`。  
- **输出 Output:** 图数据写入 `memory/skillchain/graph.jsonl`（追加式 JSONL）。该文件为本地数据，已通过 `.gitignore` 排除。  
  *Graph stored in `memory/skillchain/graph.jsonl` (append-only JSONL). This file is local and ignored via `.gitignore`.*

---

## 技能契约 | Skill contract

使用场景、触发词、工作流、本体契约与 schema 说明见 [SKILL.md](SKILL.md)。  
*See [SKILL.md](SKILL.md) for when to use this skill, triggers, workflow, ontology contract, and schema references.*

## 许可 | License

MIT · Copyright (c) 2025 hyx-cn · See [LICENSE](LICENSE).
