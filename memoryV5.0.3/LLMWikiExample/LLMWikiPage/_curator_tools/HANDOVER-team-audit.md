# 交接文档：LLMWikiPage 团队审计（Curator 模式）

> 交接时间：2026-11-10。上一个会话已完成「脚本恢复 + 内容同步 + Q4 首轮审计」，本会话只负责**执行修复**。
> 执行前先读：`meta/kb-protocol.md`（Curator 四步流程）+ `meta/working-modes.md`（Curator 模式定义）。

---

## 1. 背景与现状

- 知识库：`D:\UDON\memoryV5.0.0\LLMWikiExample\LLMWikiPage\`（415 页 md，Obsidian wikilink 风格）
- 内容已与原仓库 [RhineLab-magellan/VRChat-Agent-Creator-Library](https://github.com/RhineLab-magellan/VRChat-Agent-Creator-Library) main **完全对齐**（2026-11-10 规范化换行后逐字节比对零差异），本地仅多 `_curator_tools\` 下的审计报告
- Curator 辅助脚本已恢复：`D:\UDON\memoryV5.0.0\LLMWikiExample\auxiliary-script\`（59 个文件，源自原仓库 `Auxiliary script/`）
- 维护断档：最后一轮复核 2026-08-20，本次审计就是补 Q4 账

## 2. 环境与脚本

- Python 3.13（`python` 命令可用），脚本直接运行，无需额外依赖（纯标准库）
- 四个核心脚本已做本机适配（`SCRIPT_DIR.parent/'memory'` → `'LLMWikiPage'`；confidence 白名单加小写、source_type 加 local）
- 运行顺序（在 auxiliary-script 目录下）：
  ```
  python validation_script.py     # frontmatter + 结构 → a4_validation_report.md
  python governance_script.py     # 死链/孤立 → a5_governance_report.md
  python version_audit_script.py  # GitHub 版本核对 → a13_version_audit_report.md（需网络，60 req/h 限流）
  python url_health_check.py      # URL 可达性 → a13_url_health_report.md（约 3.5 分钟）
  ```
- 报告全部落在 `LLMWikiPage\_curator_tools\`，脚本只写报告，不改知识页

## 3. 已知误报与坑（勿浪费时间）

1. **governance 报的 220 条死链全是误报**：脚本只认普通相对路径链接，不认 Obsidian `[[wikilink]]`。不要把库里的 wikilink 改成普通链接来"修"它们。
2. **validation 剩 4 个失败**（2×TOO_FEW_TAGS、2×INVALID_LEVEL:`governance`）：是库演进出的新值不在脚本白名单，属脚本滞后，不是页面错误。
3. **换行符**：本地 CRLF、原仓库 LF。任何与原仓库 diff 前先 `\r\n` → `\n` 规范化，否则 297 个文件"假差异"。
4. **url_health 的 420 个超时不可直接定性**：本机网络环境所致，换网络/代理后重跑再判断真死链；23 个 4xx 可先看。
5. **沙箱**：run_code/pwsh 在 D:\UDON 下可能遇 ACL 报错（`grantWrite Win32 5`），该会话文件策略已是 danger-full-access，照常用即可。
6. **证据纪律**（来自原仓库 update-checklist 的 Evidence Gate，库内已部分内化）：官方文档与可复现实测冲突时**保留实测结论**；禁止把 A 版本的验证推广到 B 版本。

## 4. 本轮审计的真实工作清单（按优先级）

### P0 —— version_audit 报的 14 个过期工具，逐一复核对应页面

| 工具 | 库内版本 → 最新 | 对应页面 |
|---|---|---|
| UdonVoiceUtils | 5.1.4 → **7.0.0（大版本，重点）** | 无专页，先 grep 定位引用 |
| VRCLightVolumes | v2.1.3 → v3.0.1（大版本） | entities/world/vrc-light-volumes.md |
| Modular Avatar | 1.17.1 → 1.18.7 | entities/avatar/modular-avatar.md（+ tutorials-detailed） |
| AvatarOptimizer | 1.9.14 → 1.9.21 | entities/avatar/avatar-optimizer.md |
| VRCFury | v1.1341.0 → 1.1442.x | entities/avatar/vrcfury-reference.md |
| Meshia | 3.2.0 → 3.3.1 | entities/avatar/meshia-mesh-simplification.md |
| TexTransTool | v1.0.1 → v1.0.2 | entities/avatar/tex-trans-tool.md |
| LAC | v0.8.0 → v0.9.0 | entities/avatar/lac-avatar-compressor.md |
| lilToon | 2.3.3 → 2.4.1 | entities/avatar/shader/liltoon/（11 页） |
| ORL Shaders | v7.2.0 → v7.3.1 | entities/avatar/shader/orl/（4 页） |
| UnlitWF | 20260208 → 20261101 | entities/avatar/shader/unlitwf/index.md |
| Graphlit | v2.7.2 → v2.7.4 | entities/world/shader/graphlit.md |
| VizVid (VVMW) | 1.7.5 → 1.8.3 | entities/world/vvmw.md |
| ULocalization | v0.8.16 → v0.8.17 | grep 定位 |

修法：拉该工具对应版本的 changelog/release note → 三支柱分类（Fill/Refresh/Quality）→ 只改受影响页面 → 更新页面 frontmatter 的 `last_review` 与版本记录。

### P1 —— 6 月批次核心规则页对照 SDK 3.10.6 复核

`concepts/rules/udonsharp-language-limits.md`（last_review 2026-06-20）及其 related 页（`networking-rules`、`vrchat-api-exposure`、`api-checker`、`not-exposed`、`exposed-types`）。对照 https://creators.vrchat.com/releases/ 检查 3.10.x 新 API 暴露状态是否已反映。

### P2 —— governance 报的 3 个真孤立文档

见 `a5_governance_report.md`「真正孤立文档」节：决定挂链（在相关页 related 里互链）还是标记 archived。长尾 29 页仅记录，不强制处理。

## 5. 团队分工建议（Agent Teams）

- **写范围切分**：按目录分派，一人一目录不重叠——如 A=lilToon+shader 系、B=avatar 工具链（MA/AOI/VRCFury/Meshia/TTT/LAC）、C=world 侧（LightVolumes/VVMW/Graphlit）+ udonsharp 规则页、D=复验员
- **流程**：修复人改页 → 复验员独立验证（重跑 validation 必须通过 + 抽查内容）→ P0 修复必须二次独立验证（kb-protocol 规定）
- **禁改**：`_curator_tools/\` 下报告不编辑；`meta/\` 协议文件本轮不动；不要为消死链改 wikilink 格式
- **frontmatter 纪律**：改完页面必须同步更新 `last_review`（当天日期）与 `updated`；tags 保持 3-8 个

## 6. 验收标准

1. 重跑 `validation_script.py`：passed ≥ 411（不因本轮修改新增失败）
2. 14 个过期工具页面全部完成复核或明确标注「本轮不更新」的原因（写进审计 journal）
3. 产出审计报告一份（P0/P1/P2 完成状态）存 `_curator_tools/\`
4. `version_audit_script.py` 复跑 outdated 数下降（版本号写回 frontmatter/FACT.md 的工具）
