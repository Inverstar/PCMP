# PCMP for Olares App 样板工程 (Boilerplate)

面向 **Olares 应用开发**的 PCMP Lite 定制模板。适用场景：单应用单仓库，以 OAC（Olares Application Chart）形式打包并分发至 Olares Market。

与通用 Lite 模板的区别：在 4 件套之上增加一层 **Olares 平台事实卡与提交规定**，把官方规范、GitBot 实际拦截规则、以及踩过的坑固化为可判定的硬约束，避免 AI 在 manifest 字段、权限范围、注入变量、Nginx 运行时上凭记忆臆造。

## 包含内容

```text
├── AGENTS.md                 # 智能体入口：协作守则 + 11 条动作禁区
├── ai/
│   ├── PROJECT.md            # 项目定位 + 18 条 Olares 平台红线（分 4 组）
│   ├── OLARES.md             # 平台事实卡与提交规定（14 章）
│   ├── STATE.md              # 全局状态快照（已合并活动任务与验收清单）
│   └── DECISIONS.md          # ADR（含 Olares 场景示例）
└── scaffold/                 # OAC 文件骨架，可直接复制进 chart 目录
    ├── Chart.yaml
    ├── OlaresManifest.yaml
    ├── values.yaml
    ├── owners
    ├── .helmignore
    ├── i18n/
    │   ├── en-US/OlaresManifest.yaml
    │   └── zh-CN/OlaresManifest.yaml
    └── templates/
        ├── deployment.yaml
        └── service.yaml
```

两层职责分明：`ai/` 管**上下文与约束**，`scaffold/` 管**文件结构**。删掉 `scaffold/` 不影响协议运转，删掉 `ai/OLARES.md` 后仍可按其信息源重建。

## 如何使用

1. 将 `AGENTS.md` 与 `ai/` 复制到应用仓库根目录；
2. 把 `scaffold/` 下的文件复制到 chart 目录，**目录名必须改成应用名**（本模板叫 `scaffold` 只是为了标识用途，复制后须改名为 `metadata.name` 的值，否则违反「四名一致」），再按实际应用改值；
3. 替换 `ai/PROJECT.md` 中 `<>` 占位符与示例应用名，逐条确认硬约束；
4. 对 AI 助手说：
   > `请按本项目的 AGENTS.md 接管开发任务，动 manifest 前先读 ai/OLARES.md。`

## 关键约束提示

**元数据与结构**

- 四名一致：目录名 == `Chart.yaml:name` == `metadata.name` == `metadata.appid`；
- 版本三处同步：`name`/`version`/`appVersion` ↔ `metadata.name`/`metadata.version`/`spec.versionName`；
- `i18n/zh-CN`、`i18n/en-US`、`owners`、`.helmignore` 缺一不可。

**展示字段**

- 图标必须精确 256×256、≤ 512 KB，`metadata.icon` 与 `entrances[*].icon` 都要换，推荐仓库 raw 直链；
- `categories` 只能取 `agents` / `developer` / `homelab` / `models` / `workspace`，严禁自定义。

**安全与运行时**

- 第三方镜像禁 `runAsUser: 0`，统一 UID 1000；修目录权限用 `beclab/aboveos-busybox` 做 initContainer；
- Nginx 非 root 必须挂 5 个 emptyDir，并用空目录遮蔽 `/etc/nginx/sites-enabled`，否则绑定 80 端口失败；
- 端口链路三处一致：containerPort == Service `targetPort`/`port` == `entrances[*].port`。

**分发**

- 基于最新 `upstream/main` 拉 `feature/<app>-v<version>`，与 upstream 只差 1 个 Commit；
- 提交信息按 `[NEW][<foldername>][<version>]<Title>` 或 `[UPDATE][...]` 格式；Bugfix 递增小版本号并同步双语 `upgradeDescription`；
- `olaresManifest.version` 与 `apiVersion` 官方文档滞后于真实仓库，取值以目标 OS 版本可安装的真实应用为基线（见 `ai/OLARES.md` §13）。

## 维护

`ai/OLARES.md` 属外部事实层，Olares 大版本更新后需重新核对官方文档与 `beclab/apps` 真实应用，核对后更新文末日期。规则来自 GitBot 实际拦截行为的条目，若平台策略调整应同步复核。
