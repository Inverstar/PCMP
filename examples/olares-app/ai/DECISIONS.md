---
pcmp_version: "2.2"
revision: 2
updated: 2026-10-25
---

# 架构与技术决策记录 (DECISIONS.md - Lite)

> 以下为格式示例，确认来源需替换为真实记录。Olares 平台相关决策一律登记在此，便于后续复审。

## DEC-001 — 应用入口鉴权级别定为 internal
日期: 2026-10-25 | 状态: Accepted
- 确认来源: 项目维护者确认（示例，请替换为真实来源与会话依据）
- Supersedes: 无
- Superseded-by: 无
- 复审触发条件 (Review Trigger): 应用新增对外分享或多人协作能力时复审
- 背景 (Context): Olares 入口支持 public / private / internal 三档，默认 private。本应用为个人数据管理类工具，仅本人使用，无跨设备外网直连需求。
- 决策 (Decision): 主入口 `authLevel` 采用 `internal`，不采用 `public`。
- 理由 (Reason): `internal` 在内网访问免鉴权，兼顾便利与安全；`public` 意味着互联网任意访问且无鉴权，风险不可接受。
- 备选方案 (Alternatives): `private`（内外网均需授权，体验偏重）；`public`（安全边界不可接受）。
- 影响 (Consequences): 用户离开内网需经 Olares 鉴权代理访问；若未来要对外分享，需改档并重新评估暴露面。

## DEC-002 — 存储权限仅申请 appData、appCache 与 Home/Documents
日期: 2026-10-25 | 状态: Accepted
- 确认来源: 项目维护者确认（示例，请替换为真实来源与会话依据）
- Supersedes: 无
- Superseded-by: 无
- 复审触发条件 (Review Trigger): 应用新增对相册、下载目录等其它用户目录的读写需求时复审
- 背景 (Context): 应用需持久化用户笔记文件与本地缓存。Olares 的 `permission.userData` 可申请任意 Home 子目录，申请范围越大审核与信任成本越高。
- 决策 (Decision): `permission` 仅声明 `appData: true`、`appCache: true`、`userData: [Documents]`，运行时通过 `.Values.userspace.*` 取实际路径。
- 理由 (Reason): 与功能实际读写范围一致，符合最小权限原则；路径不写死，避免与系统注入值冲突。
- 备选方案 (Alternatives): 直接申请整个 `Home`（过度授权，审核风险高）。
- 影响 (Consequences): 后续若需读写其它目录，须修改 manifest 并登记新 ADR；好处是权限声明可审计、易解释。

## DEC-003 — 将 Olares 提交规定与 GitBot 避坑规则固化为模板硬约束
日期: 2026-10-25 | 状态: Accepted
- 确认来源: 项目维护者在 2026-10-25 会话中明确给出 8 类提交规定（图标、分类白名单、镜像权限、Nginx 非 root、前后端协同、运行时 Hook、元数据对齐、分支与 PR 规范）
- Supersedes: 无
- Superseded-by: 无
- 复审触发条件 (Review Trigger): GitBot 校验规则调整、Olares 分类白名单变更、或出现新的稳定复现的拦截报错时复审
- 背景 (Context): 这些规则分散在提交经验与报错回溯中，官方文档未完整覆盖（如分类白名单、Nginx 非 root 的 80 端口陷阱）。AI 在无约束时会凭通用 K8s 经验臆造配置，导致 PR 被驳回或运行期启动失败。
- 决策 (Decision): 在 `ai/OLARES.md` 中以 `[规定]` 标注集中记录，并同步写入 `ai/PROJECT.md` 硬约束与 `AGENTS.md` 动作禁区；标注体系区分 `[事实]`（官方文档）、`[规定]`（组织强制）、`[建议]`（可否决）、`[未决]`（待澄清）。
- 理由 (Reason): 规定需可判定才能约束 AI。每条都落到具体字段、具体挂载点或具体命令，避免"注意安全"这类无法执行的表述。
- 备选方案 (Alternatives): 只写在 PR 模板或提交清单中（AI 不会主动读，约束力弱）；只依赖官方文档（覆盖不全）。
- 影响 (Consequences): 模板文件变长，但违反成本下降；`[未决]` 条目（分类白名单与存量旧取值并存）需在 GitBot 规则更新后复核，避免长期挂账。
