---
pcmp_version: "2.2"
revision: 9
updated: 2026-10-27
---

# 项目当前状态快照 (STATE.md)

- **Last Updated**: 2026-10-27
- **Overall Status**: PCMP v2.2 协议与工具链保持封板冻结。DEC-007 联动划权已由人类确认生效，适用范围限定为被管理项目，本仓不受降级影响。

## 已完成 (Completed)
- [已确认] [DEC-001] 发布 PCMP v1.0 与升级至 v2.1 协议规范手册（双语版）。
- [已确认] 建立能力边界声明、冲突分级核验准则、信息分层标签与恢复测试标准。
- [已确认] 添加 `.gitignore`，安全忽略 `node_modules/`、`package*.json`、`.claude/`、`.agents/`、`ai/.cache/` 与 Python 缓存。
- [已确认] 补充推荐工具 STC 说明及材料审阅授权准则。
- [已确认] 成功向远端 GitHub 仓库推送 `main` 分支与 `v1.0.0` / `v2.1.0` 标签。
- [已确认] 完成 PCMP 本项目自身的 `ai/` 上下文目录初始化。
- [已确认] [DEC-002] 编写并落地方案 A 的 SQLite 派生索引工具 `scripts/pcmp_index.py`。
- [已确认] [DEC-003] 确立单仓库分层架构（方案 1），更新 README 突出“单文档开箱自举”指南。
- [已确认] [DEC-004] 推出官方 PCMP Skill (`skills/pcmp/`)，提供全套斜杠命令并配置至本地环境。
- [已确认] [DEC-005] 引入 PCMP Lite 极简 4 件套与 Full 档位双轨标准，瘦身重构 `AGENTS.md`，实现初版 `--check`。
- [已确认] [DEC-006] 全量回填历史 ADR（DEC-001~004）；硬化 `--check` 强校验（脚本双副本 SHA-256 镜像比对、双语标题结构对称性、ADR 与 TASK 字段强校验）；接入 GitHub Actions CI（`.github/workflows/ci.yml`）；制作 `examples/lite/` 完整可运行开箱样板；在 `AGENTS.md` 补齐 Python 脚本编码约束。
- [已确认] 将 `examples/lite/` 纳入单测与 CI 自动化守护（`test_examples_lite_compliance`）；统一修正双语校验口径为“版本与章节骨架对齐”；正式封板 v2.2。
- [已确认] [DEC-007] PCMP × Project Wiki 联动划权：状态 / 决策 / 知识 / `AGENTS.md` 四个领域各留一个正文源，另一侧只留索引或链接。人类已确认，适用范围限定为被管理项目；借鉴清单三条仍为只攒不做。

## 进行中 (In Progress)
- [无] v2.2 功能冻结未松动，本轮只补治理边界。

## 阻塞项 (Blocked)
- 无

## 当前风险与焦点 (Current Risks & Focus)
- 划权表只在 `ai/DECISIONS.md` 写一遍，`AGENTS.md` 与状态文件只做指向，避免第二套真源。
- 借鉴清单三条不得趁 DEC-007 生效顺带上马，v2.2 解冻前不动。

## 下一步即时动作 (Immediate Next Action)
- 推送确认提交，等分支合并；随后继续挑真实业务工程跑 Lite 档位收集冷启动恢复数据。
