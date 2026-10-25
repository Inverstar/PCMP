# 工程智能体规范 (AGENTS.md - Olares App)

## 项目定位

本项目是一个 **Olares 应用**，采用 **PCMP Lite** 管理上下文：纯 Markdown 驱动、零工具依赖、单目录自足。
交付物为符合 OAC（Olares Application Chart）规范的应用包，最终分发至 Olares Market。

## 关键路径

- 项目硬约束：[ai/PROJECT.md](ai/PROJECT.md)（19 条平台红线，动工前必读）
- Olares 平台事实卡与提交规定：[ai/OLARES.md](ai/OLARES.md)（OAC 结构、图标与分类、安全策略、Nginx 非 root、Hook、PR 规范）
- 状态与活动任务：[ai/STATE.md](ai/STATE.md)（接管必读）
- 决策真源：[ai/DECISIONS.md](ai/DECISIONS.md)

## 智能体协作守则

1. **接管必读**：换新会话先读 `ai/PROJECT.md` 与 `ai/STATE.md`，明确当前任务与红线后再动工；
2. **先确认参考仓库**：开工前必须拿到本地可读的 `beclab/apps`（官方仓库或 fork）路径，登记到 `ai/OLARES.md` §14。路径缺失时**先向人类索取，不要动手**——所有字段、目录结构、运行时配置都应从同类应用中对照得出，而不是凭记忆生成；
3. **改 manifest 前先查事实卡**：任何 `OlaresManifest.yaml`、`Chart.yaml`、`templates/` 的改动，先对照 `ai/OLARES.md` 对应章节，不得凭记忆补字段；
4. **四名与版本成组修改**：目录名、`Chart.yaml:name`、`metadata.name`、`metadata.appid` 四处同名；`name`/`version`/`appVersion` 与 `metadata.name`/`metadata.version`/`spec.versionName` 三组同步，禁止只改一处；
5. **先查白名单再填展示字段**：`categories` 只取 `agents`/`developer`/`homelab`/`models`/`workspace`；图标必须 256×256 且 ≤ 512 KB，改完核对尺寸与体积；
6. **运行时改动先查避坑章节**：涉及 Nginx、initContainer、权限、端口时，先读 `ai/OLARES.md` §7 与 §8，按既定写法改；
7. **报错先检索再改**：遇到启动失败或审核驳回，先在参考仓库检索相同报错涉及的关键字，看同类应用如何规避，再决定改法；
8. **状态合一**：Lite 档位不维护独立 `TASK.md`，任务进展、验收勾选与 Next Action 直接写入 `ai/STATE.md`；
9. **决策留痕**：权限范围、鉴权级别、镜像来源、分类选择、规范版本基线等变更，必须在 `ai/DECISIONS.md` 追加 ADR 并附确认来源；
10. **收尾更新**：任务完成后递增 `ai/STATE.md` 的 `revision`，更新完成项与即时下一步。

## 动作禁区

以下操作未经人类明确授权一律不做：

1. **不无参考地写配置**：参考仓库路径未登记前，不生成 `OlaresManifest.yaml`、`templates/` 的字段与结构；不凭"官方应该是这样"补全任何未核实的键；
2. **不给第三方镜像 Root**：不对非 `beclab` 镜像配置 `runAsUser: 0`；权限修复只走 `beclab/aboveos-busybox` initContainer，不给业务容器提权；
3. **不放宽安全边界**：不把 `entrances[].authLevel` 改为 `public`，不扩大 `permission` 申请范围，不加 `privileged`；
4. **不碰分发控制文件**：不创建或提交 `.suspend`、`.remove`；不修改 `owners` 账号列表；
5. **不猜规范版本**：`olaresManifest.version` 与 `apiVersion` 未核实前不填值，先报 `[未决]`；
6. **不自定义分类**：不填白名单之外的 `categories`，不复用旧取值（`Entertainment`、`Tools`、`Utilities`、`Productivity`）；
7. **不使用不合格图标**：不接受 512×512、1024×1024、非正方形或超过 512 KB 的图标，不引用临时图床；
8. **不硬编码注入值**：不在 `values.yaml` 或模板中写死用户名、域名、存储路径、中间件凭据；
9. **不用浮动镜像标签**：不引入 `latest` 或未固定 digest 的镜像；
10. **不为改配置而重建镜像**：上游缺陷优先用 `/patch` + `sitecustomize.py` 运行时 Hook；
11. **不擅自扩分支范围**：不在同一分支夹带其他应用变更，不引入 Merge 提交，保证与 `upstream/main` 只差 1 个 Commit；
12. **不改参考仓库**：不修改参考仓库中的其他应用，不整目录复制其文件而不做适配；
13. **不扩大审阅范围**：不主动扫描应用包之外的工程目录。

## 遇到冲突时

官方文档与真实应用仓库不一致时（典型如 `olaresManifest.version`、`apiVersion`、`categories` 取值），按 `ai/OLARES.md` §13 处理：以目标 OS 版本可安装的真实应用为基线，并向人类报告差异，不要自行二选一。
