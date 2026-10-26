---
pcmp_version: "2.2"
revision: 6
updated: 2026-10-27
---

# 架构与技术决策记录 (DECISIONS.md)

## DEC-001 — PCMP 协议规范确立与开源发布
日期: 2026-10-19 | 状态: Accepted
- 确认来源: 用户在会话中授权确认发布 V1.0 与 V2.1
- Supersedes: 无
- Superseded-by: 无
- 复审触发条件 (Review Trigger): 当协议核心架构发生重大范式变更或决定拆分独立多生态体系时
- 背景 (Context): AI 辅助编程环境下，普遍存在会话易中断、上下文失忆、无法长期追溯设计意图等工程痛点。
- 决策 (Decision): 确立并发布 PCMP（项目上下文管理协议），以中英双语原版规范形式开源发布，核心状态维护于 `ai/` 目录。
- 理由 (Reason): Markdown + 文件系统具备极致的通用性、透明性与平台无关性，不依赖任何特定模型或专有 IDE。
- 备选方案 (Alternatives): 专有 IDE 插件配置、闭源 SaaS 平台、单一大提示词模板。放弃原因：缺乏通用性与版本控制友好度。
- 影响 (Consequences): 为项目提供了标准化的上下文沉淀与接管规范；需要按规范维护 `STATE.md` 与 `TASK.md`。

---

## DEC-002 — 采用 Python 3 原生标准库实现 SQLite 派生索引层 (方案 A)
日期: 2026-10-19 | 状态: Accepted
- 确认来源: 用户明确指示“按方案A去实现”
- Supersedes: 无
- Superseded-by: 无
- 复审触发条件 (Review Trigger): 当生态需要支持跨语言编译型单二进制或 Python 标准库无法满足检索性能需求时
- 背景 (Context): 随着项目规模和会话增多，从纯 Markdown 文件中进行结构化筛选（如过滤所有 Proposed 决策、查找特定标签、全文检索）效率较低，需要一个毫秒级的本地加速索引层。
- 决策 (Decision): 采用方案 A，使用 Python 3 原生标准库（`sqlite3` 包含 FTS5、`hashlib`、`pathlib`）实现单文件轻量 CLI 工具 `scripts/pcmp_index.py`。
- 理由 (Reason): Python 3 在现代开发环境中近乎 100% 预装；其标准库开箱即用，无需执行 `pip install` 安装任何外部依赖，极大降低了用户和智能体的使用门槛。
- 备选方案 (Alternatives):
  - 方案 B (Node.js)：需要依赖 `better-sqlite3` 或 C++ 编译，在多系统环境下易报构建错误。
  - 方案 C (Go/Rust 单文件编译)：执行性能极高，但在当前原型阶段跨平台交叉编译和分发维护成本偏高。
- 影响 (Consequences): 生成的索引缓存固定存放于 `ai/.cache/index.sqlite`（已被 `.gitignore` 忽略）；索引可随时全量删除重建，不改变 Markdown 的唯一权威地位。

---

## DEC-003 — 确立单仓库分层架构（方案 1）与单文档开箱自举定位
日期: 2026-10-19 | 状态: Accepted
- 确认来源: 用户明确指示“使用方案1，如果后续项目变大或者需要考虑和兼容的工具变多时再考虑方案2”
- Supersedes: 无
- Superseded-by: 无
- 复审触发条件 (Review Trigger): 当工具矩阵扩大、多语言客户端涌现且需要独立发版治理时复审分库方案
- 背景 (Context): 引入 `scripts/pcmp_index.py` 与 `ai/` 自举目录后，需要明确 PCMP 是否依然坚持“单文档初始化”的核心初心，避免外部使用者误以为使用 PCMP 必须引入一整套脚本或环境。
- 决策 (Decision): 采纳方案 1，坚持“单文档驱动一切”的协议本质。在单仓库内建立明确的三层划分：① 核心协议规范（单文档自足，外部只需复制）；② 官方可选工具（scripts 参考实现，按需自取）；③ 本仓库自举上下文（ai/ 目录范例）。后续待生态扩大、工具矩阵增加时再独立分立 `pcmp-cli` 仓库（方案 2）。
- 理由 (Reason): 保持当前仓库的轻量与自洽，消除外部使用者的概念困惑，同时避免过早拆分多仓库带来的维护与发版成本。
- 备选方案 (Alternatives): 立即拆分出独立的 `pcmp-cli` 仓库（方案 2）。延后理由：当前处于协议演进初期，单脚本参考实现已能满足需求，过早分库增加维护负担。
- 影响 (Consequences): README.md 明确突出“单文档开箱自举”指南，清晰界定脚本为非必需的可选加速工具。

---

## DEC-004 — 推出官方 PCMP Skill 智能体技能标准
日期: 2026-10-20 | 状态: Accepted
- 确认来源: 用户指示“可以做成skill吗？这样可以把指令做成命令来用；好，建议动作也一起做”
- Supersedes: 无
- Superseded-by: 无
- 复审触发条件 (Review Trigger): 当主流智能体生态标准（如 MCP、统一 Skill 规范）发生重大标准重构时
- 背景 (Context): 人类开发者在不同对话中频繁手动复制长篇提示词（初始化、接管、收尾、ADR登记等）操作繁琐；主流智能体环境（Claude Code、Antigravity 等）支持 Skill 规范。
- 决策 (Decision): 推出官方 PCMP Skill，在 `skills/pcmp/` 中建立通用技能定义 `SKILL.md` 并内嵌索引脚本，提供 `/pcmp init`、`/pcmp takeover`、`/pcmp finalize`、`/pcmp adr`、`/pcmp assess`、`/pcmp index` 等斜杠命令；并部署至本地 `.claude/skills/pcmp` 与 `.agents/skills/pcmp`。
- 理由 (Reason): 将协议核心工作流封装为自动化动作，极大提升开发者日常操作效率，降低协议执行门槛，同时完全兼容原有的纯单文档使用方式。
- 备选方案 (Alternatives): 仅提供纯文本 Prompt 模板。放弃原因：长 Prompt 维护成本高，易遗漏收尾或更新步骤。
- 影响 (Consequences): 仓库增加 `skills/pcmp/` 目录；`AGENTS.md` 引用该 Skill 指令；README.md 增补 Skill 命令对照表。

---

## DEC-005 — 引入 Lite 档位、强校验机制与 AGENTS.md 瘦身治理
日期: 2026-10-20 | 状态: Accepted
- 确认来源: 用户明确指示“可以请立刻执行”确认 5 项治理建议
- Supersedes: 无
- Superseded-by: 无
- 复审触发条件 (Review Trigger): 当 Lite 档位在实践中出现无法满足的状态管理瓶颈，或衍生出更多自动化校验规则时复审
- 背景 (Context): PCMP 机制在应对个人或小型项目时显现出“治理通胀”倾向；原 AGENTS.md 写作规则占比过大而缺乏关键工程硬事实与边界；缺乏机器级合规与双语一致性自动化校验。
- 决策 (Decision):
  1. 确立 PCMP 双档位：Lite（极简 4 件套，活动任务并入 STATE）与 Full（完整治理套件）；
  2. 重构瘦身 AGENTS.md，精简写作规则并注入 15 行高密度工程硬事实、常用命令与红线禁区；
  3. 强化 TASK.md 必须包含 Constraints & Non-Goals 与 Relevant Files；
  4. 强化 ADR 结构，引入 Supersedes、Superseded-by 与复审触发条件；
  5. 在 scripts/pcmp_index.py 中增加 --check 自动化合规检查并配套完整单元测试。
- 理由 (Reason): 保持 PCMP 纯 Markdown 权威初心的同时，提供适应小项目的极低负担落地路径，并通过机器检验确保协议合规与双语一致。
- 备选方案 (Alternatives): 继续保留单一套件要求所有项目写满 7 个文档与会话记录。放弃原因：加剧使用者的抵抗心理与治理通胀。
- 影响 (Consequences): 协议升级至 v2.2；测试与合规检查纳入收尾标准动作；提升智能体与开发者的协同体验。

---

## DEC-006 — 历史 ADR 字段补齐、--check 强硬化、CI 闭环与 Lite 样板建立
日期: 2026-10-20 | 状态: Accepted
- 确认来源: 用户明确指示“请完成”执行 Alice 二次评价的 5 项深度治理建议
- Supersedes: 无
- Superseded-by: 无
- 复审触发条件 (Review Trigger): 当 CI 校验规则需支持更多语言环境，或 Lite 样板需要扩展针对特定框架的模板时复审
- 背景 (Context): 协议 v2.2 确立了新规范，但历史 ADR 未回填新字段；--check 缺乏对脚本副本一致性与双语标题对称性的硬约束；缺少 GitHub Actions 导致质量靠自觉；Lite 缺乏直接可用的开箱样例工程。
- 决策 (Decision):
  1. 全量回填 DEC-001 至 DEC-004 的 Supersedes、Superseded-by 与复审触发条件；
  2. 强化 scripts/pcmp_index.py --check：增加脚本双副本 SHA-256 一致性校验、双语二级标题结构对称性校验，并将 ADR 字段完整性与 TASK 关键小节缺失直接设为 error；
  3. 接入 GitHub Actions CI 工作流（.github/workflows/ci.yml）；
  4. 创建 examples/lite/ 极简 4 件套可运行样板工程；
  5. 在 AGENTS.md 中补齐 Python 脚本编码约束（零 pip 依赖、PEP 8、单文件自足）。
- 理由 (Reason): 将治理从“框架设计完成”推进到“深度自动化落地与开箱体验闭环”，消除荣誉制依赖与重复真源漂移风险。
- 备选方案 (Alternatives): 保持 warning 提示与手动跑测试。放弃原因：容易遗忘，无法建立机器级别的硬质防线。
- 影响 (Consequences): 每次 push/PR 均强制通过完整测试与合规检查；外部使用者可直接复制 examples/lite/ 立即开箱。

---

## DEC-007 — PCMP × Project Wiki 联动划权
日期: 2026-10-27 | 状态: Proposed
- 确认来源: 待人类确认。草案由人类起草，Proposed 期间不生效
- Supersedes: 无
- Superseded-by: 无
- 复审触发条件 (Review Trigger): 人类确认转 Accepted 时；v2.2 解冻并评估借鉴清单时；被管理项目同时安装两者且出现真源冲突时
- 适用范围 (Scope): 同时安装 PCMP 与 Project Wiki 的被管理项目。PCMP 本仓库当前无 `.project-wiki/`，本仓 `DECISIONS.md` 仍是决策正文，降为索引只适用于被管理项目。
- 背景 (Context): 两个系统都写 `AGENTS.md`，都记状态与决策。同项目共存时同一事实会被写两遍，漂移后没有哪一侧负责兜底。
- 决策 (Decision): 按领域划权，每个领域只留一个正文源，另一侧只留索引或链接。

| 领域 | 唯一真源 | 另一侧的角色 |
| :--- | :--- | :--- |
| 状态 | PCMP `ai/STATE.md` + `ai/TASK.md` | Project Wiki `STATUS.md` 只留一行链接，不写正文 |
| 决策 | Project Wiki `changes/decisions/ADR-*`，可挂需求与代码路径 | PCMP `DECISIONS.md` 降为索引：只保留编号、标题、确认来源（批准人）、指向 ADR 的链接 |
| 知识 | Project Wiki 独占需求、技术文档、traceability | PCMP 不碰。会话沉淀与接管/收尾流程归 PCMP；Project Wiki `logs/` 只记知识变更审计，粒度不同可并存 |
| `AGENTS.md` | 两侧各自所有 | Project Wiki 用带标记区块替换安装，与 PCMP 内容互不干扰 |

- 借鉴清单 (Backlog，只攒不做):
  1. 机器契约单一定义 + CI 查漂移，对应 Project Wiki 的 `schema/project-wiki.yml` 与 `check_contracts.py`；
  2. `DECISIONS` / `sessions` 渐进披露索引；
  3. intake 的「证据≠真理」。
  三条不进 v2.2，等 Lite 实测数据齐、v2.2 解冻后再评估立项。
- 理由 (Reason): 两者长处不重叠。PCMP 强在状态与接管流程的极低负担，Project Wiki 强在有 ID、有 traceability 的知识库。划权后各做擅长的事，不必合并，也不必二选一。
- 备选方案 (Alternatives):
  - 只留一个系统：放弃 PCMP 的轻量接管，或放弃 Project Wiki 的 traceability，两边都是净损失。
  - 同一事实双向同步：需要持续对账，且漂移无法机器发现。
- 影响 (Consequences): 被管理项目中 PCMP `DECISIONS.md` 由正文降为索引，`--check` 对 Accepted 决策的确认来源校验照旧适用于索引行；人类确认前本决策不产生任何强制动作。
