# Handoff

**一份 Markdown，让任何模型、任何会话，都能从上一个会话停下的一步接着干——不用重做、不用重新回答、不会再踩一遍你已经排除的死路。**

交接不是总结。总结告诉你“发生了什么”，交接告诉下一个 AI：**现在什么是真的、下一步做什么、什么不要重复、什么还没被证实。**

```
你：   交接一下
AI：   已写 .handoff/2026-10-29-1558-tv-setup.md（status: active，42 行）

--- 换了会话、换了模型、第二天 ---

你：   接上次
AI：   目标：给客厅那台电视去广告去遥测。
        停在：工具已部署，电视一次都还没连过。
        下一步：先用 ADB 读出电视平台，再跑只读检查。
        有一条失效了：昨天那条端口检查结果不再成立，正在重跑。
```

## 为什么是这一版

交接提示词很容易写歪，而且错法高度雷同：文档写成流水账、把“废案”删掉（下一个 AI 于是把死路重走一遍）、没验证就说“已完成”、密钥跟着一起带出去。这个技能就是照着这些坑写的，而且刻意写得足够短，任何模型都能照着执行。

- **通用**：一个 Markdown 文件，无脚本、无命令行工具、无依赖、不绑定厂商。聊天窗口、Claude Code、Codex、Cursor、Copilot、Gemini CLI、OpenCode、你自己写的 agent 都能用。
- **自然语言触发**：说 `交接一下` / `接上次` / `handoff` / `pick up where I left off` 就行，不用记命令。
- **事实是查出来的，不是回忆出来的**：每个产物要么这一轮验证过，要么明确标 `unverified`。
- **“废案”是必填章节**：最省时间的一节，也是别的格式普遍缺的一节。
- **机器可读的外壳**：五个前置字段（`date`、`task`、`status`、`supersedes`、`verified`），脚本或 agent 可以据此查找、排序、判断是否被取代。
- **接续有明确契约**：接收方先复述、再做失效检查、并且知道什么时候算干完——而不是越做越跑偏。
- **对密钥失败即止**：已知的密钥形态会被隐去；如果隐去后仍有敏感信息会外泄，技能会拒绝写出文件。

## 与同类对比

| | 本技能 | mattpocock `handoff` | Jiaojie 交接 | CatchUp | Superpowers |
|---|---|---|---|---|---|
| 形态 | 单个 Markdown 技能 | 单个 Markdown 技能 | 技能 + zip/审计模式 | 命令行 + 本地会话文件 | 技能包 |
| 依赖 | 无 | 无 | 打包模式需要 Python | Node CLI | 插件运行时 |
| 跨模型/跨 agent | 是 | 是 | 是（多运行时） | 支持 14 种，不含 Hermes | 主要面向 Claude Code |
| 机器可读前置字段 | 有 | 无 | 有（coverage/status） | 不适用（工具） | 不适用 |
| 接续时强制失效检查 | 有 | 无 | 有“需重新验证”提示 | 部分（重读会话） | 无 |
| 读取真实会话记录 | 取决于 harness 是否暴露 | 无 | 无 | **有** | 无 |
| “废案”必填 | 有 | 无 | 有 | 无 | 无 |
| 篇幅 | 约 120 行提示词 | 约 16 行 | 较大，多模式 | 工具 | 技能包 |

想要一个把 Claude Code 会话直接交接给 Codex 的命令行工具，用 CatchUp；需要审计打包、跨组织交付，用 Jiaojie；想要“手机聊天窗口和编码 agent 都跟得上、接续不跑偏的最小东西”，用这个。

## 安装

把 `SKILL.md` 放到你的 agent 读取技能的位置即可，别的不需要。

| Agent | 放哪里 |
|---|---|
| **Hermes Agent** | `skills/handoff/SKILL.md`，或把 profile 的技能目录指向本仓库 |
| **Claude Code** | `~/.claude/skills/handoff/SKILL.md`（或项目内 `.claude/skills/`） |
| **Codex / OpenCode** | 项目 `AGENTS.md`：把本文件正文追加进去，或按你的加载方式放 `skills/handoff/SKILL.md` |
| **Cursor / Copilot / Windsurf** | 把正文粘进 `.cursor/rules/handoff.mdc` 或 `.github/copilot-instructions.md` |
| **Gemini CLI / 任意聊天界面** | 上传 `SKILL.md`，或把它作为会话第一条消息粘贴 |

本仓库的 `AGENTS.md` 就是“直接粘进规则文件”的形态。

## 它写出来的文件长什么样

```markdown
---
date: 2026-10-29T15:58+08:00
task: 给客厅那台 Android 电视去广告与遥测
status: active
supersedes: none
verified: 2026-10-29
---

## 停在哪
去广告工具箱已部署，Python 环境验证通过（三个工具 `--help` 都 exit 0）。
电视本体完全没动过，证据：探针还没跑。

## 下一步
先用 ADB 读出电视平台，再对候选清单跑只读检查。
完成标准：读出的平台与清单所属平台一致，且清单里每个包都被归类为 存在/已禁用/不存在。

## 已定决策
- 先只读检查，再禁用 —— 因为清单选错会连远程支持和 OTA 一起去掉。

## 废案
- 用上一代机型的清单 —— 平台代际不同，包名对不上，会关错东西。
- 直接卸载预装商店而不是禁用 —— 需要 root，且没有回滚。

## 关键产物
- /path/to/toolkit —— 已验证
- /path/to/toolkit/examples/<device>/debloat.txt —— 未验证（还没跟真机比对）

## 边界
- 已授权：只读探测。
- 必须重新确认：禁用任何系统包。

## 还没回答的问题
- 换不换桌面启动器？（决定要不要额外装启动器）
```

上限 60 行，真实用法大多落在 30～45 行。

## 它做不到的

- **读不到你看不见的会话**。有些 harness 不把完整记录给模型，它就只用可见对话——这类信息一律标 `unverified`。
- **不搬文件**。只搬引用，不搬内容；要跨机器请用压缩包或同步工具。
- **不替代上下文压缩**。压缩是让一个长会话活着，交接是让工作跨会话继续。

## 来源与致谢

思路来自 `mattpocock/skills`（最小交接）、`Jordanwei1/jiaojie-skill`（废案、重新验证、格式取舍）、`wilbeibi/catchup`（读取真实会话记录）、`obra/superpowers`（技能要写明自己的失败模式）。详见 [ATTRIBUTION.md](ATTRIBUTION.md)。

## 作者

**biggershun（大顺）** — [github.com/biggershun](https://github.com/biggershun)

搜索词：交接技能、AI 交接、上下文交接、换会话继续、handoff skill、agent handoff、session handoff、context handoff、resume agent session、Claude Code 技能、Codex 技能、Hermes Agent 技能。

MIT。