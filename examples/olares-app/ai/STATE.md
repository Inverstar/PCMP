---
pcmp_version: "2.2"
revision: 2
updated: 2026-10-25
---

# 项目当前状态与活动任务快照 (STATE.md - Lite)

- **Last Updated**: 2026-10-25
- **Overall Status**: 应用本体功能可用，正在按 OAC 规范补齐打包配置，尚未提交 Market。

---

## 🎯 当前唯一活动任务 (Active Task)

> Lite 档位特性：活动任务直接合并维护在此，无需单独创建 `TASK.md`。

- **任务名称**：完成 OAC 打包并首次提交 Olares Market
- **背景与范围**：应用功能已可运行，需按 OAC 规范补齐元数据、入口、权限、资源与国际化文件，通过本地安装验证后提交 PR。
- **约束与非目标**：权限按实际需要最小声明；非目标：本次不做多架构镜像构建，仅提交 `amd64`。
- **关联文件**：`Chart.yaml`、`OlaresManifest.yaml`、`values.yaml`、`templates/deployment.yaml`、`i18n/`、`owners`、`.helmignore`
- **验收标准**：
  - [ ] 四名一致：目录名 == `Chart.yaml:name` == `metadata.name` == `metadata.appid`；
  - [ ] 版本三处对齐：`name`/`version`/`appVersion` ↔ `metadata.name`/`metadata.version`/`spec.versionName`；
  - [ ] 图标为 256×256、≤ 512 KB 的公网直链，`metadata.icon` 与 `entrances[*].icon` 均已替换；
  - [ ] `categories` 仅取白名单值（`agents`/`developer`/`homelab`/`models`/`workspace`）；
  - [ ] `i18n/zh-CN` 与 `i18n/en-US` 已提供，`owners` 与 `.helmignore` 就位；
  - [ ] `entrances` 至少 1 个，`authLevel` 已确认；
  - [ ] `permission` 与实际读写目录一致，无多余申请；
  - [ ] `required*` 与 `limited*` 资源值经实测填写；
  - [ ] 镜像 tag 已固定且等于 `spec.versionName`，无 `latest`；
  - [ ] 容器端口、Service `port`/`targetPort`、`entrances[*].port` 三处一致；
  - [ ] 第三方镜像以 UID 1000 运行，无 `runAsUser: 0`；
  - [ ] `helm template` 渲染通过，Olares 主机安装成功；
  - [ ] 分支与 `upstream/main` 只差 1 个 Commit，提交信息符合 `[NEW]`/`[UPDATE]` 格式。

---

## 已完成里程碑 (Completed)

- [已确认] 应用核心功能实现并通过本地验证；
- [已确认] 基础 `templates/` 资源模板就位。

## 进行中 (In Progress)

- [进行中] 编写 `OlaresManifest.yaml` 元数据与 `entrances` 声明。

## 阻塞项 (Blocked)

- 无

## 当前风险与焦点 (Risks & Focus)

- `olaresManifest.version` 与 `apiVersion` 取值需以真实可用应用为基线，不可凭文档或记忆填写（见 `ai/OLARES.md` §13）；
- 首次提交若 `owners` 与提交账号不一致，GitBot 会直接驳回；
- Nginx 反代场景若漏挂 `/etc/nginx/sites-enabled` 空目录，非 root 绑定 80 会启动失败。

## 下一步即时动作 (Immediate Next Action)

- 参照 [OLARES.md](OLARES.md) §3 逐键核对 `OlaresManifest.yaml`，补齐 `i18n/` 与 `spec` 资源声明后执行 `helm template` 验证。
