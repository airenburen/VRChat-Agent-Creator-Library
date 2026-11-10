---
title: "verifier oracle — VRChat SDK 3.10.4 / 3.10.5 / 3.10.6 官方事实清单"
category: meta
knowledge_level: auxiliary
status: active
tags: [oracle, verifier, sdk-3.10.6]
source: 官方 release notes
version: 1.0
last_review: 2026-11-10
---

# 复验基准：SDK 3.10.4 / 3.10.5 / 3.10.6 官方事实清单

> 由 verifier (task-5) 直接从官方 release notes 抽取，用于逐条对账 worker 改写内容。
> 用法：KB 里出现「本轮新增的 SDK 事实」时，必须能在本清单里找到对应条目；本清单没有的，要求页面给出别的可核证锚点（官方 API 文档 / 二进制 census / 实测记录）。
> 抓取时间：2026-11-10（本机 Asia/Shanghai）。

## 官方源与元数据

| 版本 | 发布日期（页面 meta） | URL |
|---|---|---|
| 3.10.4 | 2026-06-17 | https://creators.vrchat.com/releases/release-3-10-4/ |
| 3.10.5 | 2026-09-04 | https://creators.vrchat.com/releases/release-3-10-5/ |
| 3.10.6 | 2026-10-30 | https://creators.vrchat.com/releases/release-3-10-6/ |
| packages tag 3.10.6 | — | https://github.com/vrchat/packages/releases/tag/3.10.6 |

抓取限制（本轮环境）：
- `raw.githubusercontent.com` ECONNRESET，不可用。
- `api.github.com` 未认证 60 req/h，本轮已被 a13 version_audit 打满 → 403 rate limit；packages tag 页面正文未能抓到（github.com HTML 抓取 30s 超时）。
- 三个 creators.vrchat.com 页面均抓取成功（HTTP 200，全量正文）。

## SDK 3.10.4（2026-06-17）官方原文要点

1. **VRCTween**：把 DOTween 库的**部分**能力暴露给 Udon（原文 "exposes select functionality from the powerful DOTween library"）。可动画 position / rotation / scale 等；有 "virtual tweens" 概念（原文只说 advanced interpolation，**未给方法清单**）；`VRCTween.DelayedCall` 等价于 `SendCustomEventDelayedSeconds` 但返回可 `Kill()` 取消的 handle；兼容 UdonSharp 与 Udon Graph。
2. **Avatar 侧 global PhysBone colliders**：avatar 项目里可把 PhysBone collider 标记为 global，最多 **4 个额外的 global collider**；plane-shaped collider **不支持** global collision。
3. **Box-shaped Contacts**（avatar + world）：宽/高/深可**独立**调整；proximity 可按「距中心」或「距某个面」计算。
4. **World 侧 VRCPhysBoneCollider 暴露给 Udon**：可动态改属性；**改完必须调 `ApplyConfigurationChanges()`**。
5. Gizmo：avatar 手指预览从 tip-to-tip 改为 center-to-center（与 client 一致）。
6. 所有 SDK 项目强制 shader chunking 4MB。
7. **Data Containers**：`VRCDataList` / `VRCDataDictionary` 可用自定义 capacity 构造；`VRCDataDictionary` 新增 `EnsureCapacity()`。**原文未提 DataList 的 Capacity 属性**。
8. `UdonSharpBehaviour` 实现 `IUdonEventReceiver`（省掉 cast）。
9. UdonSharp 修复「特定成员数量下 enum 误编译」。
10. beta.2：修 VRCTween 在 Worlds SDK 的集成。beta.3：新增 `DelayedSetActive`、`TweenPitch`(AudioSource)、可预建 paused/zero-duration tween；多 avatar 场景下 Allow Self / Allow Others 按「每个 avatar 是独立玩家」处理。

## SDK 3.10.5（2026-09-04）官方原文要点

1. **AudioLink 支持 Toon Standard**（含移动端）；需世界里有 AudioLink prefab，否则可开 Fallback Mode；三种内置效果 Pulse / Scroll / Bar，可叠加 UV-based 模式。
2. **VRCQualitySettings.RealtimeReflectionProbes**：可从 Udon get/set，全平台可开关。
3. **VRCQualitySettings.ShadowmaskMode**：PC 上可开 "Distance Shadowmask"，从 Udon get/set（类型未在 release note 写明）。
4. **UdonSharp Assembly Version Defines**：可在 U# 代码里用 Assembly Definition 的 Version Define（例：`3.10` → `#if SDK_310_OR_NEWER`）。
5. **VRCPickup 新增 Outline Renderers 属性**（手动指定 hover 高亮的部件）。
6. **VRCRaycast 新增 Root Transform 属性**（射线起点可与组件所在 transform 不同）。
7. **Global PhysBone Collider 新增 Allow Self / Allow Others**（avatar 侧，可选不与自身/他人 avatar 的 PhysBone 碰撞）。
8. play mode 中 reload editor domain 不再让 PhysBones / Contacts 失效；ClientSim 网络工具 null ref 修复（domain reload 关闭时）。
9. Constraints 的 Activate / Zero 可在 inspector 撤销，并标记场景/prefab 未保存。
10. VRCRaycast 组件 disabled 时参数按「未命中」置位（以前保留旧值）。
11. **SDK 会剥离不认识的 `VRC_ENABLE_` 前缀 scripting define**。
12. **VRCJson**：反序列化鲁棒性改进；`[}` 这类错误闭合不再当合法；字符串里的 `[` `{` 正确处理。
13. **Pipeline Manager 验证**：上传前扫描重复 PipelineManager；同一 GameObject 上不能再有多个 PipelineManager。
14. 无 eye bone 的 humanoid avatar 头部 contact 位置/尺寸修复。
15. Add Components 菜单重组为 "VRChat" 分类，内部组件隐藏。
16. avatar 自动生成的 Contacts / PhysBone Colliders 会随整体移动旋转正确镜像，手动改过的 transform 会保留（完全缺失才重置），上传时校验。
17. UdonSharp 脚本创建对话框限制在 Assets 文件夹（beta.2：又恢复支持 Packages 文件夹；项目外创建给出明确错误）。
18. ClientSim：**切 Play→Edit 时不再触发 OnDisable() 等 Unity 事件**（更贴近 client 行为）。
19. ClientSim：persistence 连续保存不再并发写。
20. 「Build & Reload」在 `VRC_SdkBuilder.ActiveBuildType` 里正确标为 **Test**。
21. EditorOnly 对象改为场景加载**后**才移除。
22. UdonSharp 更新期间暂停 asset import。
23. **DataToken 存 int 无法转 long** 的问题修复。
24. beta.2：菜单 SubMenu→其它类型自动清空 sub menu 引用；无 description 的 avatar 上传失败修复；**VRCJson 嵌套花括号** `{ "x": { "y": "{" } }` 修复；**PhysBone "Reset When Disabled" 现在也重置 animator 参数与 Udon state**。
25. 官方质量设置页「read-only」与新增 setter 的矛盾：**release note 明确说可从 Udon 设置**（本清单以此为准，KB 若写 read-only 需标注冲突）。

## SDK 3.10.6（2026-10-30）官方原文要点

1. **Native Face Tracking**（本次主打）：Avatar Descriptor 新增 Face Tracking 区块；Unified Expressions / VRCFT 标准；跨多个 mesh 驱动 blendshape 与 viseme；Viseme Dampen；Layer Blendshapes Sets；Parameter Behaviors；网络同步不占 synced parameter 预算；桌面 Selfie Expression + Android + OSC `/tracking/face/v1`；新增 Face Tracking Operations / Forwarded Parameters 计入 Performance Rank。
2. **VRCBillboard 组件（world）**：让任意 transform 朝向玩家相机（桌面）/头部（VR）；也可**从 Udon 注册 transform**。`_smoothingFactor` 字段在 inspector 里是 0..1 归一化 slider（beta.3 改）。
3. **Contact receiver 新能力**：
   - **Advanced Proximity Parameters**（beta.3 改名 **Advanced Parameters**，且不再只对 Proximity 生效，对所有 receiver 类型生效）；
   - **Material Contacts**：把最近 8 个 contact sender 的 world-space transform 等数据写进最多 4 个 mesh renderer 的材质属性；shader 侧用 SDK 里的 `VRCContacts.cginc`（含 `VRC_CONTACT_BUFFER_SIZE` define）；
   - beta.3 新增 advanced 参数：**sender lossy scale**、**point separation**（sender/receiver 最近内点的世界空间距离）；两个 sender proximity 都为 1 时取 point separation 更小的；
   - receiver 可为材质写入指定**独立于 animator 参数名**的参数名；
   - beta.3 修 Material Contacts 在 "Cast Shadows" 关闭的 Renderer 上失效。
4. **Constraints Smoothing Factor**：>0 时约束平滑趋近目标而非瞬切。
5. **Constraints source 辅助方法**：新增 add / remove / get / set sources 系列方法，**Udon 里应优先用这些方法而不是 Sources 列表**；直接改列表有可靠性问题，但为向后兼容**仍对 Udon 白名单开放**。release note **未逐一列举方法名**（`AddSource/SetSource/GetSource/SourceCount/SmoothingFactor` 之类的名字需另有锚点）。
6. 约束对 **Target Transform 的 C# 侧改动**在 play mode 下也会响应（SDK 项目内）。
7. **VRCVelocityTracker 组件（world）**：从 Udon 读速度，**不跑自己的 update loop**。
8. **VRCPlacementZone 组件（world）**：控制用户 portal / sticker / sharing pedestal / world-attached item 的摆放区域；可按类型 **block / only-allow / unaffected**。
9. **VRCRaycast v1.1（avatar）**：已有 avatar 继续用 1.0 直到在 SDK 项目升级；**可命中其他玩家（Player layer）**；不再强制要求 result transform；**client 2026.4.1 起远程玩家碰撞体改为与本地玩家一致的 capsule**。
10. **Interact Outline Renderers**：实现 `Interact()` 的 UdonBehaviour 新增 Outline Renderers 属性，可指定 hover 时描边用哪些子 renderer；**Pickup 与 interactable 在列表为空时不再默认高亮 SkinnedMeshRenderer**（client 2026.4.1 起）。
11. Boolean avatar parameter 的 expression 恢复 **Value 选项**（3.10.2 曾移除）；SDK 会对该用法给 warning，仍推荐用 int 参数表示互斥状态。
12. contact 修复：无 volume 的 receiver 的 proximity 生成、极小半径 sphere sender 检测、极薄 box receiver 的检测与 proximity。
13. avatar 上传在无 description 时失败的问题修复。
14. DPID 下 bit-swizzled normal map 渲染修复。
15. UdonGraph 节点上 `?` 帮助链接修复。
16. SDK splash 分别链到 Android / iOS 指南。
17. particle system max particles = 0 时 SDK 与 client 的 performance rank 计算不一致修复。
18. avatar playable layer slot 配置错误新增 validation warning。
19. 非均匀缩放 transform 下 PhysBone 稳定性改进（仍不完全支持、不推荐）。
20. **null VRCUrl 在启用 persistence 的世界里同步时抛 Udon 异常**修复。
21. **Udon 编译器**：enum 按值比较 vs 按整数比较不一致修复（`(foo >= bar)` 以前 false，现在 true，无需 cast int）；**命名参数跳过可选参数**修复（`MyMethod(1, c:3)` 以前编译成 `MyMethod(1)` 静默丢参）。
22. beta.2：`ContactReceiver.SetParameter` 破坏性重命名回滚（有工具被搞坏）；"Auto-Fill Blendshapes" 不再填充被搜索过滤隐藏的 viseme key；world SDK 新增外部缩略图提醒对话框。
23. beta.3：Advanced Parameters 从 toggle 变 multiselect dropdown（只生成需要的参数）；"Face Tracking Meshes" 表头可拖放添加；`IsBasicFaceTrackingKey` C# API 也接受 FaceTrackingKey enum；修 Eye Gaze Mode 编辑器预览（以前误当 Bone Rotation）。
24. beta.4：expression parameter 三个新选项 —— **Lerp Duration**、**Bit Width**（<8 bit）、**Representation**（float unsigned 0..1 / int signed -128..127）。
25. beta.4 → live：**face tracking instruction 限制提升**（avatar 全域 512→2048；每 behaviour instruction 64→256、character 512→1024、token 128→256）。
26. Known issue：build 时偶发 "All pipe instances are busy"（Unity 问题，重启编辑器）。
27. **release note 里没有出现**（worker 若写必须另有锚点）：`WorldQualitySettings` 组件名、`UdonBehaviour.LastErrorReason`、`GetSourceCount()` / `SetSourceWeight()`（后者大概率为旧资料臆造，**release note 无此方法**）、`VRCBillboardFacing`/`VRCBillboard` 的 Udon 方法名清单、Contacts 的 "Use Face Proximity" 字段名。

## 3.10.4/3.10.5/3.10.6 覆盖矩阵（供 a14 报告复用）

| SDK 增量 | 版本 | 官方锚点 | KB 应落位 |
|---|---|---|---|
| VRCTween / VRCTweenHandle | 3.10.4 | RN 3.10.4 Features | entities/api/vrctween.md |
| Contacts box shape (w/h/d, face proximity) | 3.10.4 | RN 3.10.4 Features | entities/avatar/contact.md + world 侧 |
| World VRCPhysBoneCollider → Udon + ApplyConfigurationChanges | 3.10.4 | RN 3.10.4 Features | entities/api/dynamics.md 等 |
| DataList/DataDictionary 自定义 capacity + EnsureCapacity | 3.10.4 | RN 3.10.4 Fixes | world/udon/data-containers/* |
| Avatar global PhysBone collider (max 4, plane 不支持) | 3.10.4 | RN 3.10.4 Features | avatar 侧 PhysBone 页 |
| VRCQualitySettings.RealtimeReflectionProbes / ShadowmaskMode | 3.10.5 | RN 3.10.5 Features | world 侧 / api 侧 |
| Assembly Version Defines | 3.10.5 | RN 3.10.5 Features | concepts/rules/udonsharp-language-limits |
| VRCPickup Outline Renderers | 3.10.5 | RN 3.10.5 Features | entities/api/pickups.md |
| VRCRaycast Root Transform / disabled 按未命中 | 3.10.5 | RN 3.10.5 Features+Fixes | (world-tools 范围) |
| VRCJson 鲁棒性 + nested braces + DataToken int→long | 3.10.5 | RN 3.10.5 | data-containers/vrcjson.md, data-tokens.md |
| Build & Reload → Test build type | 3.10.5 | RN 3.10.5 Fixes | world 工具链页 |
| ClientSim Play→Edit 不触发 Unity 事件 | 3.10.5 | RN 3.10.5 Fixes | concepts/rules 或 ClientSim 页 |
| PhysBone Reset When Disabled 也重置 animator 参数与 Udon state | 3.10.5 beta.2 | RN 3.10.5 | avatar PhysBone 页 |
| Constraints Smoothing Factor + source 方法族 | 3.10.6 | RN 3.10.6 | entities/api/dynamics.md, entities/avatar/vrc-constraints.md |
| VRCBillboard | 3.10.6 | RN 3.10.6 Features | entities/api/vrc-billboard.md |
| VRCVelocityTracker | 3.10.6 | RN 3.10.6 Features | world 组件页 |
| VRCPlacementZone | 3.10.6 | RN 3.10.6 Features | world 组件页 |
| Interact Outline Renderers | 3.10.6 | RN 3.10.6 Fixes | entities/api 交互页 |
| Udon enum 比较 / 命名参数编译器修复 | 3.10.6 | RN 3.10.6 Fixes | concepts/rules/udonsharp-language-limits |
| null VRCUrl + persistence 同步修复 | 3.10.6 | RN 3.10.6 Fixes | api/persistence.md |
| Advanced Parameters / Material Contacts | 3.10.6 | RN 3.10.6 Features+beta.3 | entities/avatar/contact.md |
| VRCRaycast v1.1 (Player layer, optional result transform) | 3.10.6 | RN 3.10.6 Features | world-tools 范围 |
| Native Face Tracking / expression Bit Width / Lerp / Representation | 3.10.6 | RN 3.10.6 | avatar-tools 范围 / avatar 参数页 |

## 反例清单（KB 里出现即为错误，除非另有更强锚点）

- `GetSourceCount()` / `SetSourceWeight()` 作为 VRC Constraint 方法 —— 3.10.6 release note **没有**这两个名字（旧参考资料臆造）。
- 把 DOTween 全套 API 当作 Udon 可用 —— 官方只说 "select functionality"。
- 把 3.10.4 的 `ApplyConfigurationChanges()` 结论推广到 Constraint 组件而不引用 3.10.6 对应条目（约束侧的依据是 "constraints respond to Target Transform changes"，方法名需另证）。
- 宣称 `WorldQualitySettings` 是 3.10.5/3.10.6 release note 明写的新组件（release note 未提；若是从 SDK 包内脚本/Inspector 观察所得，必须写清来源）。
- 把 "client 2026.4.1 远程玩家胶囊体" 写成本轮实测 —— 官方 release note 只是声明，未重测。
