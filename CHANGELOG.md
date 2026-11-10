# Changelog

本文件记录本项目的所有重要变更。

格式基于 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)，版本号遵循[语义化版本](https://semver.org/lang/zh-CN/)。

> 本文件是版本历史的**单一事实来源**：README 仅预告最新版本亮点，GitHub Release 的内容取自本文件。

## [Unreleased]

- 暂无待发布的变更。

## [v5.0.3] - 2026-11-10 — SDK 3.10.6 知识基线

> 新增知识根 `memoryV5.0.3/`，`memoryV5.0.2/` 保留不动（并存快照）。

### Added
- `entities/api/vrctween.md`（新）：VRCTween / VRCTweenHandle API 面 + 「与 DOTween 的边界」。
- `entities/world/scene-components/` 新增 5 页：`vrc-billboard.md`、`vrc-velocitytracker.md`、`vrc-placementzone.md`（均 3.10.6）、`world-quality-settings.md`（3.10.5）、`interact-outline-renderers.md`（3.10.5/3.10.6）。
- `_curator_tools/`：本轮审计产物（validation / governance / version audit / 团队审计 / SDK oracle / 基线快照）。

### Changed
- **SDK 基线 3.10.3 → 3.10.6**：3.10.4（VRCTween、Contacts Box 形状、World 侧 VRCPhysBoneCollider 暴露、DataList/DataDictionary 容量）、3.10.5（VRCQualitySettings 读写、WorldQualitySettings、Assembly Version Defines、Assets/Packages 建 U# 脚本）、3.10.6（6 个 VRC Constraint 的 source 方法组 + SmoothingFactor、VRCBillboard/VRCVelocityTracker/VRCPlacementZone、U# enum 比较与命名参数修复、null VRCUrl 同步修复）。
- **15 项第三方工具版本追平**：MA 1.18.7 / AAO v1.9.21 / VRCFury v1.1442.0 / Meshia 3.3.1 / TTT v1.0.2 / LAC v0.9.0 / lilToon 2.4.1 / ORL v7.3.1 / UnlitWF 20261101 / Graphlit v2.7.4 / VRCLV v3.0.1 / VVMW 1.8.3 / UVU 7.0.0 / NDMF 1.14.8 / ULocalization v0.8.17。
- `FACT.md` 新增「VRChat SDK 版本基线」与工具链现行版本表；`index.md`、`_always-load.md`、域索引同步。
- 仓库根新增 `.gitattributes`（`* text=auto eol=lf`），固定入库换行。

### Fixed
- **`OnContactStay` 不存在**：Contacts 仅 `OnContactEnter` / `OnContactExit`；持续接触须 Enter 置标志 + Exit 清标志 + `Update` 读取。全库相关表述统一纠正。
- **`GetSourceCount()` / `SetSourceWeight()` 不存在**：按索引改权重须 `GetSource` → 改 `Weight` → `SetSource`。
- **VRCTween 归属**：3.10.4 引入（原记 3.7+，无锚点）。
- **`Use Face Proximity`** 为组件 authoring 字段，非 Udon wrapper 方法。
- **`{parameter}_SenderType` 0–4 编码**补官方原文与生效条件（仅 Receiver 勾选 Advanced Parameters 后生成）。
- **lilToon 2.1.5 / 2.1.6 归属**：毛发 VRCLV 改逐像素属 2.1.5；2.1.6 为 VRCLV optimization。
- **lilToon / UnlitWF 的 VRCLV 与 LTCGI 支持**：原对比表误标为不支持（lilToon 1.10.0 起 VRCLV、1.8.0 起 LTCGI）。
- **相对链接错层 8 处**（shader 索引 / comparisons 的 `[[...]]` 残留与 `../` 层数错误）。

### 保留（本轮不改）
- 三个导出快照页（`udon-type-exposure` / `exposed-types` / `not-exposed`）**保留 3.10.3**，注明「本轮未重新导出」——无证据不抬版本。
- `related` 字段格式债（150 条 `[[...]]` 残留 / 47 条目标缺失 / 3 条中文脏数据）与 35 处正文伪链接，均与 v5.0.2 基线持平，建议单独立项。

### 校验
- `validation_script.py`：**421/421 通过，0 失败**（基线 411/415，4 失败）。
- 治理死链：220 → 214（无新增）；版本审计 outdated：14 → 1（VRCFury beta tag 口径）、unknown 1 → 0。


## [v5.0.2] - 2026-11-10 — 显示名机制新页 + 索引同步 + 目录版本号对齐

> 版本号跳过 5.0.1（未单独发布），本批改动一并进入 v5.0.2。

### Added
- Avatar 域新页 `entities/avatar/menu-display-name-resolution.md`（贡献：@LinnnnYue，PR #5）：LI（lilycalInventory 1.5.2）与 MA（Modular Avatar 1.18.7）菜单显示名的来源与回退链——组件级 2 级、Costume 级 7 级、MA label 2 级；另含 Inspector 的 label↔GameObject 名绑定、Unity 序列化转义规则（`\xNN` / `\uNNNN`）、批量改名的数组索引陷阱（第 N 格 → `costumes.Array.data[N-1]`）与安全写入 5 步。结论已按上游源码逐条复核。

### Changed
- **知识根目录版本号对齐**：`memoryV5.0.0/` → `memoryV5.0.2/`（README 与包内发布说明同步；历史版本条目保留原目录名，不回溯改写）。
- **索引同步**：`entities/avatar/index.md`（MOC 入口、核心文档表、子域表、frontmatter related；版本 1.10 → 1.11）与顶层 `index.md`（Avatar 域路由表、文件清单）登记新页。
- **页数口径校正为实际值**：知识页 414 → 416（v5.0.0 报告后新增 1 页未计入），`entities/` 351 → 353，`entities/avatar/` 99 → 101，`FACT.md` Avatar 域 63 → 65 个文件。

### Fixed
- `menu-display-name-resolution.md` §2.2：Costume 级回退链「六级」更正为「七级」（与同节 ①–⑦ 表格及源码一致）。
- `menu-display-name-resolution.md` `sources` 字段：源码路径 `Runtime/ObjHelper.Component.cs` 更正为 `Editor/Helper/ObjHelper.Component.cs`（上游该文件不在 Runtime 下）。
- `menu-display-name-resolution.md` 挂入域索引与顶层路由，消除「合入后无法从 index 到达」的孤页风险（对应 C3 口径）。

---

## [v5.0.0] - 2026-08-20 — LLMWikiPage 自包含知识发布包

### Added
- Unity 场景组件专页约 45 页：AudioSource / Light / NavMeshAgent / ParticleSystem / RigidBody / Collider / Joint / Camera / Canvas / Renderer / VideoPlayer 等。
- 工具页：udonsharp-linter、unity-mcp、dressing-tools、gesture-manager、lilycal-inventory、faceemo、runtime-texture-compression、physbones、avatar-audit-methodology。
- API / 组件专页：vrc-billboard、unity-audio-filters、unity-constraints、unity-rendering-aux、vrc-rendertexture。

### Changed
- **形态转型**：从「SOUL + Memory 工作库」转为「纯知识发布包」，知识根 = `memoryV5.0.0/LLMWikiExample/LLMWikiPage/`（414 个 Markdown 知识页，约 3.90 MB）。
- **目录重构**为 `entities`（351）`concepts`（47）`comparisons`（6）`queries`（1）`meta`（6）+ 顶层 `FACT.md` / `index.md` / `_always-load.md`。
- **全库自包含**：仅依赖可解析 `[[wikilink]]` 与 `http(s)://` URL，零硬编码本地路径（正文路径示例已参数化，无个人路径）。
- 审查报告自发布包移出至仓库根 `reports/`（`V5独立审查报告-2026-08-20.md`、`发布检查报告-2026-08-20.md`），**不随发布包分发**；发布包仅含 `memoryV5.0.0/`，GitHub Release 仅分发该文件夹。

### Fixed
- **审查循环修复轮**（修复后全指标复跑归零）：绝对路径示例参数化（10 页→0）；soft raw 锚点改公开 URL / 抽象描述（4→0）；vrchatsdk 18 页 `source` 补全真实官方 / 社区 URL；全库旧工作库层（raw / sources / references / reviews）路径文本清除（44 文件）；obsidian-governance 重写（去个人路径、拓扑对齐）；queries 归档填充 5 条真实问答；重复 tag 清除；库内相对路径提示全量解析校验 0 缺失。
- **复跑发布就绪四指标全门 0**（`publish-readiness-check.py` 幂等验证，SHA-256 两次一致）：
  - C1 自包含：死链文件链接 0 / 站点相对 URL 0 / wikilink 死链 0
  - C2 置信度：非法值 0（high 204 / medium 208 / low 2，low 已带来源 + 理由）
  - C3 Fact Index：孤页 0 / index 死链 0（FACT.md 结构树 = 44 目录）
  - C4 标签：缺失 0 / 分类法 150 / 分类法外复用（≥2 次）0
- 交叉口径：site-scan 知识域 363 页（死链 0 / frontmatter 100%）；factcheck-audit 411 页。
- **独立 V5 连接自包含审查**：本地文件连接 = 0（publish-readiness C1–C4 幂等两跑 + 包边界目标级审计 + precise_scan + self_contain 四口径一致）；库内相对路径可接受；四指标独立复跑全门 0；factcheck 锚定缺口 0。**零违例，无需修复，版本保持 v5.0.0**。

### Removed
- 移除旧 `memory v3.0.0/` 工作库目录。
- 工作库 `memoryV4.0.0/`（466 文件）归档（不随发布包分发）。
- 治理层（reviews / _curator_tools / .obsidian / 备份 / 源注册表）不随发布包携带，治理内容收敛至 `meta/`。

---

## [v4.0.0] - 2026-07—08 — 工作库阶段（未对外发布）

### Added
- 保留 v3 拓扑的持续建设：新容器 / 工具 / 组件页、治理沉淀、Obsidian 审计。

### Changed
- 作为 v5.0.0 的数据源工作库（466 文件），未单独打版发布。

---

## [v3.0.0] - 2026-07-01

### Added
- 🎨 **Poiyomi Shaders 完整入库**：8 主题知识文档 + 65 篇原始参考文档（10.0 版本），覆盖五大变体、AudioLink 集成、Modular Shader System、9 种 Lighting Type、Quest 优化策略。
- 👤 **玩家操作知识库导入**：从 VRChat 官方 user-guide 导入 6 篇新建文档（Trust Rank、Safety System、Skeletal Input、标准手姿、Expression Menu、Launch Options）+ 2 篇增量修改。
- 🔧 **VCC / ALCOM 系统化**：VCC 架构 / 工作流 / VPM 格式 + ALCOM 开源替代品（Rust + Tauri, MIT）的完整参考文档。
- 📡 **OSC 协议重大更新**：从 23KB 扩展到 32KB（+40%），修正 P0 Chatbox 3 参数签名冲突，新增 10+ 核心补充、6 行为细节、8 附录含资源分类索引。
- 📦 **VPM 镜像站完整数据**：vcc.vrczh.org 全量 57 个 VPM 仓库元数据，含 8 大核心包详细分析 + 镜像 / 上游对比。
- 🌍 **Community Labs / Companions / Steam Audio**：World 发布流程（Stage 2）、Items vs Props 分裂、2025.4.2+ 音频系统迁移。
- 📹 **VRCCameraSettings / VRCQualitySettings**：屏幕 / 手持相机 + VR 双眼模式、阴影距离覆盖 + 质量只读。

### Changed
- **规模变化**：文件从 313 增至 351（+38），总规模从 2.8MB 增至 3.26MB。
- World 域 149→154 篇；Avatar 域 51→75 篇（+47%，Shader 子域大幅扩展）；Sources 域 14→25 篇（VPM 镜像站数据）；Hybrid 域 4→6 篇（VCC / ALCOM）。
- **知识库治理指标**：YAML frontmatter 覆盖率 ~95% → **100%**；死链 ~26 → **0**；搜索可达率（T1 实测）**99.6%**；文档数 351；总大小 ~3.26 MB。

### Fixed
- **A18 — V3.1 正式发布**：数据基线修正 351/351 验证通过（92% → 100%）；26 个死链清零；27 个 frontmatter 失败修复；8 个并行 Agent 加速比 2.9x。
- **A19 — 全面审计**：T1 搜索可达性 981 个测试问题、整体可达率 99.6%；T2 框架合理性识别 212 个 aliases 不足、123 个 related 缺失；T3 元文件审查修复 4 项 P0 问题（SOUL last_review、路径错误等）；SOUL.md 非人格内容移除、数据基线声明准确。
- **A20 — 搜索可达性实测**：12 个并行 Agent 覆盖 346/351 文档（98.6%）；1 处路径笔误修复、3 处迁移断链标记；确认知识库无内容缺失。
- **总结**：v3.0.0 是「内容规模化 + 质量工程化」的双重升级——完成 Avatar Shader（Poiyomi 8 主题）、VPM 生态（57 仓库）、OSC 协议（+40%）等关键内容的大规模填充，并经 A18-A20 三个治理周期建立完整的知识库质量保障闭环。

---

## [v2.0.0] - 2026-06-21

### Added
- **内容增量**：文件从 295 增至 313（+18），总规模从 2.5MB 增至 2.8MB。
- 新增 VRCTween 官方补间文档。
- 新增 Udon Persistence 实战子目录（8 文档）。
- Data Containers 子路径重构。
- LuraSwitch2 更新至 v1.06（VN3 License）。
- VRCGraphics 重构纳入 udon 子目录。

### Changed
- **架构升级**：根目录新增 `SOUL.md`（v5.0-slim），定义 Agent 为「知识驱动型 VRChat 技术架构师」，包含 6 种工作模式自动选择、6 域路由表和工程人格设定。
- **元数据标准化**：所有核心文件新增 YAML frontmatter（title、category、tags、confidence 等），实现可机器解析的知识图谱层。
- **知识库治理体系建立**：新增 `meta/` 目录，含知识写入协议、工作模式详细定义、辅助脚本清单三项核心治理文件，从 SOUL.md 迁移瘦身而来；`_curator_tools/` 为未来策展工具预留。
- **总结**：v2.0.0 是架构性升级，核心变化是 Agent 身份定义 + 元数据标准化 + 治理体系建立，为知识库的可持续维护奠定基础。

---

## [v1.0.0] - 2026-06-20

### Added
- VRChat 技术知识库初始版本：295 个文件，覆盖 World、Avatar、Hybrid、Platform、VRChatSDK、Sources 六大领域。
