# A5 治理报告(死链 + 孤立文档)

**生成时间**: 2026-11-10
**扫描范围**: memory/ 全库(排除 _curator_tools/)
**总文档数**: 421

---

## 概览

| 指标 | 数量 |
|------|------|
| 总文档数 | 421 |
| 死链数(去重) | 214 |
| 真正孤立文档(被引=0 且 引=0) | 3 |
| 长尾文档(被引=0 但 主动引用) | 28 |
| 高被引文档(被引>5) | 155 |

---

## 死链清单(214 条)

| 源文档 | 死链目标 | 类型 |
|--------|----------|------|
| `FACT.md` | `[[_always-load]]` | related |
| `FACT.md` | `[[concepts/patterns/index.md]]` | related |
| `FACT.md` | `[[entities/avatar/avatar-dynamic-bone-limits.md]]` | related |
| `FACT.md` | `[[entities/avatar/avatar-fallback-system.md]]` | related |
| `FACT.md` | `[[entities/avatar/avatar-particle-system-limits.md]]` | related |
| `FACT.md` | `[[entities/avatar/full-body-tracking.md]]` | related |
| `FACT.md` | `[[entities/avatar/ik-2.0.md]]` | related |
| `FACT.md` | `[[entities/avatar/shader/poiyomi/shader-locking-mechanism.md]]` | related |
| `FACT.md` | `[[entities/world/community-labs.md]]` | related |
| `FACT.md` | `[[index]]` | related |
| `FACT.md` | `源库：Sources 目录（见 raw/）` | related |
| `_always-load.md` | `[[entities/avatar/shader/liltoon/index.md]]` | related |
| `_always-load.md` | `[[entities/avatar/shader/poiyomi/index.md]]` | related |
| `_always-load.md` | `[[entities/world/bakery/index.md]]` | related |
| `_always-load.md` | `[[entities/world/creator-economy.md]]` | related |
| `_always-load.md` | `[[entities/world/items.md]]` | related |
| `_always-load.md` | `[[entities/world/shader/index.md]]` | related |
| `_always-load.md` | `[[entities/world/supported-assets.md]]` | related |
| `concepts/hybrid/index.md` | `../../entities/api/player-api.md]]` | related |
| `concepts/hybrid/index.md` | `[[FACT]]` | related |
| `concepts/hybrid/index.md` | `[[alcom.md]]` | related |
| `concepts/hybrid/index.md` | `[[audio-link.md]]` | related |
| `concepts/hybrid/index.md` | `[[avatar-camera-plugins.md]]` | related |
| `concepts/hybrid/index.md` | `[[avatar-world-boundaries.md]]` | related |
| `concepts/hybrid/index.md` | `[[avatar-world-contact-bridge.md]]` | related |
| `concepts/hybrid/index.md` | `[[avatar-world-sync-model.md]]` | related |
| `concepts/hybrid/index.md` | `[[osc-protocol.md]]` | related |
| `concepts/hybrid/index.md` | `[[player-observation-boundaries.md]]` | related |
| `concepts/hybrid/index.md` | `[[player-world-trigger-bridge.md]]` | related |
| `concepts/hybrid/index.md` | `[[udon-world-plugins.md]]` | related |
| `concepts/hybrid/index.md` | `[[vcc.md]]` | related |
| `concepts/misc/index.md` | `[[accessibility-guide.md]]` | related |
| `concepts/misc/index.md` | `[[postprocessing-principles.md]]` | related |
| `concepts/misc/index.md` | `[[postprocessing-usage.md]]` | related |
| `concepts/patterns/index.md` | `../../[[../hybrid/index.md]]` | related |
| `concepts/patterns/index.md` | `../../[[../misc/index.md]]` | related |
| `concepts/patterns/index.md` | `../../entities/api/index.md]]` | related |
| `concepts/patterns/index.md` | `../../entities/avatar/index.md]]` | related |
| `concepts/patterns/index.md` | `[[unorthodox-patterns.md]]` | related |
| `concepts/rules/index.md` | `../../[[../patterns/index.md]]` | related |
| `concepts/rules/index.md` | `[[FACT]]` | related |
| `concepts/rules/index.md` | `[[networking-rules.md]]` | related |
| `concepts/rules/index.md` | `[[performance-rules.md]]` | related |
| `concepts/rules/index.md` | `[[quest-constraints.md]]` | related |
| `concepts/rules/index.md` | `[[runtime-texture-compression.md]]` | related |
| `concepts/rules/index.md` | `[[udonsharp-language-limits.md]]` | related |
| `entities/api/index.md` | `[[animator.md]]` | related |
| `entities/api/index.md` | `[[api-checker.md]]` | related |
| `entities/api/index.md` | `[[audio.md]]` | related |
| `entities/api/index.md` | `[[dynamics.md]]` | related |

*仅显示前 50 条,完整 214 条见 a5_dead_links_full.md*

---

## 真正孤立文档(3 个)

**定义**: 被引=0 且 引=0

| # | 文档 | category |
|---|------|----------|
| 1 | `_always-load.md` | misc |
| 2 | `meta/index.md` | meta |
| 3 | `meta/obsidian-governance.md` | meta |

---

## 长尾文档(28 个,被引=0)

| 文档 | 主动引用数 |
|------|----------|
| `entities/avatar/index.md` | 50 |
| `entities/vrchatsdk/index.md` | 17 |
| `concepts/hybrid/index.md` | 11 |
| `entities/world/examples/persistence/index.md` | 11 |
| `entities/world/udon/data-containers/performance-notes.md` | 9 |
| `entities/world/scene-components/unity-audio-filters.md` | 6 |
| `entities/world/scene-components/unity-constraints.md` | 6 |
| `entities/avatar/shader/unlitwf/index.md` | 5 |
| `entities/vrchatsdk/api-groups.md` | 5 |
| `entities/world/udon/midi/index.md` | 5 |
| `meta/auxiliary-scripts.md` | 5 |
| `concepts/rules/index.md` | 4 |
| `entities/world/scene-components/unity-environment-components.md` | 4 |
| `entities/world/udon/networking/patterns.md` | 4 |
| `entities/world/udon/persistence/patterns/index.md` | 4 |
| `entities/world/udon/udonsharp/index.md` | 4 |
| `concepts/misc/index.md` | 3 |
| `entities/platform/meta-quest-troubleshooting.md` | 3 |
| `entities/world/audio-steam.md` | 3 |
| `entities/world/data-containers.md` | 3 |
| `entities/world/patterns/index.md` | 3 |
| `entities/world/udon/networking/tools/index.md` | 3 |
| `entities/platform/account-registration-cn.md` | 2 |
| `entities/world/development/index.md` | 2 |
| `entities/world/examples/index.md` | 2 |
| `entities/world/patterns/dynamic-player-list.md` | 2 |
| `comparisons/index.md` | 1 |
| `queries/index.md` | 1 |

---

## 高被引文档(Top 20)

| 文档 | 被引数 |
|------|--------|
| `index.md` | 76 |
| `entities/world/whitelisted-world-components.md` | 39 |
| `entities/avatar/modular-avatar.md` | 33 |
| `entities/api/networking.md` | 32 |
| `entities/world/performance-guide.md` | 31 |
| `entities/avatar/performance-rank.md` | 24 |
| `entities/avatar/optimization-guide.md` | 22 |
| `entities/avatar/avatar-modding-guide.md` | 20 |
| `entities/avatar/animator-system.md` | 19 |
| `entities/api/not-exposed.md` | 18 |
| `entities/api/persistence.md` | 18 |
| `entities/avatar/playable-layers.md` | 17 |
| `entities/world/udon/index.md` | 17 |
| `FACT.md` | 16 |
| `entities/api/pickups.md` | 16 |
| `entities/world/udon/ai-navigation.md` | 16 |
| `entities/world/udon/animation-events.md` | 16 |
| `concepts/rules/udonsharp-language-limits.md` | 15 |
| `entities/api/dynamics.md` | 15 |
| `entities/world/udon/data-containers/byte-and-bit-operations.md` | 15 |

---

## 治理建议

### 🔴 死链(214 条)

- **行动**: 修复或删除死链引用
- **优先**: Tier A 核心文档的死链先修

### 🟡 真正孤立文档(3 个)

- **行动**: 评估是否有保留价值
- **选项**: (1) 建立引用关系 (2) 合并到父文档 (3) 标记 deprecated/archived

### 🟢 长尾文档(28 个)

- **行动**: 评估是否被遗漏引用
- **选项**: (1) 添加到相关文档的 related (2) 降低为 archived

