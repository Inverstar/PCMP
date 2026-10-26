<!-- stc:start -->
<!-- stc:mode=default -->
## 中文写作：STC（Simplified Technical Chinese）

写中文时必须遵循 STC 规范：自然、简短、准确，任务回执必须分为“做完了”“需要你”“知会”。完整规则见 `.claude/skills/stc/SKILL.md`。
<!-- stc:end -->

## 项目宗旨与定位
PCMP (Project Context Management Protocol) 是面向人机协同的通用工程上下文管理协议。核心理念为：**纯 Markdown 权威驱动、单文档开箱自举、派生索引可随意删除重建**。

## 关键路径索引
- 核心协议原版：[PROJECT_CONTEXT_ZH.md](PROJECT_CONTEXT_ZH.md) / [PROJECT_CONTEXT.md](PROJECT_CONTEXT.md)
- 项目状态真源：[ai/PROJECT.md](ai/PROJECT.md)（硬约束）、[ai/STATE.md](ai/STATE.md)（状态）、[ai/TASK.md](ai/TASK.md)（活动任务）、[ai/DECISIONS.md](ai/DECISIONS.md)（ADR）
- 派生工具与技能：[scripts/pcmp_index.py](scripts/pcmp_index.py)、[skills/pcmp/SKILL.md](skills/pcmp/SKILL.md)

## 常用工程命令
- **自动化合规检查**：`python3 scripts/pcmp_index.py --check`
- **运行单元测试套件**：`python3 -m unittest discover tests`
- **增量刷新派生索引**：`python3 scripts/pcmp_index.py --update`
- **全量重建派生索引**：`python3 scripts/pcmp_index.py --rebuild`
- **检索决策与标签**：`python3 scripts/pcmp_index.py --query-decisions` / `--query-tags`

## 脚本编码规范
- **Python 派生工具约定**：仅依赖 Python 3 原生标准库，零 pip 外部依赖；保持单文件自足；严格遵循 PEP 8 规范。

## 联动边界（[提议] DEC-007，未生效）
- 与 Project Wiki 同项目共存时按领域划权：状态归本仓 `ai/STATE.md` + `ai/TASK.md`，决策正文归 `.project-wiki/changes/decisions/ADR-*`，本仓 `DECISIONS.md` 只留索引。划权表见 `ai/DECISIONS.md` DEC-007，不在别处复述。
- `AGENTS.md` 内 `PROJECT-WIKI` 标记区块由 project-wiki skill 安装与替换，不得手改；标记外内容归本仓。
- 借鉴清单（机器契约查漂移、渐进披露索引、intake 证据≠真理）只攒不做，v2.2 解冻后再评估。

## 红线与禁区 (Hard Boundaries)
1. **协议原版保护**：严禁未经人类授权直接修改根目录通用协议原版（变更须遵循优化版与补丁回流流程）；
2. **决策链条完整**：严禁隐式推翻已 Accepted 决策；新方案必须记录 `Supersedes` 并附带确认来源；
3. **协议产品解耦**：PCMP 仅为协同管理协议，严禁作为业务代码逻辑或运行依赖硬编码进产品源码；
4. **材料审阅授权**：审阅、审计或样式检查的材料由人类指定或显式授权挑选，未获许可前不得擅自扫描全工程扩大审阅范围。
