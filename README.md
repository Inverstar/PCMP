# PCMP (Project Context Management Protocol)

[![Version](https://img.shields.io/badge/Version-2.2-blue.svg)](PROJECT_CONTEXT_ZH.md)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)
[![AI-Agnostic](https://img.shields.io/badge/AI--Agnostic-Universal-purple.svg)](PROJECT_CONTEXT.md)
[![Recommended: STC](https://img.shields.io/badge/Recommended-STC-orange.svg)](https://github.com/daimonia-ai/STC-Simplified-Technical-Chinese)

**PCMP (项目上下文管理协议 / Project Context Management Protocol)** 是一个专为人类开发者与 AI 编程智能体（AI Coding Agents）协同工作而设计的**通用工程上下文管理协议**。

它不绑定任何特定模型、IDE 或专用插件，完全基于**本地文件系统与 Markdown**运作，旨在解决复杂软件与工程交付中常见的“**上下文爆炸、会话遗忘、状态脱节、文档与代码不一致、AI 幻觉决策**”等核心痛点。

---

## 📖 核心文档入口

* 🇨🇳 **[PROJECT_CONTEXT_ZH.md](PROJECT_CONTEXT_ZH.md)**：完整中文规范手册（协议标准、Lite/Full 档位、Schema、交互指令、治理边界与自检清单）
* 🌐 **[PROJECT_CONTEXT.md](PROJECT_CONTEXT.md)**：Full English Specification Manual

---

## 🚀 极简上手：单文档开箱自举与双档位 (Quickstart)

> [!IMPORTANT]
> **PCMP 的核心灵魂是“纯 Markdown 驱动一切”，绝不强制绑定任何软件工具或外部依赖。**

为了杜绝在个人或小型项目中产生“治理通胀”，PCMP 原生支持双档位：

* **PCMP Lite（极简 4 件套）**：专为单人、快速原型与维护期项目设计，仅需 4 个文件（`AGENTS.md` + `ai/PROJECT.md` + `ai/STATE.md` 合并活动任务 + `ai/DECISIONS.md`），免建任何复杂规划或会话归档。开箱可用样例工程见 [examples/lite/](examples/lite/)；
* **PCMP Full（完整套件）**：面向团队与多阶段长期工程，提供完整的任务单（`TASK.md`）、架构图谱、会话沉淀及自动化校验。

此外提供领域定制模板，在 Lite 之上叠加行业平台约束：

* **PCMP for Olares App**：面向 Olares 应用开发（单应用单仓库、OAC 打包分发）。在 Lite 之上增加平台事实卡与 OAC 文件骨架，把官方规范、GitBot 实际拦截规则与提交踩坑经验固化为 18 条可判定红线——覆盖图标 256×256/512KB、分类白名单、第三方镜像禁 Root、Nginx 非 root 五挂载与 80 端口遮蔽、系统注入变量禁硬编码、一分支一提交等。见 [examples/olares-app/](examples/olares-app/)。

在任何新项目中使用 PCMP，只需简单三步：

1. **拷贝单文档**：将 [PROJECT_CONTEXT_ZH.md](PROJECT_CONTEXT_ZH.md)（或英文版 [PROJECT_CONTEXT.md](PROJECT_CONTEXT.md)）复制到新项目的根目录下；
2. **一条指令初始化**：向你的 AI 助手（Cursor / Claude Code / Windsurf / GitHub Copilot / Antigravity 等）发送：
   > `请读取 PROJECT_CONTEXT_ZH.md，按 Lite 档位初始化本项目的 ai/ 上下文管理体系并评估当前项目状态。`
3. **完成自举**：AI 将全自动建立极简上下文。后续每次换新会话，AI 即可依据 `ai/STATE.md` 毫秒级恢复进度并接管工作。

---

## 🏛️ 本仓库架构与分工说明 (Repository Architecture)

为了让开发者既能获得**纯粹的协议规范**，又能获得**开箱即用的可选参考工具**，本仓库采用分层自洽结构：

| 层次 | 包含内容 | 定位与说明 |
| :--- | :--- | :--- |
| **1. 核心协议规范 (Specification)** | `PROJECT_CONTEXT_ZH.md`<br>`PROJECT_CONTEXT.md` | **协议本体（核心必须）**。单文档自足，外部项目仅需拷贝此文件即可完整运作，零外部依赖。 |
| **2. 可选参考工具 (Reference Tools)** | `scripts/pcmp_index.py` | **可选派生加速层**。基于 Python 3 原生标准库实现的 SQLite 派生索引工具。当历史文档较多时按需取用，不拷贝完全不影响核心规范运转。 |
| **3. 本仓库自举范例 (Dogfooding Context)** | `ai/`<br>`AGENTS.md` | **实践范例**。展示 PCMP 规范如何管理本仓库自身的演进状态与架构决策。 |

---

## ⚡ 核心原则 (Core Principles)

1. **当前状态优先，但必读含定义 (Current State First)**  
   AI 智能体接管必读顺序 = **本协议 + `ai/PROJECT.md`（目标与硬约束） + `ai/STATE.md` + `ai/TASK.md`**。严禁仅凭状态摘要推测系统约束，严禁通读全部历史会话反推现状。
2. **冲突分级核实，不设绝对优先 (Tiered Conflict Verification)**  
   代码与测试证明的是“当前实现”，但不自动推翻用户原始需求、安全约束或已确立的 ADR 决策。发现不一致时：**标记差异 ➔ 核实依据（需求/ADR/测试） ➔ 决定修正实现还是更新文档**。
3. **最小有效上下文 (Minimal Effective Context)**  
   仅维护开发决策和推进任务必需的高价值信息；历史会话记录定期压缩摘要，防止上下文膨胀。
4. **AI 与平台无关 (AI- & Platform-Agnostic)**  
   兼容 Claude Code, Cursor, Windsurf, GitHub Copilot, Antigravity 等任意 AI 助手与开发环境。
5. **信息分层标注 (Information Layering)**  
   关键信息显式标注：`[事实]` / `[假设]` / `[提议]` / `[已确认]` / `[未决]`。AI 的建议未经人类确认不得记为已决定。
6. **协议与产品代码严格解耦 (Decoupling Protocol from Product Code)**  
   PCMP 仅为指导人机协同的工程管理规范，严禁将其作为业务逻辑或运行时依赖硬编码进产品源码。
7. **Markdown 权威，派生可全量重建 (Markdown as Authority)**  
   一切权威事实与决策均存在于 Markdown 文件中；索引与缓存（如 SQLite）均为可随时删除重建的派生工具。即使删除所有派生缓存，人和 AI 仍能完整接管项目。

---

## 📂 上下文目录标准结构 (`ai/`)

在遵循 PCMP 的项目中，AI 协同状态统一维护在工程根目录的 `ai/` 文件夹下：

```text
项目根目录/
├── PROJECT_CONTEXT.md        # 英文完整通用协议 (原版)
├── PROJECT_CONTEXT_ZH.md     # 中文完整通用协议 (原版)
├── AGENTS.md / CLAUDE.md     # 智能体通用工程入口与规则配置
├── .gitignore                # 忽略项: 包含 node_modules/ 与 ai/.cache/
└── ai/
    ├── PROJECT.md            # 项目定位、宗旨、核心目标、边界与硬约束 (长期稳定)
    ├── ARCHITECTURE.md       # 系统架构、模块设计、数据流与技术约定 (长期稳定)
    ├── STATE.md              # 项目全局当前状态快照 (动态更新, 带 revision)
    ├── TASK.md               # 当前唯一活动任务与验收指标 (动态更新, 带 revision)
    ├── PLAN.md               # 高阶路线图与里程碑规划 (动态更新)
    ├── DECISIONS.md          # 架构决策记录 (ADR 强溯源, 长期累积)
    ├── ISSUES.md             # 已知缺陷与技术风险跟踪 (动态更新)
    ├── sessions/             # 结构化会话沉淀记录 (追加模式, 定期压缩)
    ├── proposals/            # 协议改进提案 (追加模式)
    ├── .cache/               # (可选) 派生索引与缓存目录 (git 忽略)
    └── PROJECT_CONTEXT.md / PROJECT_CONTEXT_ZH.md   # (可选) 项目定制使用版
```

---

## 🔄 核心工作流程

```text
1. 开发者发出指令 ──► 2. AI 确定生效协议 ──► 3. 查阅 PROJECT.md & 状态任务快照
                                                                │
                                                                ▼
6. 沉淀会话与更新状态 ◄── 5. 代码实现与测试验证 ◄── 4. 检索 DECISIONS.md 推进任务
```

### 常用人类交互提示词 (Standard Prompts)

在任何 AI 智能体对话框中，可直接使用以下标准化指令：

* **项目初始化**：
  > `请读取 PROJECT_CONTEXT_ZH.md，初始化本项目的 ai/ 上下文管理体系并评估当前项目状态。`
* **跨会话 / 换新 AI 接管**：
  > `请读取 PROJECT_CONTEXT_ZH.md 接管项目，按必读顺序查阅 ai/PROJECT.md、ai/STATE.md 和 ai/TASK.md，列出未决问题后继续推进当前任务。`
* **只读状态与一致性诊断**：
  > `请读取 PROJECT_CONTEXT_ZH.md，评估项目当前状态与架构一致性，分析代码与文档是否存在脱节。不要修改代码。`
* **方案规划与 ADR 决策提议**：
  > `请读取 PROJECT_CONTEXT_ZH.md，结合 ai/PLAN.md 提出下一步开发计划或架构方案建议。不要修改代码。`
* **会话收尾与沉淀**：
  > `请按照 PCMP 协议规范收尾当前会话：验证产物，在 ai/sessions/ 沉淀会话记录，更新 ai/STATE.md 和 ai/TASK.md，新决定登记到 ai/DECISIONS.md（未确认标 Proposed）。`

---

## 🛠️ 配套与推荐工具 (Ecosystem & Tools)

### 1. 官方智能体技能：PCMP Skill (`skills/pcmp/`)

为各大 AI 编码助手（Claude Code、Antigravity、Cursor 等）提供开箱即用的**快捷斜杠命令**，无需再手动复制冗长的标准提示词：

| 快捷命令 | 对应工作流 | 自动化执行内容 |
| :--- | :--- | :--- |
| `/pcmp init [lite\|full]` | 上下文体系初始化 | 检查项目现状，按 Lite（4 件套）或 Full（全功能）建立 `ai/` 规范目录。 |
| `/pcmp takeover` | 跨会话 / 换新 AI 接管 | 严格按规范阅读 `PROJECT.md` ➔ `STATE.md` ➔ `TASK.md` 并向开发者汇报当前焦点。 |
| `/pcmp check` | 自动化合规健全性校验 | 自动校验 Frontmatter、ADR 字段完整性、双语版本与章节骨架对齐、脚本副本一致性。 |
| `/pcmp finalize` | 会话收尾与沉淀 | 运行测试与合规检查、审查 diff、沉淀会话、递增 revision、登记 ADR 并刷新索引。 |
| `/pcmp adr <标题>` | 登记架构决策 | 按标准模板向 `ai/DECISIONS.md` 追加决策（带 Supersedes 与复审触发条件）。 |
| `/pcmp assess` | 状态一致性诊断 | 只读分析源代码物理实现与架构文档脱节情况，输出诊断报告（不改代码）。 |
| `/pcmp index` | 派生索引查询 | 结构化查询决策状态、检索分层标签或通过 FTS5 进行全文搜索。 |

**引入方式**：
将本仓库的 `skills/pcmp/` 目录复制到目标项目的 `.claude/skills/pcmp`（适用于 Claude Code）或 `.agents/skills/pcmp`（适用于 Antigravity）即可自动生效。

### 2. 官方可选参考工具：SQLite 派生索引与校验脚本 (`pcmp_index.py`)

当项目长期演进、历史会话与决策较多时，可按需使用本仓库提供的单脚本工具（基于 Python 3 原生标准库，**零外部 pip 依赖**）：

```bash
# 可选：下载单脚本到项目中（不下载完全不影响 PCMP 核心规范运作）
curl -fsSL https://raw.githubusercontent.com/AIDockerCloud/PCMP/main/scripts/pcmp_index.py -o scripts/pcmp_index.py

# 自动化合规检查 (强校验 Frontmatter、revision、ADR 完整性、双语版本与章节骨架对齐、脚本副本一致性)
python3 scripts/pcmp_index.py --check

# 运行自动化测试套件
python3 -m unittest discover tests

# 全量构建或增量更新派生缓存 (存放在 ai/.cache/index.sqlite，已被 .gitignore 忽略)
python3 scripts/pcmp_index.py --rebuild
python3 scripts/pcmp_index.py --update

# 结构化过滤与全文检索
python3 scripts/pcmp_index.py --query-decisions --status-filter Proposed
python3 scripts/pcmp_index.py --query-tags --tag-filter proposal
python3 scripts/pcmp_index.py --search "关键词"
```

### 3. 推荐搭配规范：STC (Simplified Technical Chinese)

PCMP 推荐搭配 [STC (简体技术中文规范)](https://github.com/daimonia-ai/STC-Simplified-Technical-Chinese) 作为中文工程文档与人机对话的规范工具。

在项目中按需安装并初始化 STC 规则：
```bash
# 1. 安装 STC 命令行与规则工具
npm install -D @daimonia/stc@latest

# 2. 初始化项目规则（确认后写入默认规则到 AGENTS.md）
npx stc init --defaults

# 3. 命令行检查中文文本规范
npx stc check <文件路径>
```

- **审阅授权准则（PCMP 推荐项）**：**审阅、审计或样式检查的材料由人类指定或显式授权挑选**；未获用户明确许可前，AI 不得擅自扫描全工程扩大审阅范围。
- **许可证说明**：STC 写作规则采用 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)，工具代码采用 [MIT License](https://opensource.org/licenses/MIT)，版权所有 (c) 2026 Daimonia (杭州代梦智能科技有限公司)。

---

## 🏷️ 版本与发布

* **当前版本**：V2.2
* **更新内容**：
  - 推出 **PCMP Lite**（极简 4 件套）与 **PCMP Full**（全功能套件）双档位规范，杜绝小项目“治理通胀”；
  - `AGENTS.md` 瘦身治理：精简中文写作约束，增加 15 行工程硬事实、常用命令与红线禁区；
  - 强化 `TASK.md` 结构：新增必选/强推荐 `Constraints & Non-Goals` 与 `Relevant Files`；
  - 强化架构决策记录（ADR）：新增 `Supersedes`、`Superseded-by` 与 `复审触发条件 (Review Trigger)` 链路字段；
  - 引入机器自动化合规校验命令 `python3 scripts/pcmp_index.py --check` 与配套自动化测试套件；
  - 升级官方 PCMP Skill，新增 `/pcmp check` 指令与分档初始化工作流。
