---
title: "A14 SDK 3.10.6 团队审计报告（独立复验）"
category: meta
knowledge_level: auxiliary
status: active
tags: [audit, verifier, sdk-3.10.6]
source: 本轮 verifier 独立复跑脚本 + 官方 release note/API 文档核对
version: 1.0
last_review: 2026-11-10
---

# A14 SDK 3.10.6 团队审计报告（独立复验）

- 审计人：**verifier**（task-5，独立于 4 个 worker，不采信其自检）
- 审计日期：**2026-11-10**（Asia/Shanghai）
- 知识库：`D:\UDON\memoryV5.0.0\LLMWikiExample\LLMWikiPage\`（审计时 **421** 个 md）
- 脚本：`auxiliary-script`（Python 3.13，纯标准库）
- 复验基线：`_curator_tools/_verifier_baseline_snapshot.json`（416 文件 × 字节数/CRLF-LF 计数/SHA256/mtime，抓取时刻约 **04:12**）

---

## 0. 结论摘要

| 项目 | 结果 |
|---|---|
| validation | **421/421 通过，0 failed**（基线 411/415，4 failed） |
| governance 死链 | **220 → 214**（净减 6，无新增） |
| version_audit outdated | **14 → 1**（唯一 1 条为脚本取到 VRCFury beta tag，非 KB 落后） |
| version_audit unknown | **1 → 0**（UnlitWF 日期编码解析修复后判 `same`） |
| 换行（CRLF/LF） | 78 个被改文件**零翻转**；全库 CRLF 319 / LF 101 / MIXED 1 |
| 越界修改 | 0（新增 5 页全部落在 world-tools 写范围；无文件删除） |
| frontmatter 合规 | 78 个被改文件中 72 个日期已更新为 2026-11-10；6 个按「机械修正不盖复核日期」惯例保持原日期（已登记，见 §7.3） |
| 事实性错误 | 发现 **1 条**（lilToon 版本历史 2.1.6 → 应为 2.1.5），其余抽检项与官方源一致 |

---

## 1. 任务完成状态表

| 任务 | owner | 范围 | 状态 | 主要交付 |
|---|---|---|---|---|
| task-1 | sdk-core2 | entities/api、entities/vrchatsdk、concepts/rules、world/udon/data-containers | ✅ completed | 24 页；新建 `entities/api/vrctween.md`；3.10.4/5/6 Udon/API 面 |
| task-2 | avatar-tools2 | entities/avatar（除 shader/） | ✅ completed | 16 页；MA/AOI/VRCFury/Meshia/TTT/LAC 版本 + Contacts/Constraints Avatar 侧 |
| task-3 | shader-tools2 | entities/avatar/shader、entities/world/shader | ✅ completed | 26 页；lilToon 2.4.1 / ORL 7.3.1 / UnlitWF 20261101 / Graphlit 2.7.4 |
| task-4 | world-tools | entities/world（除 shader/ 等） | ✅ completed | 工具版本 5 项 + 新建 5 个 3.10.5/3.10.6 组件页 |
| task-5 | verifier | _curator_tools、auxiliary-script | ✅ 本报告 | 基线、脚本修、二次独立验证、本审计 |
| task-6 | shader-tools2 | comparisons/ | ✅ completed | ORL/lilToon 过期版本引用收尾 |
| task-7 | avatar-tools2 | concepts/hybrid、entities/avatar/contact.md、api/official-doc-clarifications.md | ✅ completed | `OnContactStay` 不存在（8 处）纠错 |

**P0/P1/P2 视角**

| 优先级 | 内容 | 状态 |
|---|---|---|
| P0 | 10 个过期工具版本复核（MA/AOI/VRCFury/Meshia/TTT/LAC/lilToon/ORL/UnlitWF/Graphlit + LV/VVMW/UVU/ULocalization/VRCX） | ✅ 全部落地 |
| P1 | SDK 3.10.4/3.10.5/3.10.6 新能力入文（VRCTween/Box Contacts/约束 source API/SmoothingFactor/5 个新组件/Outline Renderers） | ✅ 全部落地 |
| P1 | 事实纠错（OnContactStay 不存在、GetSourceCount/SetSourceWeight 不存在、3.10.3 快照不冒充 3.10.6） | ✅ 全部落地 |
| P2 | 死链治理、related 格式债、world 侧移植残留目录 | ⏸ **本轮不动**，见 §8 遗留项 |

---

## 2. 工具版本：旧 → 新（含核证锚点）

> 「a13 复核」= 本轮我用 `version_audit_script.py` 直连 GitHub API 得到的最新 release；「tag 日期」= GitHub release 页面正文日期。

| 工具 | KB 旧 | KB 新 | 官方最新 | 核证来源 | 判定 |
|---|---|---|---|---|---|
| Modular Avatar | 1.17.1 | **1.18.7** | 1.18.7 | a13 API + releases 列表 | ✅ |
| AvatarOptimizer | 1.9.14 | **v1.9.21** | v1.9.21 (2026-10-30) | a13 API | ✅ |
| VRCFury | v1.1341.0 | **v1.1442.0** (stable) | 1.1442.1-beta.15（beta 通道） | a13 API + KB 双轨记录 | ✅（见 §4.1） |
| Meshia | 3.2.0 | **3.3.1** | 3.3.1 (2026-11-04) | a13 API + release 正文（KD-tree off-by-one） | ✅ |
| TexTransTool | v1.0.1 | **v1.0.2** | v1.0.2 (2026-06-26) | a13 API + release 正文（TextureBlender 内存泄漏 #1102） | ✅ |
| LAC | v0.8.0 | **v0.9.0** | v0.9.0 (2026-07-17) | a13 API | ✅ |
| lilToon | 2.3.3 | **2.4.1** | 2.4.1 (2026-10-20) | a13 API + `Assets/lilToon/CHANGELOG.md` 全文 | ✅ |
| ORL Shaders | v7.2.0 | **v7.3.1** | v7.3.1 (2026-07-11) | GitHub tag 页正文（LTCGI 屏修复） | ✅ |
| UnlitWF | UnlitWF_Shader_20260208 | **UnlitWF_Shader_20261101** | 20261101 (2026-11-01) | GitHub tag 页 CHANGELOG（见 §2.1） | ✅ |
| Graphlit | v2.7.2 | **v2.7.4** | v2.7.4 (2026-09-11) | GitHub tag 页正文（Linux 纹理打包 #8） | ✅ |
| VRCLightVolumes | v2.1.3 | **v3.0.1** | v3.0.1 (2026-10-14) | a13 API（大版本 2→3） | ✅ |
| VizVid (VVMW) | 1.7.5 | **1.8.3** | 1.8.3 (2026-11-07) | a13 API | ✅ |
| UdonVoiceUtils | 5.1.4 | **7.0.0** | 7.0.0 | a13 API（跨两个大版本） | ✅ |
| ULocalization | v0.8.16 | **v0.8.17** | v0.8.17 | a13 API（KB 无专页，仅脚本口径同步） | ✅ |
| VRCX | v2026.05.03 | **v2026.09.16** | v2026.09.16 | a13 API（KB 落在 platform 目录，本轮未改） | ✅ |

### 2.1 UnlitWF 20261101 官方 CHANGELOG（我直接从 tag 页抓取，作为核证锚点）

- Added（Shader）：主贴图 2nd 增加混合模式（加算/乘算/屏幕等）；Overlay 贴图混合模式增加「焼き込みリニア」。
- Changed（Shader）：**支持 VRC Light Volumes 3.0.1**；Changed（Tool）：lilToon → UnlitWF 材质转换时考虑 Main Texture 2nd。
- Fixed（Shader）：`WF_UnToon_Custom_ClearCoat_Transparent` 及部分 URP 系 shader 因 keyword 定义缺失导致渲染不良。

> 交叉印证：UnlitWF 20261101 声称支持 VRCLightVolumes **3.0.1**，与 world-tools 把 VRCLightVolumes 抬到 v3.0.1 的结论一致（两条独立证据互洽）。

---

## 3. SDK 3.10.4 / 3.10.5 / 3.10.6 覆盖矩阵

> 官方锚点已由我逐页抓取核验（`creators.vrchat.com/releases/release-3-10-{4,5,6}/`，全部 HTTP 200 全量正文）；下表「落位」列是本轮实际写入的页面（我复核过页面里确有对应条目）。

| SDK 增量 | 版本 | 官方锚点 | 落位页 | 状态 |
|---|---|---|---|---|
| VRCTween / VRCTweenHandle（DOTween **select** functionality） | 3.10.4 | RN 3.10.4 Features | `entities/api/vrctween.md` + `entities/world/udon/vrctween/`（7 页） | ✅ |
| Contacts Box 形状（宽/高/深独立、face proximity） | 3.10.4 | RN 3.10.4 Features + 官方 Contacts 页 | `entities/avatar/contact.md` | ✅ |
| Avatar 侧 global PhysBone collider（最多 4 个、plane 不支持） | 3.10.4 | RN 3.10.4 Features | avatar 侧 PhysBone 页 / `concepts` | ✅ |
| World 侧 VRCPhysBoneCollider → Udon + `ApplyConfigurationChanges()` | 3.10.4 | RN 3.10.4 Features | `entities/api/dynamics.md` | ✅ |
| DataList/DataDictionary 自定义 capacity + `EnsureCapacity()` | 3.10.4 | RN 3.10.4 Fixes | `entities/world/udon/data-containers/*`（5 页） | ✅ |
| VRCQualitySettings.RealtimeReflectionProbes / ShadowmaskMode | 3.10.5 | RN 3.10.5 Features | `entities/world/vrc-quality-settings.md` 等 | ✅ |
| AudioLink for Toon Standard | 3.10.5 | RN 3.10.5 Features | shader 侧（lilToon AudioLink 页） | ✅ |
| UdonSharp Assembly Version Defines | 3.10.5 | RN 3.10.5 Features | `concepts/rules/udonsharp-language-limits.md` | ✅ |
| VRCPickup Outline Renderers | 3.10.5 | RN 3.10.5 Features | `entities/api/pickups.md` | ✅ |
| VRCRaycast Root Transform / 禁用按未命中 | 3.10.5 | RN 3.10.5 Features+Fixes | world 侧 | ✅ |
| VRCJson 鲁棒性 + nested braces + DataToken int→long | 3.10.5 | RN 3.10.5 | `data-containers/vrcjson.md`、`data-tokens.md` | ✅ |
| Build & Reload → `VRC_SdkBuilder.ActiveBuildType` = Test | 3.10.5 | RN 3.10.5 Fixes | world 工具链页 | ✅ |
| ClientSim Play→Edit 不触发 Unity 事件 | 3.10.5 | RN 3.10.5 Fixes | ClientSim / rules | ✅ |
| PhysBone "Reset When Disabled" 也重置 animator 参数与 Udon state | 3.10.5 beta.2 | RN 3.10.5 | avatar PhysBone 页 | ✅ |
| **Constraints SmoothingFactor + source 方法组** | 3.10.6 | RN 3.10.6 + 官方 constraints-api 页 | `entities/api/dynamics.md`、`entities/avatar/vrc-constraints.md` | ✅ |
| Constraints 响应 C# 改 TargetTransform（play mode） | 3.10.6 | RN 3.10.6 | 同上 | ✅ |
| VRCBillboard（+ `_smoothingFactor` 归一化 slider） | 3.10.6 | RN 3.10.6 + 官方 `worlds/udon/vrcbillboard` | `entities/api/vrc-billboard.md` + `scene-components/vrc-billboard.md`（API/组件双页，索引已互链） | ✅ |
| VRCVelocityTracker（无自更新循环） | 3.10.6 | RN 3.10.6 + 官方 `worlds/components/vrc_velocitytracker` | `scene-components/vrc-velocitytracker.md` | ✅ |
| VRCPlacementZone（block / only-allow / unaffected） | 3.10.6 | RN 3.10.6 + 官方 `worlds/components/vrc_placementzone` | `scene-components/vrc-placementzone.md` | ✅ |
| Interact Outline Renderers（无 Udon accessor；空列表不再默认画 SkinnedMesh） | 3.10.5/3.10.6 | RN 3.10.5 + 3.10.6 | `scene-components/interact-outline-renderers.md` | ✅ |
| Contact Advanced Parameters / Material Contacts / `VRCContacts.cginc` | 3.10.6（beta.3 改名/扩展） | RN 3.10.6 + 官方 Contacts 页 | `entities/avatar/contact.md` | ✅ |
| Udon 编译器：enum 按值比较 / 命名参数跳过可选参数 | 3.10.6 | RN 3.10.6 Fixes | `concepts/rules/udonsharp-language-limits.md`（+ deep-pitfalls） | ✅ |
| null VRCUrl + persistence 同步 | 3.10.6 | RN 3.10.6 Fixes | `entities/api/persistence.md` | ✅ |
| VRCRaycast v1.1（Player layer / result transform 可选）+ client 2026.4.1 远程胶囊体 | 3.10.6 | RN 3.10.6 Features | world 侧（**按「引用的 re-test 注记」写，未当实测**） | ✅ |
| Native Face Tracking / expression Lerp·BitWidth·Representation | 3.10.6 | RN 3.10.6 | avatar 侧 | ✅ |
| **未在 release note 出现**：`WorldQualitySettings` 组件名、`UdonBehaviour.LastErrorReason`、`GetSourceCount()`/`SetSourceWeight()` | — | 三份 RN 全文检索无匹配 | 见 §5.3 处理方式 | ⚠️ 已按「需另引锚点/标 skip」处理 |

---

## 4. 验证数字：前后对比

### 4.1 对比表

| 指标 | 基线（本轮开工） | 终值 | 判定 |
|---|---|---|---|
| validation 文件数 | 415 | **421** | +6（新增页，非重复） |
| validation 通过 | 411/415 | **421/421** | ✅ |
| validation 失败 | 4（2×TOO_FEW_TAGS、2×INVALID_LEVEL） | **0** | ✅ |
| governance 死链（唯一） | 220 | **214** | ✅ 净减 6，无新增 |
| governance 孤立文档 | 3 | 3 | 持平 |
| governance 长尾文档 | 29 | 33 | 新页尚未被引用 |
| governance 高被引 | 153 | 155 | 持平 |
| a13 outdated | 14（历史口径）/ 7（限流口径） | **1** | ✅ |
| a13 unknown | 12（限流）/ 1（解析失败） | **0** | ✅ |
| a13 up-to-date | 1（限流口径） | **19** | ✅ |

### 4.2 必读口径说明（否则数字会误导）

**(a) VRCFury 判 outdated 是脚本口径，不是 KB 落后。**
a13 取的是 `releases/latest`，拿到的 `com.vrcfury.vrcfury/1.1442.1-beta.15` 是 **beta 预发布**；KB 记录的是 **stable `v1.1442.0`（2026-11-05）**，并在 `entities/avatar/vrcfury-reference.md:9` 写成双轨：
`"v1.1442.0 (2026-11-05 stable); beta 通道 v1.1442.1-beta.15 (2026-11-09)"` —— 记录方式正确。
**不建议据此改页面。** 建议后续给 a13 加 prerelease 过滤（`tag_name` 含 `-beta`/`-alpha`/`-rc` 时回退到上一个非预发布 tag）。

**(b) a13 的 TOOLS 表已同步为 KB 已复核版本，outdated 归零 = 脚本与 KB 口径一致，不代表工具不再更新。**
`version_audit_script.py` 的 `TOOLS` 第 3 字段是**硬编码的 kb_claimed_version**（不读页面 frontmatter）。本轮的「14 → 1」是两件事叠加的结果：
1. worker 把页面版本真正抬到最新；
2. 我把 TOOLS 表的期望值同步到「KB 已复核版本」。
所以这一行的语义是「**KB 与 GitHub 已对齐**」，不是「工具都停止更新」。真正判断「是否又过期」要等下一次 GitHub 发版后重跑。

**(c) governance 的 214 条不是「全是误报」**，必须按类型拆开（见 §6）。HANDOVER 里「220 条全是误报」的说法不成立。

---

## 5. 二次独立验证记录

方法：不读 worker 自检，只做两件事 —— ① 直接抓官方源与 KB 文本逐条对；② 用自建快照/SHA256 做字节级前后对比。抽样覆盖 4 个 worker，每个 ≥2 页（实际 11 页）。

### 5.1 sdk-core2（task-1）

**抽检页：`entities/api/dynamics.md`、`entities/api/vrctween.md`、三个 3.10.3 快照页**

| 核对点 | 我的独立结论 |
|---|---|
| 约束 source 方法名 `AddSource/RemoveSource/GetSource/SetSource/GetSources/SetSources/ClearSources/SourceCount` | ✅ 官方 `common-components/constraints/constraints-api`（HTTP 200，Last updated 2026-10-30）成员表逐项对应 |
| `GetSourceCount()` / `SetSourceWeight()` 不存在 | ✅ 我独立全文检索：各 **0 处**（与 KB 的「硬证据锚点」一致） |
| `ApplyConfigurationChanges()` 对 Constraint 适用 | ✅ 官方页有独立小节原文「You must call this after modifying the properties of a constraint via a script」——KB 的写法**没有**把 3.10.4 的 PhysBone 结论硬套到约束上 |
| SmoothingFactor 公式 `lerp(current, target, 1 - exp(-deltaTime / smoothingFactor))` 与「约每 N 秒到 63%」 | ✅ 与官方页**逐字**一致 |
| Sources 列表「仅前 16 个元素可被 Animator 定位」 | ✅ 官方页原文 "only the first sixteen elements" |
| VRCTween 控制方法 `Play/Pause/Kill/Complete/Restart/Flip/PlayBackwards/PlayForwards/Goto` + 查询属性 `IsValid/IsPlaying/IsActive/Elapsed/Duration/IsBackwards` + 设置 `OnComplete/OnRewind/SetLoops/SetDelay/SetUpdate/From/SetSpeedBased/SetEase/SetDuration/ChangeEndValue` | ✅ 与官方 `worlds/udon/vrctween/settings` **逐项**一致（含 OnRewind 不在 Restart 触发、SetUpdate 枚举 0-3、From 仅非虚拟 tween 且须立即调用） |
| 虚拟补间签名 `TweenFloat/TweenInt/TweenColor/TweenVector3/DelayedCall/DelayedSetActive` | ✅ 与官方 `virtual-tweens` 页签名一致（`(from,to,dur,this,nameof(field),nameof(cb),ease)`） |
| 输入验证（null target / 负或 NaN duration / ~520k 幅度 / path resolution clamp 1-50） | ✅ 官方主页 Input Validation 原文 |
| 「DOTween 只是 select functionality，不得假设 Sequence 等可用」 | ✅ 既符合 RN 措辞，也有明确「反面纪律」段；比「DOTween 全可用」这类常见误解更稳 |
| 三个 3.10.3 快照页（`udon-type-exposure` / `exposed-types` / `not-exposed`） | ✅ **标题保留 3.10.3、统计不换版本**，并写明「本轮未重新导出」「3.10.4–3.10.6 新增 API 不在本快照内」「不能反证新 API 未暴露」，另附 3.10.4/3.10.6 缺口清单。**Evidence Gate 通过** |
| `entities/world/udon/vrctween/index.md` 的「SDK 3.7+」冲突 | ✅ 已由 world-tools 更正为 3.10.4+ 并留「勘误」段（我复核原文仍在页内） |

### 5.2 avatar-tools2（task-2 / task-7）

| 核对点 | 我的独立结论 |
|---|---|
| `entities/avatar/contact.md` Box 形状「宽/高/深独立」 | ✅ 官方 Contacts 页：Box 用 `Size` 逐轴（X/Y/Z），与 RN 3.10.4「width/height/depth independently」互证 |
| `Use Face Proximity` 仅 Box、按 **+Z 面**算 | ✅ 官方原文 "For box-shaped Contact Receivers... distance to the positive Z face" |
| `{parameter}_SenderType` 0-4 编码 | ✅ 官方页 0=无/未识别、1=同 avatar、2=他人 avatar、3=world、4=spawned item |
| Advanced Parameters 八类（含 `SenderLossyScale`、`PointSeparation`、beta.3 语义） | ✅ 官方页 + RN 3.10.6 beta.3 一致 |
| **`OnContactStay` 不存在**（task-7 核心裁决） | ✅ 我独立抓官方 Contacts 页全文：`OnContactEnter` 2 处、`OnContactExit` 2 处、`OnContactStay` **0 处**；KB 现有引用全部是「不存在」纠错语境（无残余误用） |
| 版本号：MA 1.18.7 / AOI v1.9.21 / VRCFury v1.1442.0 / Meshia 3.3.1 / TTT v1.0.2 / LAC v0.9.0 | ✅ 与 a13 API 结果 + tag 日期一致；页面均给出 [FACT] 级锚点 |

### 5.3 shader-tools2（task-3 / task-6）

| 页面 | 我的独立核对 | 结论 |
|---|---|---|
| `entities/avatar/shader/liltoon/index.md` 版本历史表 | 对照 `Assets/lilToon/CHANGELOG.md` 全文：2.1.0「VRCLV 改逐像素」、2.1.10「Border 默认 3」、2.2.0「`_UdonForceSceneLighting`」、2.3.0「APV」、2.3.4「URP 附加光移入 pixel shader」、2.4.0「Minimal VRCLV 3.0 support」、2.4.1「light probe 亮度」 | ⚠️ **发现 1 处事实错误**：表中「毛发后于 **2.1.6** 跟进」应为 **2.1.5**（CHANGELOG 2.1.5：`Fur shader VRCLV calculation now performed in pixel shader`；2.1.6 是「VRCLV optimization / 兼容 VRCLV 1.0.0 / 边缘光上限提到 3」）。其余全部正确 |
| 同页「2.3.3 → 2.4.1 无属性重命名、无删除效果」 | 我逐条读 2.3.4 / 2.4.0 / 2.4.1 三个 CHANGELOG 段：均**无 Removed 段、无改名条目**（2.3.4 只有 Changed/Added/Fixed） | ✅ **反直觉结论成立** |
| `liltoon/troubleshooting.md` 10 行「已修复」表 | 逐行对 CHANGELOG：preset HDR 颜色、纹理复制粘贴、UV 设置不复制、custom shader 换行符、`lilToonSetting.json` 空、UI Toolkit 构建崩溃、invalid version check、Multi outline RenderQueue、`mainTexCheckWords` 'emission' typo、light probe 亮度 | ✅ 10/10 命中（含「changelog 原文」引用逐字正确） |
| `entities/avatar/shader/orl/overview.md` v7.3.0 变更表 | 对照 GitHub v7.3.0 release 正文（06 Jul）：Channel selectors、Decal 多混合模式、Emission UV tile/Rim Masking/Power、Rim Light Normal Map Influence、`%Priority()`、`%Linear()`、`%SkipAlwaysIncludedBlocks()`、`GLOBAL_uvArray`、VRCFeatures fallback、Alpha Scale -4~4（负值反相 albedo）、float2 UV 默认、`%OutlinePassModifiers` stencil、SSR Queue≤2500、Meta Pass、Layered Material 第 3 层、Texture3D 改写、移除多余 sampler | ✅ 全部命中；「release note 无 Breaking Changes 段」的表述也正确（我核实 v7.3.0 无该小节，故它把三条归为「行为变化」而非「破坏性变更」是**更稳**的写法） |
| `entities/world/shader/graphlit.md` 版本记录 | v2.7.4（2026-09-11）「Fixed texture packing on linux #8」✅；v2.7.3「Unity 6.5」与 v2.7.2 未逐条复核（页面自己已标注「未比对源码」） | ✅ 举证边界诚实 |
| task-6（comparisons/） | ORL/lilToon 过期版本引用已同步；`comparisons/filamented-comparison.md` 保持 LF | ✅ |
| UnlitWF 页 vs 官方 CHANGELOG | 主贴图 2nd 混合模式、Overlay 焼き込みリニア、VRC LightVolumes 3.0.1 支持、lilToon→UnlitWF 转换考虑 Main Texture 2nd、ClearCoat_Transparent/URP keyword 缺失修复 | ✅ 与官方 tag 页一致 |

### 5.4 world-tools（task-4）

| 核对点 | 我的独立结论 |
|---|---|
| 5 个新组件页 source URL 可达性 | ✅ `worlds/udon/vrc-billboard/` 200（标题 VRCBillboard）、`worlds/components/vrc_placementzone/` 200、`worlds/components/vrc_velocitytracker/` 200；`world-quality-settings.md` 已改为 `release-3-10-5`；`interact-outline-renderers.md` 用 3.10.5+3.10.6 release 页 |
| `ask.vrchat.com/t/developer-update-13-august-2026/48800`（sdk-core2 的 billboard API 页引用） | ⚠️ **本机不可达**（`fs-quality-fetch: ECONNRESET`），无法独立确认；不作为死链判定（见 §9.4） |
| 新页换行 | ✅ 5 个新页全部 **CRLF**，与库主流一致 |
| 索引挂链 | ✅ `scene-components/index.md:71-75` 已挂 5 条并带 ⭐ 版本标注；API/组件双页互链已写明；⚠️ `entities/world/index.md` 只挂到 `vrc-quality-settings`，未直达 5 个新页（P2 建议，非错误） |
| 工具版本 LV v3.0.1 / VVMW 1.8.3 / UVU 7.0.0 / ULocalization v0.8.17 / VRCX v2026.09.16 | ✅ a13 全部判 `same`；LV 3.0.1 与 UnlitWF 20261101 的支持声明互证 |

---

## 6. 死链：按类型拆分（不沿用「全是误报」）

我写了独立分类器（`auxiliary-script/_verifier_deadlink_split.py`，自己解析 frontmatter 与正文）。方法口径写在 §6.5，**与 Lead/shader-tools2 的 11/46 数字有差异，原因是口径不同**，不是彼此矛盾。

### 6.1 四类分解（我实测）

| 类型 | 数量 | 性质 | 处理 |
|---|---|---|---|
| A. `[[wikilink]]` 被 governance 当路径拼 → 误报 | ~180（总数 214 的绝对主体） | **误报**（脚本不识别 wikilink） | 不改，脚本层面已记录原因 |
| B. `related:` 里残留 `[[...]]` 写法（目标存在） | **150 条目标存在** / 27 条目标缺失或套嵌错（如 `../../[[../hybrid/index.md]]`） | **格式不规范** | 本轮不动（会牵动大量页面） |
| C. `related:` 里是中文描述而非路径（如「源库：Sources 目录（见 raw/）」） | **3 条**（`FACT.md`、`index.md`、`entities/world/vvmw.md`） | 不是死链，是 frontmatter 脏数据 | 本轮不动 |
| D. `related:` 纯相对路径但目标不存在 | **47 条**（去掉同 basename 兜底后 8 条） | **真断链** | 本轮**不修**，见 §8 |
| E. 正文 markdown 真断链 | **0 条**（现存 7 条命中均为伪链接：`exposed-types.md` 里 6 处 `[Boolean](Boolean)`/`[List`1[T]](...)` 这类类型名，`teaching-methodology.md` 1 处乱码目标） | 已由 shader-tools2 修完 shader 侧 8 条 | 遗留伪链接见 §8 |

### 6.2 D 类（47 条）分区

| 区域 | 条数 | 典型目标（多为移植残留目录） |
|---|---|---|
| `entities/world/` | **32** | `systems/`(6)、`obstacle-course/`(6)、`persistence/`(3)、`drones/`(3)、`tools/`(2)、`patterns/`(3)、`udon-example-scene/`(2)、`udon/world-debug-views.md` 等 |
| `concepts/patterns/` | **9** | 5 条写成 `FACT.md`（应为 `../../FACT.md`）、2 条 `../../entities/...index.md]]`（尾随 `]]`） |
| `concepts/hybrid/index.md` | 1 | `../../entities/api/player-api.md]]`（尾随 `]]`） |
| `entities/world/index.md` | 1 | `../../index]]`（尾随 `]]`） |
| `FACT.md` / `index.md` / `vvmw.md` | 3 | 中文描述项（同 C 类） |
| `entities/avatar/physbones.md` | 1 | `quest-constraints.md`（页面不存在） |
| `meta/network-search-protocol.md` | 1 | `../../SOUL.md`（仓库外文件） |

### 6.3 本轮是否引入新断链？

- governance 总数 **220 → 214**（净减 6），**没有任何指标上升**，故**净新增为 0**。
- 逐条看：47 条 D 类里能定位到「本轮被改文件」的只有 `entities/world/index.md`、`vvmw.md` 两条，且都是「尾随 `]]`」/「中文描述」这类**旧格式债**（不是新写的路径）。
- ⚠️ 但要注意：**我的字节级快照抓取时刻约 04:12**，此前已被改的约 13 个文件（`vrctween.md` 新建 + liltoon 11 页 + `modular-avatar.md`）无法做前后对比，只能靠内容核对。**这是一个真实的复验盲区**，见 §8.4。

### 6.4 修复建议（不夹带进本轮）

- 不补建 `systems/`、`obstacle-course/` 等目录（会凭空造结构，且这些页是从别处移植的残影）。
- 不批量删 `related`（会丢语义关系）。
- 建议单独立项「related 字段归一化」：把 `[[...]]` 统一转成纯路径或统一转成 wikilink（二选一），并清掉中文描述项；`FACT.md` / 尾随 `]]` 这类属**低风险机械修正**，可一次性脚本处理 + 人工抽检。

### 6.5 口径差异说明（与 Lead 的 11/46）

| 口径 | Lead/shader-tools2 | 我（verifier） |
|---|---|---|
| 是否把「同 basename 在别处存在」当有效 | 否（严格路径） | 是（Obsidian 习惯兜底）→ 47 vs 46 |
| 是否把 `[[...]]` 目标缺失计入「真断链」 | 部分计入 | 单列为 B 类（27 条） |
| 正文伪链接（类型名/乱码） | 可能计为断链 | 单列为 E 类伪链接（7 条） |
| 结论 | 11 markdown + 46 related | **0 真 markdown 断链（8 条已修）+ 47 related 严格缺失 + 150 格式噪声** |

两套数字**都能复现**，差别只在分类边界；本报告以我的口径为准并保留对照。

---

## 7. 三查：frontmatter / 换行 / 越界

### 7.1 frontmatter（78 个被改文件全扫）

- 72 个：`last_review` = `updated` = **2026-11-10**，tags 全部 3–8，必填字段齐全。
- 6 个按「机械修正不盖复核日期」惯例**保持原日期**（见 §7.3 登记）。

### 7.2 换行（字节级，含新增文件）

| 指标 | 数值 |
|---|---|
| 全库（421 文件） | **CRLF 319 / LF-only 101 / MIXED 1** |
| MIXED | 仅 `entities/vrchatsdk/index.md`（本轮前即 mixed，基线既有） |
| 78 个被改文件的 **EOL 翻转** | **0**（无一个 CRLF↔LF 互转） |
| 5 个新增文件 | 全部 CRLF |
| Lead 点名的 4 个 LF 文件 | `comparisons/filamented-comparison.md`(LF 176)、`shader/filamented/overview.md`(LF 125)、`shader/filamented/pbr-improvements.md`(LF 170)、`shader/scss.md`(LF 350) —— **全部在 101 个 LF 名单内，且改后仍 LF** |
| 6 个新建 vrctween 教程页 | **LF-only**（`world/udon/vrctween/{basics,control,custom-tween,patterns,sequence,tween-types}.md`），而同目录 `index.md` 是 CRLF → **同目录混排**，建议后续统一（新文件不构成回归，仅风格不一致） |

### 7.3 越界与「机械修正」登记

- **越界修改：0**。5 个新增文件全部在 `entities/world/scene-components/`（world-tools 写范围）；无文件删除；`_curator_tools/` 下只新增我自己的报告与工具数据，无 worker 越界写入。
- **按惯例保持原日期、需在此登记的机械修正（6 处）**：

| 文件 | 原日期 | 改了什麼 | 修改方 |
|---|---|---|---|
| `comparisons/build-time-vs-runtime.md` | 2026-06-20 | 链接路径修正（内容未复核） | shader-tools2（task-6） |
| `comparisons/index.md` | 2026-08-09 | tags 补 `index`（2 → 3，修 TOO_FEW_TAGS） | **verifier（我）** |
| `queries/index.md` | 2026-08-09 | tags 补 `index`（2 → 3，修 TOO_FEW_TAGS） | **verifier（我）** |
| `entities/avatar/shader/poiyomi/shader-locking-mechanism.md` | 2026-07-04 | 链接/版本引用机械修正 | shader-tools2 |
| `entities/avatar/shader/scss.md` | 2026-06-21 | 链接路径修正（内容未复核） | shader-tools2 |
| `entities/world/scene-components/index.md` | 2026-08-17 | 挂 5 个新页链接 + 覆盖表更新 | world-tools |

> ⚠️ 最后一条是我的**判断异议**：`scene-components/index.md` 这次不是「纯机械修正」，而是**新增了 5 条导航与覆盖说明**，按惯例 ③ 应判为内容变更 → 建议补 `last_review/updated = 2026-11-10`（或由 Lead 明确裁定「索引挂链算机械」）。我未越权修改，仅报告。

---

## 8. 遗留项清单

| # | 项 | 严重度 | 建议 |
|---|---|---|---|
| 8.1 | lilToon 版本历史表「毛发后于 2.1.6 跟进」应为 **2.1.5** | **P2（事实错误）** | 交 shader-tools2 或 Lead 定点修 1 行 |
| 8.2 | `related:` 格式债：150 条 `[[...]]` 目标存在但写法不规范 / 47 条严格缺失 / 3 条中文描述项 | P2 | 单独立项，**不批量删、不补建目录** |
| 8.3 | 正文 7 条伪链接（`exposed-types.md` 6 处类型名、`teaching-methodology.md` 1 处乱码目标） | P3 | 低风险清理，可随下次内容更新做 |
| 8.4 | **复验盲区**：字节快照抓于约 04:12，此前已改的约 13 页无法做前后字节对比 | P2（方法论） | 下次团队开工**第一件事**先抓快照（本次教训） |
| 8.5 | `entities/world/index.md` 未直达 5 个新组件页（只到 `vrc-quality-settings`） | P3 | 建议补索引行 |
| 8.6 | 同目录换行混排：`world/udon/vrctween/`（index CRLF，6 个子页 LF） | P3 | 统一为 LF 或 CRLF（择一并写入 protocol） |
| 8.7 | `entities/world/scene-components/index.md` 日期未更新（§7.3 异议） | P2（合规判断） | 由 Lead 裁定 |
| 8.8 | a13 未过滤 prerelease，导致 VRCFury 永远显示 outdated | P2（脚本） | 建议后续给 `version_audit_script.py` 加 prerelease 过滤（本次**未改**，避免越权扩语义） |
| 8.9 | `version_audit_script.py` 的 `days_since()` 硬编码基线日 `2026-06-21`，日期口径不是「今天」 | P3（脚本） | 顺手可改为 `date.today()`，本次未改 |
| 8.10 | `entities/api/vrc-billboard.md` 的 source 含 `ask.vrchat.com`（本机不可达） | P3 | 保留但在报告登记不可核证 |

---

## 9. 环境限制（本轮实测，务必随报告保留）

1. **`creators.vrchat.com` 的 Docusaurus 404 陷阱**：错误路径也会返回 **HTTP 200 + "Page Not Found" 正文**，必须看正文而不是状态码。正确路径：约束 API 是 `common-components/constraints/constraints-api`（`worlds/udon/vrc-constraints/` 是 404）；VRCTween 是 `worlds/udon/vrctween/`（`common-components/vrctween/` 是 404）。
2. **同一 URL 不同 fetch 后端结果可能不同**：`raw.githubusercontent.com` 走 `fs-quality-fetch` 后端失败（worker 报）、走 `web_fetch` 直连 200（Lead 实测）。GitHub 侧我一律走 `github.com` HTML 页 / `api.github.com`，并全部二次核对正文。
3. **GitHub API 未认证限流 60/h**：本轮开工时 12/20 个工具 403，导致 a13 首跑出现 `unknown 12`。额度在 UTC 整点重置后复跑成功。**任何「查询失败」结论都要先看是否 403**。
4. **`github.com` HTML 抓取曾 30s 超时**（本机代理恢复后 200/约 7-10s）；`ask.vrchat.com` 至今不可达（`ECONNRESET`）。
5. **GitHub Pages 域名**（`lilxyzw.github.io`、`z3y.github.io` 等）本轮未单独验证；若将来 `url_health_check.py` 标红，**不定性为死链**。
6. **`url_health_check.py` 本轮未跑**：约 3.5 分钟 + 420 个超时误判的已知噪声，性价比不足；死链结论以 governance + 我的分类器为准。
7. **无 git、无备份**：`D:\UDON` 不是 git 仓库，全库无 zip/bak 快照，**字节级前后对比只能靠自建快照**。

---

## 10. 本轮 verifier 自身改动登记（审计留痕，防止"谁改了什么"丢失）

| 文件 | 改动 | 理由 | 换行 |
|---|---|---|---|
| `auxiliary-script/validation_script.py` | `ALLOWED_LEVELS` 增加 `'governance'` + 2 行注释 | meta/ 两页 `knowledge_level: governance` 是库演进，脚本白名单滞后（**TOO_FEW_TAGS 阈值未动**） | CRLF 保持 |
| `auxiliary-script/version_audit_script.py` | ① `TOOLS` 表 15 项 kb_claimed_version 同步为 KB 已复核版本；② `normalize_version()` 增加 8 位日期编码回退；③ `audit_one_tool()` 区分 `unknown_kind=query/parse` 并给解析失败填可读 error；④ 报告层「查询失败」改为「查询结果不可用」+ 说明 | Lead 授权；只修解析与口径，**未放宽任何判定语义** | CRLF 保持（399/0） |
| `comparisons/index.md`、`queries/index.md` | tags 各 +1（`index`） | 唯一授权的两处页面修正（修 TOO_FEW_TAGS） | 原为 LF，改后仍 LF |
| `auxiliary-script/_verifier_diff_snapshot.py`（新） | 快照比对工具：SHA256 + CRLF/LF 计数 + 增删改 + EOL 翻转检测 | 本机无 git，需自建前后对比 | LF 新建 |
| `auxiliary-script/_verifier_frontmatter_scan.py`（新） | 被改文件 frontmatter 合规扫描（日期/tags/必填） | 复验自动化 | LF 新建 |
| `auxiliary-script/_verifier_deadlink_split.py`（新） | 死链分类（wikilink 误报 vs 真断链 vs 格式噪声） | 拆开「214 条」这团账 | LF 新建 |
| `_curator_tools/_verifier_baseline_snapshot.json`（新） | 416 文件基线（bytes/CRLF/LF/SHA256/mtime） | 前后对比唯一基准 | JSON |
| `_curator_tools/_verifier_sdk3106_oracle.md`（新） | 3.10.4/3.10.5/3.10.6 官方事实清单 + 覆盖矩阵 + 反例清单 | 对账 oracle | LF 新建 |
| `_curator_tools/a4_validation_report.md` / `a5_governance_report.md` / `a13_version_audit_report.md` | 由脚本重新生成（覆盖） | 脚本产出 | 脚本自定 |

---

## 11. 复验命令（任何人可重跑）

```powershell
cd D:\UDON\memoryV5.0.0\LLMWikiExample\auxiliary-script
Get-Process python -ErrorAction SilentlyContinue   # 先确认无并发实例
python validation_script.py      # 期望 421/421 passed, 0 failed
python governance_script.py      # 期望 死链 214 / 孤立 3 / 长尾 33 / 高被引 155
python version_audit_script.py   # 期望 up-to-date 19 / outdated 1(VRCFury beta) / unknown 0（需 GitHub 配额）
python _verifier_diff_snapshot.py        # 期望 EOL FLIPS 0
python _verifier_frontmatter_scan.py     # 期望 除 6 个已登记机械修正外全 ok
python _verifier_deadlink_split.py       # 期望 47 条 strict related 缺失
```

---

## 12. 一句话结论

**SDK 3.10.4/3.10.5/3.10.6 的官方增量已完整落到对应页面并带可复现锚点；14 个过期工具全部追平；validation 从 4 个失败变为 0 失败、死链净减 6 且无新增、换行零翻转。唯一事实错误是 lilToon 版本历史里 2.1.6→2.1.5 的一处笔误；另有 related 格式债与 world 侧移植残留目录属基线遗留，建议单独立项而非夹带进本轮。**

---

## 13. Lead 收尾处置（2026-11-10 06:1x，在本报告交付后）

> 本节由 Lead 追加，记录对 §0 / §7.3 中两项未决事项的**最终处置**，以及处置后重新跑的三件套结果。

### 13.1 唯一事实错误：已修（2 处，不止 1 处）

verifier 报的 `liltoon/index.md` 2.1.6 → 2.1.5 **成立**，我按官方 CHANGELOG 原文复核：

```
## [2.1.6] - 2025-08-05
### Changed
- VRCLV optimization
- Compatible with VRCLV 1.0.0
- Increased the upper limit of the VRCLV rim light range to 3

## [2.1.5] - 2025-08-04
### Changed
- Fur shader VRCLV calculation now performed in pixel shader
- Shader optimization
```

**「毛发 VRCLV 改逐像素」确实属于 2.1.5，verifier 判断正确。** 但同一错误其实出现了**两处**，第二处在 verifier 的抽检路径之外：

| 文件 | 原写法 | 已修为 |
|---|---|---|
| `liltoon/index.md:101` | 「2.1.0，毛发后于 **2.1.6** 跟进」 | 「2.1.0；毛发着色器于 **2.1.5** 跟进，2.1.6 为 VRCLV optimization」 |
| `liltoon/vrchat-specific.md:83` | 「2.1.6 | 兼容 VRCLV 1.0.0；边缘光上限 3；**毛发的 VRCLV 计算也移到像素着色器**」 | 拆为两行：`2.1.5`（毛发改像素着色器）+ `2.1.6`（VRCLV optimization / 兼容 1.0.0 / 边缘光上限 3） |

> 这条说明：**抽检命中 1 处 ≠ 全库只有 1 处**。同一事实错误在多个页面重复出现，是本轮反复验证过的模式（此前 LTCGI/VRCLV 也曾在 3 处扩散）。

### 13.2 合规异议：verifier 判断正确，已按建议改

`entities/world/scene-components/index.md` 新增 5 条导航 + 覆盖表更新，**不属于「纯机械修正」**，verifier 的异议成立。已处置：

- `version: 1.1 → 1.2`
- `last_review: 2026-08-17 → 2026-11-10`
- `updated: 2026-08-17 → 2026-11-10`
- 同时把这 5 个新页补进该页 frontmatter 的 `related:` 列表（原先只有正文表格挂链，related 未同步）

**凡「新增/删除导航条目、改动表格覆盖范围」一律视为内容变更，必须盖复核日期** —— 此前我给的「索引挂链算机械」的隐含假设不成立，以本条为准。

### 13.3 重跑三件套（Lead 处置后）

| 指标 | a14 交付时 | Lead 处置后 | 说明 |
|---|---|---|---|
| validation | 421/421，0 failed | **421/421，0 failed** | 未回退 |
| governance 死链 | 214 | **214** | 无新增（新增的 5 条 related 全部命中已存在文件） |
| governance 长尾 | 33 | **28** | ↓5，因 5 个新页被 related 引用后脱离长尾 |
| governance 孤立 | 3 | **3** | 持平 |
| 换行 | 零翻转 | **零翻转** | 三个改动文件 CRLF 保持：127/0、194/0、214/0 |

### 13.4 对 §8 遗留项的分级确认

- **related 字段归一化**（150 条格式噪声 + 47 条严格缺失 + 3 条中文脏数据）：**确认为独立立项**，本轮不夹带。建议做法：先只做「`[[...]]` 残留去除」这一零风险子集，再评估缺失项。
- **world 侧移植残留目录**（`systems/` / `obstacle-course/` / `drones/` / `tools/` / `patterns/` 32 条）：**不补建目录、不批量删 related**。理由：补建会污染结构，批量删会丢语义关系。
- **P3 伪链接**（`entities/api` 6 条 + `teaching-methodology` 1 条）：属扫描器误报范畴，基线既有，不修。

### 13.5 Lead 自身改动登记（对应 §10 留痕原则）

| 文件 | 改动 | 原因 |
|---|---|---|
| `liltoon/index.md` | 2.1.6 → 2.1.5（1 行） | §13.1 事实错误 |
| `liltoon/vrchat-specific.md` | 2.1.6 行拆为 2.1.5 + 2.1.6（1 行 → 2 行） | §13.1 同一错误第二处 |
| `scene-components/index.md` | version/last_review/updated + related 补 5 条 | §13.2 合规异议 |
| `FACT.md` | 新增「VRChat SDK 版本基线」整节 + 工具链现行版本表 + 约束/Contacts 规格 | Lead 收尾（会话前期） |
| `index.md`、`_always-load.md` | 版本号、日期、VRCTween 版本勘误、5 条 SDK 3.10.6 路由行 | Lead 收尾（会话前期） |

### 13.6 最终结论修订

原 §12 结论**在事实层面成立**，但需补两点：**（1）lilToon 的事实错误实为 2 处而非 1 处，已全部修复；（2）`scene-components/index.md` 的复核日期已按 verifier 异议补正。** 处置后三件套数字未回退，死链仍为 214 无新增，长尾因新页挂链净减 5。
