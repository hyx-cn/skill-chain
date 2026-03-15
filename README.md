# SkillChain

面向 **Cursor**、**Codex**、**OpenClaw**、**OpenCode** 等技能的供应链与生态分析。扫描本地已安装技能、构建依赖图，并支持健康检查、重叠检测与报告，优先离线运行。

Supply chain intelligence and ecosystem analysis for **Cursor**, **Codex**, **OpenClaw**, and **OpenCode** skills. Scan locally installed skills, build a dependency graph, and run health checks, overlap detection, and reports—offline-first.

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

## 快速开始 | Quick start

```bash
# 一键：重置 → 扫描 → 健康检查 → 重叠分析 → 报告
# One-shot: reset, scan, health, overlaps, report
python3 scripts/ingest.py analyze-all

# 或分步执行 | Or step by step
python3 scripts/ingest.py scan
python3 scripts/analyze.py report
```

自定义扫描目录 / Override default paths:

```bash
python3 scripts/ingest.py analyze-all --dirs ~/.cursor/skills-cursor ~/.codex/skills
```

---

## 默认扫描路径 | Default scan paths

以下路径会按默认尝试，不存在的会跳过。  
*These are tried by default; missing paths are skipped.*

| 平台 Platform | 路径 Paths |
|---------------|------------|
| Cursor   | `~/.cursor/skills-cursor` |
| Codex    | `~/.codex/skills` |
| OpenClaw | `~/.openclaw/skills`, `~/.openclaw/extensions`, `/Applications/OpenClaw.app/Contents/Resources/skills` |
| OpenCode | `~/.opencode/skills` |

若存在，还会自动加入：项目内 `./skills`、以及 npm 全局目录下 `$(npm root -g)/<platform>/skills`（platform 为 openclaw、cursor、codex、opencode）。  
*Project-local `./skills` and npm global `$(npm root -g)/<platform>/skills` (openclaw, cursor, codex, opencode) are added when present.*

---

## 命令一览 | Commands

| 命令 Command | 说明 Description |
|--------------|------------------|
| `python3 scripts/ingest.py scan` | 发现技能并构建/更新图 · Discover skills and build/update graph |
| `python3 scripts/ingest.py status` | 查看图摘要 · Show graph summary |
| `python3 scripts/ingest.py reset` | 清空图 · Clear graph |
| `python3 scripts/ingest.py enrich` | 拉取 Clawhub 元数据（可选） · Fetch Clawhub metadata (optional) |
| `python3 scripts/ingest.py analyze-all` | 重置 + 扫描 + 健康 + 重叠 + 报告 · Reset + scan + health + overlaps + report |
| `python3 scripts/analyze.py stats` | 数量、分类、热门包 · Counts, categories, top packages |
| `python3 scripts/analyze.py health` | 各技能健康分 · Per-skill health scores |
| `python3 scripts/analyze.py overlaps` | 重叠或重复技能 · Overlapping or duplicate skills |
| `python3 scripts/analyze.py supply-chain --skill <slug>` | 某技能的完整供应链 · Full supply chain for a skill |
| `python3 scripts/analyze.py report` | 生成完整 Markdown 报告 · Full markdown report |

---

## 数据与存储 | Data and storage

- **输入 Input:** `SKILL.md` 前置元数据，`requirements.txt`、`pyproject.toml`、`package.json`，`scripts/*.py`（AST），`_meta.json` / `.clawhub/origin.json`。  
  *`SKILL.md` frontmatter, `requirements.txt`, `pyproject.toml`, `package.json`, `scripts/*.py` (AST), `_meta.json` / `.clawhub/origin.json`.*
- **输出 Output:** 图数据写入 `memory/skillchain/graph.jsonl`（追加式 JSONL）。该文件为本地数据，已通过 `.gitignore` 排除。  
  *Graph stored in `memory/skillchain/graph.jsonl` (append-only JSONL). This file is local and ignored via `.gitignore`.*

---

## 技能契约 | Skill contract

使用场景、触发词、工作流、本体契约与 schema 说明见 [SKILL.md](SKILL.md)。  
*See [SKILL.md](SKILL.md) for when to use this skill, triggers, workflow, ontology contract, and schema references.*

## 许可 | License

见仓库或技能元数据。  
*See repository or skill metadata.*
