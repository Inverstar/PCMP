---
pcmp_version: "2.2"
revision: 9
updated: 2026-10-27
---

# 当前唯一活动任务 (TASK.md)

## 当前任务与核心目标 (Current Task & Objective)
- **任务名称**：PCMP × Project Wiki 联动划权立项与确认（DEC-007，Accepted）。
- **核心目标**：把两个系统的真源边界写成可核验的划权表并落库；人类确认后转 Accepted，收尾推送。

## 背景与范围 (Why & Scope)
- **背景**：同项目同时安装 PCMP 与 Project Wiki 时，`AGENTS.md`、状态、决策三处都可能双写，漂移后没有哪一侧负责兜底。
- **范围**：`ai/DECISIONS.md`（DEC-007 正文）、`AGENTS.md`（联动边界三条）、`ai/STATE.md`、`ai/TASK.md`、`ai/sessions/SESSION-2026-10-27-001.md` 与 `-002.md`。不改协议原版，不改脚本。

## 约束与非目标 (Constraints & Non-Goals)
- **硬约束**：DEC-007 的降级只作用于同时安装两者的被管理项目；PCMP 本仓无 `.project-wiki/`，`DECISIONS.md` 仍是决策正文。v2.2 协议功能继续冻结。
- **非目标**：借鉴清单三条（机器契约查漂移、渐进披露索引、intake 证据≠真理）只登记不实现；不新建 `.project-wiki/` 骨架；不动 `PROJECT_CONTEXT*.md`。

## 关联关键文件 (Relevant Files)
- `ai/DECISIONS.md`：DEC-007 划权表正文，唯一正文源
- `AGENTS.md`：联动边界与 `PROJECT-WIKI` 标记区块归属
- `ai/STATE.md`：状态快照
- `ai/sessions/SESSION-2026-10-27-001.md`、`-002.md`：起草与确认两次会话沉淀

## 验收结果 (Acceptance Result)
- [x] DEC-007 以 Proposed 落库，划权表四行齐全，适用范围与 PCMP 自身仓库区分清楚；
- [x] `AGENTS.md` 补联动边界三条，只做指向不复述表格；
- [x] 单元测试 9/9 通过，`--check` 0 错误 0 警告；
- [x] 人类确认 DEC-007，状态转 Accepted，确认来源指向 `SESSION-2026-10-27-002.md`。

## 即时动作 (Immediate Next Action)
- 推送确认提交，等分支合并；随后切回 Lite 档位实测。
