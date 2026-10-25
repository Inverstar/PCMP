# 项目核心定义 (PROJECT.md)

> 本文件为模板示例，`<>` 与示例应用名请替换为实际内容。硬约束部分请逐条确认后再删改。
> 详细依据与操作写法见 [OLARES.md](OLARES.md)，本文件只列结论。

## 1. 项目定位与宗旨

- **项目名称**：`<应用名>`（示例：Sample Notes）
- **核心定位**：面向 Olares 用户的 `<一句话说明应用做什么>`，以 OAC 形式打包分发至 Olares Market。
- **目标运行环境**：Olares `>= <版本，如 1.12.6-0>`，架构 `amd64` / `arm64`。

## 2. 核心目标 (Scope)

- 提供 `<核心能力 1>`；
- 提供 `<核心能力 2>`；
- 通过 Olares 入口（Entrance）暴露 Web UI，按需申请数据与缓存目录；
- 可按 OAC 规范打包、升级，并通过 Terminus-Gitbot 校验。

## 3. 明确非目标 (Non-Goals)

- 不做 Olares 系统级改造（不修改平台组件、不改动 cluster 级资源）；
- 不申请超出功能所需的用户目录与网络出站权限；
- 不为规避平台限制而重建上游镜像（优先用运行时 Hook）。

## 4. 不得违反的硬约束 (Hard Boundaries)

违反任一条即导致安装失败、GitBot 驳回或权限越界。

### 4.1 参考基线

1. **先有参考仓库再动手**：开工前须在 [OLARES.md](OLARES.md) §14 登记本地 `beclab/apps`（官方仓库或 fork）路径。manifest 字段、目录结构、运行时写法一律从同类应用对照得出，禁止凭记忆生成。路径缺失时先向人类索取，不得跳过。

### 4.2 元数据与结构对齐

2. **四名一致**：应用目录名、`Chart.yaml:name`、`metadata.name`、`metadata.appid` 完全相同，全小写无特殊字符。
3. **版本三处同步**：`Chart.yaml` 的 `name`/`version`/`appVersion` 分别等于 manifest 的 `metadata.name`/`metadata.version`/`spec.versionName`。
4. **镜像版本锁定**：`values.yaml` 的 `image.tag` 禁用 `latest`，须为具体上游版本，并与 `spec.versionName` 一致。
5. **必备文件齐全**：`i18n/zh-CN`、`i18n/en-US`、`owners`、`.helmignore` 缺一不可。

### 4.3 展示元数据

6. **图标严格合规**：`metadata.icon` 与 `entrances[*].icon` 均须为 **256×256 像素**、**≤ 512 KB** 的 PNG/WEBP 公网直链，推荐仓库 raw 链接。
7. **分类白名单**：`metadata.categories` 只能取 `agents` / `developer` / `homelab` / `models` / `workspace`，严禁自定义名称。

### 4.4 安全与运行时

8. **第三方镜像禁 Root**：非 `beclab` 镜像禁止 `runAsUser: 0`，统一 UID 1000 + `runAsNonRoot: true`。
9. **权限修复只走 initContainer**：目录属主修复用 `docker.io/beclab/aboveos-busybox:1.37.0` 执行 `mkdir -p` / `chown -R 1000:1000` / `chmod -R 775`，不给业务容器提权。
10. **禁止硬编码系统注入值**：用户名、域名、存储路径、中间件地址与凭据一律引用 `.Values.bfl.*`、`.Values.userspace.*`、`.Values.postgres.*` 等，详见 [OLARES.md](OLARES.md) §6。
11. **Nginx 非 root 五挂载齐全**：`/etc/nginx/conf.d`、`/var/lib/nginx`（预建 5 个子目录）、`/var/log/nginx`、`/run`、`/etc/nginx/sites-enabled`（空 emptyDir 遮蔽 80 端口站点），缺一即启动失败。
12. **业务端口非特权**：监听 8000/8080 等端口，禁止 80/443；容器端口、Service `port`/`targetPort`、`entrances[*].port` 三处一致。

### 4.5 架构与分发

13. **前后端同域交付**：Web 前端与后端两镜像优先单 Pod 多容器 + 内置 Nginx 反代，保证 Same-Origin，不靠 CORS。
14. **上游缺陷用 Hook 修**：镜像配置解析缺陷优先用 ConfigMap 挂 `/patch` + `PYTHONPATH` + `sitecustomize.py` 补丁，不重建镜像。
15. **权限最小化**：`permission` 只声明功能实际需要的目录；`userData` 写具体子目录，不得直接申请整个 `Home`。新增权限须登记 ADR。
16. **入口鉴权默认收紧**：`authLevel` 保持 `private` 或 `internal`，改 `public` 必须有 ADR 说明必要性。
17. **一分支一提交**：基于最新 `upstream/main` 拉 `feature/<app>-v<version>`，与 `upstream/main` 对比只能有 1 个 Commit，提交信息按 `[NEW]`/`[UPDATE]` 格式。
18. **规范版本不靠猜**：`olaresManifest.version` 与 `apiVersion` 以目标 OS 版本可安装的真实应用为基线，冲突时按 [OLARES.md](OLARES.md) §13 处理。
19. **分发文件不得误动**：`owners` 变更须经维护者确认；`.suspend` / `.remove` 严禁为调试而提交。

## 5. 本地验证方式

- 参考基线：确认 [OLARES.md](OLARES.md) §14 已登记参考仓库路径，新增字段均能对应到某个真实应用；
- 结构自检：对照 [OLARES.md](OLARES.md) §1 核对必备文件，逐条比对 §4 硬约束；
- 元数据校验：四名一致、版本三处一致、图标尺寸与体积、分类白名单；
- 模板渲染：`helm template <chart-dir>` 确认 YAML 可渲染；
- 安装验证：在 Olares 主机实际安装、访问入口、执行一次升级。
