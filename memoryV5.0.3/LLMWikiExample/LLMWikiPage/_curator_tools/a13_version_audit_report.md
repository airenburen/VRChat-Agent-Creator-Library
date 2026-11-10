# A13 工具版本审计报告
**生成时间**: 2026-11-11
**审计工具数**: 20
**数据来源**: GitHub REST API (releases/latest + commits)
**判定规则**: 停滞阈值 365 天;版本按 major.minor.patch 段比较

---

## 总体概览

| 指标 | 数量 | 占比 |
|------|------|------|
| 总工具数 | 20 | 100% |
| 知识库最新或领先(latest/same) | 18 | 90% |
| 已过期(outdated) | 2 | 10% |
| 未知(查询失败/无法解析) | 0 | 0% |
| 同版本但已停滞(> 365 天) | 1 | - |

---

## 详细核对结果

| # | 工具名 | 知识库版本 | 最新版本 | 差异 | 上次 Commit | 距今天数 | 状态 |
|---|--------|------------|----------|------|-------------|----------|------|
| 1 | Modular Avatar | `1.18.7` | `1.18.7` | same | 2026-11-08 | 3d | ✅ |
| 2 | AvatarOptimizer | `v1.9.21` | `v1.9.22` | outdated | 2026-11-10 | 1d | 🔴 |
| 3 | VRCFury | `v1.1442.0` | `com.vrcfury.vrcfury/1.1445.0` | outdated | 2026-11-10 | 1d | 🔴 |
| 4 | Meshia | `3.3.1` | `3.3.1` | same | 2026-11-04 | 7d | ✅ |
| 5 | MA2BT | `v2.0.2` | `v2.0.2` | same | 2026-05-09 | 186d | ✅ |
| 6 | TexTransTool | `v1.0.2` | `v1.0.2` | same | 2026-06-26 | 138d | ✅ |
| 7 | LAC | `v0.9.0` | `v0.9.0` | same | 2026-10-28 | 14d | ✅ |
| 8 | lilToon | `2.4.1` | `2.4.1` | same | 2026-10-20 | 22d | ✅ |
| 9 | ORL Shaders | `v7.3.1` | `v7.3.1` | same | 2026-07-11 | 123d | ✅ |
| 10 | UnlitWF | `UnlitWF_Shader_20261101` | `UnlitWF_Shader_20261101` | same | 2026-10-30 | 12d | ✅ |
| 11 | Graphlit | `v2.7.4` | `v2.7.4` | same | 2026-09-16 | 56d | ✅ |
| 12 | AudioLink | `v3.1.2` | `3.1.2` | same | 2026-11-01 | 10d | ✅ |
| 13 | VizVid (VVMW) | `1.8.3` | `1.8.3` | same | 2026-11-07 | 4d | ✅ |
| 14 | UdonVoiceUtils | `7.0.0` | `7.0.0` | same | 2026-09-02 | 70d | ✅ |
| 15 | Sardinal | `v0.9.8` | `v0.9.8` | same | 2026-01-28 | 287d | ✅ |
| 16 | ULocalization | `v0.8.17` | `v0.8.17` | same | 2026-07-14 | 120d | ✅ |
| 17 | VRCLightVolumes | `v3.0.1` | `v.3.0.1` | same | 2026-11-10 | 1d | ✅ |
| 18 | EasyQuestSwitch | `v1.4.0` | `v1.4.0` | same | 2024-03-25 | 961d | ⚠️ |
| 19 | VPM Template | `1.0.0` | `0.0.7` | latest | 2026-07-01 | 133d | ✅ |
| 20 | VRCX | `v2026.09.16` | `v2026.09.16` | same | 2026-11-10 | 1d | ✅ |

---

## 分组明细

### 🔴 已过期(2 个)

**说明**: 知识库版本低于 GitHub 最新 Release,且项目仍活跃(> 0 天内有 commit)。

| 工具 | 知识库 | 最新 | 差距 |
|------|--------|------|------|
| AvatarOptimizer | `v1.9.21` | `v1.9.22` | 见原文 |
| VRCFury | `v1.1442.0` | `com.vrcfury.vrcfury/1.1445.0` | 见原文 |

### 🟡 已停滞(1 个,版本一致但无更新 > 365 天)

| 工具 | 最后 Commit | 距今 |
|------|-------------|------|
| EasyQuestSwitch | 2024-03-25 | 961d |

---

## 治理建议

- **🔴 优先**:更新 2 个过期工具的 frontmatter.version。
- **🟡 评估**::1 个工具版本一致但长期无 commit,建议评估是否仍维护。

---

## 附录:工具清单

| # | 工具名 | 知识库版本 | GitHub 仓库 |
|---|--------|------------|-------------|
| 1 | Modular Avatar | `1.18.7` | https://github.com/bdunderscore/modular-avatar |
| 2 | AvatarOptimizer | `v1.9.21` | https://github.com/anatawa12/AvatarOptimizer |
| 3 | VRCFury | `v1.1442.0` | https://github.com/VRCFury/VRCFury |
| 4 | Meshia | `3.3.1` | https://github.com/RamType0/Meshia.MeshSimplification |
| 5 | MA2BT | `v2.0.2` | https://github.com/Null-K/MA2BT |
| 6 | TexTransTool | `v1.0.2` | https://github.com/ReinaS-64892/TexTransTool |
| 7 | LAC | `v0.9.0` | https://github.com/Limitex/avatar-compressor |
| 8 | lilToon | `2.4.1` | https://github.com/lilxyzw/lilToon |
| 9 | ORL Shaders | `v7.3.1` | https://github.com/orels1/orels-Unity-Shaders |
| 10 | UnlitWF | `UnlitWF_Shader_20261101` | https://github.com/whiteflare/Unlit_WF_ShaderSuite |
| 11 | Graphlit | `v2.7.4` | https://github.com/z3y/Graphlit |
| 12 | AudioLink | `v3.1.2` | https://github.com/llealloo/audiolink |
| 13 | VizVid (VVMW) | `1.8.3` | https://github.com/JLChnToZ/VVMW |
| 14 | UdonVoiceUtils | `7.0.0` | https://github.com/Guribo/UdonVoiceUtils |
| 15 | Sardinal | `v0.9.8` | https://github.com/ikuko/Sardinal |
| 16 | ULocalization | `v0.8.17` | https://github.com/ikuko/ULocalization |
| 17 | VRCLightVolumes | `v3.0.1` | https://github.com/REDSIM/VRCLightVolumes |
| 18 | EasyQuestSwitch | `v1.4.0` | https://github.com/vrchat-community/EasyQuestSwitch |
| 19 | VPM Template | `1.0.0` | https://github.com/vrchat-community/template-package |
| 20 | VRCX | `v2026.09.16` | https://github.com/vrcx-team/VRCX |

