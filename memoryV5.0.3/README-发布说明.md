# VRChat-Agent-Open-Memory-Library · LLMWikiPage 知识发布包（v5.0.3 · 2026-11-10）

## 包内容

- `LLMWikiExample/LLMWikiPage/` —— **422** 个 Markdown 知识页（完全自包含：内链为 `[[wikilink]]`
  或相对路径 markdown 链接 + 完整外链 URL；不依赖除 URL 以外的任何本地路径）
- 本说明

> 📋 本轮审计产物（校验报告、版本审计、团队审计、SDK oracle）存放于 `LLMWikiPage/_curator_tools/`，
> 随本版一并入库作为变更证据。

## 相对 v5.0.2 的变更

### Added — SDK 3.10.4 / 3.10.5 / 3.10.6 新能力

- `entities/api/vrctween.md`（新）：VRCTween / VRCTweenHandle API 面，含「与 DOTween 的边界」——官方只暴露 DOTween 的 "select functionality"，不得假设全量 API 可用。
- `entities/world/scene-components/vrc-billboard.md`（新）：VRCBillboard 世界端行为（Transform 键控最后注册胜出、任意脚本可 Unregister、pass 在 Udon PostLateUpdate 之后、默认 yaw-only）。
- `entities/world/scene-components/vrc-velocitytracker.md`（新）：四个 getter 的单位与采样基准，禁用时返回 0。
- `entities/world/scene-components/vrc-placementzone.md`（新）：限制用户放置内容；仅适用于用户放置的 portal/sticker 等，非作者摆的 `VRC_PortalMarker`；动态改区必须网络同步。
- `entities/world/scene-components/world-quality-settings.md`（新，3.10.5）：场景阴影距离 / Shadowmask Lighting Mode / 实时探针。
- `entities/world/scene-components/interact-outline-renderers.md`（新，3.10.5/3.10.6）：Interact 描边渲染器字段，**无 Udon accessor**。

### Changed — 第三方工具版本追平（15 项）

| 工具 | 旧 | 新 |
|---|---|---|
| Modular Avatar | 1.17.1 | **1.18.7** |
| AvatarOptimizer | v1.9.14 | **v1.9.21** |
| VRCFury | v1.1341.0 | **v1.1442.0**（stable；beta 通道 1.1442.1-beta.15） |
| Meshia | 3.2.0 | **3.3.1** |
| TexTransTool | v1.0.1 | **v1.0.2** |
| LAC (avatar-compressor) | v0.8.0 | **v0.9.0** |
| lilToon | 2.3.3 | **2.4.1** |
| ORL Shaders | v7.2.0 | **v7.3.1** |
| UnlitWF | UnlitWF_Shader_20260208 | **UnlitWF_Shader_20261101** |
| Graphlit | v2.7.2 | **v2.7.4** |
| VRC Light Volumes | v2.1.3 | **v3.0.1**（断裂大版本，含迁移步骤） |
| VizVid (VVMW) | 1.7.5 | **1.8.3** |
| UdonVoiceUtils | 5.1.4 | **7.0.0** |
| NDMF | 1.14.0 | **1.14.8** |
| ULocalization / VRCX | v0.8.16 / v2026.05.03 | v0.8.17 / v2026.09.16（库内无落点页，记入审计报告） |

### Fixed — 事实纠错

- **`OnContactStay` 不存在**：VRChat Contacts 只有 `OnContactEnter` / `OnContactExit`；「持续接触」须 Enter 置标志 + Exit 清标志 + `Update` 读标志。写成 `OnContactStay` 可能编译通过但运行时静默无回调。全库相关表述已统一。
- **`GetSourceCount()` / `SetSourceWeight()` 不存在**：约束按索引改权重须 `GetSource` → 改 `Weight` → `SetSource`。官方 Constraints API 页全文检索这两名为 0 处。
- **VRCTween 是 SDK 3.10.4 引入**（旧记 3.7+，无锚点）。
- **`Use Face Proximity` 是组件 authoring 字段**，非 Udon wrapper 方法。
- **`{parameter}_SenderType` 0–4 编码**补官方原文与生效条件（仅在 Receiver 勾选 Advanced Parameters 后生成）。
- **lilToon 2.1.5 / 2.1.6 归属修正**：毛发 VRCLV 改逐像素属 2.1.5，2.1.6 为 VRCLV optimization。
- **lilToon / UnlitWF 的 VRCLightVolumes 与 LTCGI 支持**：原对比表标为不支持，实为 lilToon 1.10.0 起支持 VRCLV、1.8.0 起支持 LTCGI。

### Changed — 文档结构与口径

- 三个「导出快照」页（`entities/api/udon-type-exposure.md` / `exposed-types.md` / `not-exposed.md`）**保留 3.10.3 版本号**并注明「本轮未重新导出」：官方 Class Exposure Tree 仅为编辑器窗口，无可抓取静态导出。**无证据不抬版本。**
- 新增 `entities/api/official-doc-clarifications.md` §16：记录 9 条官方文档自相矛盾（含 `VRCQualitySettings` 页导言 "read-only" 与新增 setter 冲突）。
- `FACT.md` 新增「VRChat SDK 版本基线（2026-11-10 复核）」整节 + 工具链现行版本表。
- 死链修复 8 处（shader 索引与 comparisons 的相对路径错层、`[[...]]` 残留）。
- 索引同步：`entities/world/scene-components/index.md`、`entities/avatar/index.md`、`entities/world/index.md`、顶层 `index.md`、`_always-load.md`。

## 就绪判定（本次发布前复算）

| 指标 | 判据 | 本次复算 |
|---|---|---|
| C1 自包含 | 相对链接死链（真实路径） = 0；正文伪链接（代码块内的 C# 签名/模板占位）35 处与 v5.0.2 基线持平 | ✅ 0 新增 |
| C2 置信度 | 缺失或非法 = 0 | ✅ 0（High 210 / Medium 210 / Low 2） |
| C3 Fact Index | 索引页可达；新增 6 页与保留页均已挂链 | ✅ |
| C4 标签 | 每页 3–8 个 | ✅ 0 违例（`validation_script.py` 421/421 通过） |

页数分布：`entities/` 359（其中 `avatar/` 101）、`concepts/` 47、`comparisons/` 6、`meta/` 6、`queries/` 1、根 3（`index.md` / `FACT.md` / `_always-load.md`）。共 **422** 页。

## 已知保留项（非本批缺陷）

- **`related` 字段格式债**：150 条 `[[...]]` 残留（目标存在，仅是格式不规范）、47 条纯路径目标缺失（多为 `entities/world/` 下指向已不存在的 `systems/` / `obstacle-course/` / `drones/` / `tools/` / `patterns/` 目录，系历史移植残留）、3 条中文描述误作路径。**本轮不动**：不补建目录、不批量删 `related`，建议单独立项。
- **正文伪链接 35 处**：代码块/行内代码里的 C# 泛型签名（`[T](Boolean)`）与模板占位（`[工具名](官网链接)`），非断链，与 v5.0.2 基线一致。
- **换行**：本版全部文件为 **LF**（仓库既有约定）；原工作副本中的 CRLF 混排未带入。
- **遮罩与渲染数据**：本包无二进制资产，全部为 Markdown。

## 证据与审计

- 全库校验：`python validation_script.py` → **421/421 通过，0 失败**（基线 411/415，4 失败）。
- 治理扫描：死链 220 → 214（无新增）。
- 版本审计：outdated **14 → 1**（唯一 1 条为 VRCFury beta tag，属脚本取 `releases/latest` 的口径问题，非知识落后）；unknown 1 → 0。
- 详见 `LLMWikiPage/_curator_tools/a14_sdk3106_team_audit.md` 与 `_verifier_sdk3106_oracle.md`。
