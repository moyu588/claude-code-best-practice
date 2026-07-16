# 📋 Claude Code Best Practice — 系统学习计划

> 本文档是基于 [claude-code-best-practice](https://github.com/shanraisshan/claude-code-best-practice) 仓库内容设计的渐进式学习课程。
> 完成每个阶段后，使用 `/quiz <阶段号>` 命令来验证学习效果。

---

## 🧭 学习路线总览

```
第 1 阶段          第 2 阶段          第 3 阶段          第 4 阶段          第 5 阶段          第 6 阶段
┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
│ 环境搭建  │ ──▶ │ 三大原语  │ ──▶ │ Harness  │ ──▶ │ 工作流   │ ──▶ │ 深度专题  │ ──▶ │ 实战+   │
│ & 热身   │     │ C·A·S   │     │ 配置体系  │     │ 方法论   │     │ & 报告   │     │ 社区     │
└──────────┘     └──────────┘     └──────────┘     └──────────┘     └──────────┘     └──────────┘
   2 天             5 天             4 天             4 天             4 天             3 天
```

---

## 🔰 第 1 阶段：环境搭建与热身（Day 1-2）

> **目标**：安装 Claude Code，理解基本交互，运行第一个示例工作流。
> **完成标准**：使用 `/quiz 1` 通过测验（正确率 ≥ 70%）

### Day 1：安装与首次体验

| 步骤  | 内容                     | 阅读材料                                                       |
| --- | ---------------------- | ---------------------------------------------------------- |
| 1.1 | 根据你的操作系统安装 Claude Code | `tutorial/day0/` 下对应的 `windows.md` / `mac.md` / `linux.md` |
| 1.2 | 学习 Claude Code 交互基础    | `tutorial/day1/README.md`                                  |
| 1.3 | 浏览 README 全貌，理解仓库定位    | `README.md`（30 分钟通读）                                       |

**学习要点：**

- Claude Code 是什么，与 Claude.ai 网页版有什么区别
- `/` 斜杠命令的基本用法
- 本仓库的目录结构（`best-practice/`、`implementation/`、`reports/`、`tips/`）
- 仓库的核心定位：从 Vibe Coding 到 Agentic Engineering

> 🎯 **检验点**：能在终端中启动 `claude`，理解 `/` 斜杠命令的基本用法。完成 `1.1-1.3` 后使用 `/quiz 1` 进行阶段测验。

### Day 2：运行天气编排系统

| 步骤  | 内容                            | 阅读材料                                                                                                                                                                       |
| --- | ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2.1 | 理解 Command → Agent → Skill 架构 | `orchestration-workflow/orchestration-workflow.md`                                                                                                                         |
| 2.2 | 实际运行 `/weather-orchestrator`  | 在你的 claude 会话中执行                                                                                                                                                           |
| 2.3 | 阅读三个关键源码文件                    | `.claude/commands/weather-orchestrator.md` → `.claude/agents/weather-agent.md` → `.claude/skills/weather-fetcher/SKILL.md` → `.claude/skills/weather-svg-creator/SKILL.md` |

**学习要点：**

- Command（命令）是什么，存储在哪里
- Agent（智能体）是什么，存储在哪里
- Skill（技能）是什么，存储在哪里
- 两种 Skill 模式的区别：Agent Skill（预加载）vs Independent Skill（手动调用）
- weather-svg-creator 的渐进式披露结构（`SKILL.md` + `examples.md` + `reference.md`）

> 🎯 **检验点**：能画出 Command → Agent → Skill 的完整调用链。**完成 1.1-2.3 全部内容后使用 `/quiz 1` 进行最终阶段测验。**

---

## 🧱 第 2 阶段：三大原语深度理解（Day 3-7）

> **目标**：透彻理解 Commands、Agents、Skills 的设计、配置和使用。
> **完成标准**：使用 `/quiz 2` 通过测验（正确率 ≥ 70%）

### Day 3：Commands（命令）

| 步骤  | 内容                    | 阅读材料                                               |
| --- | --------------------- | -------------------------------------------------- |
| 3.1 | Commands 概念与最佳实践      | `best-practice/claude-commands.md`                 |
| 3.2 | 理解实际实现                | `implementation/claude-commands-implementation.md` |
| 3.3 | 动手：创建你自己的 `/hello` 命令 | 参考 `.claude/commands/time-command.md` 的结构          |

**学习要点：**

- Command 的 Markdown 文件结构
- Command 与 Agent/Skill 的关系
- 何时使用 Command 而非直接对话
- 内置 Command vs 自定义 Command

### Day 4：Subagents（子智能体）

| 步骤  | 内容                     | 阅读材料                                                |
| --- | ---------------------- | --------------------------------------------------- |
| 4.1 | Agents 概念与最佳实践         | `best-practice/claude-subagents.md`                 |
| 4.2 | 理解实际实现                 | `implementation/claude-subagents-implementation.md` |
| 4.3 | 研究 weather-agent 的完整定义 | `.claude/agents/weather-agent.md`                   |
| 4.4 | Agent Memory 机制        | `reports/claude-agent-memory.md`                    |

**学习要点：**

- Agent 的 YAML frontmatter 字段：`name`、`description`、`tools`、`model`、`skills`、`permissionMode` 等
- Agent 的 `skills` 字段如何实现预加载
- Agent Memory 的作用域（`user`、`project`、`local`）
- 何时使用 Agent 而非直接对话

### Day 5：Skills（技能）— 上篇

| 步骤  | 内容                       | 阅读材料                                             |
| --- | ------------------------ | ------------------------------------------------ |
| 5.1 | Skills 概念与最佳实践           | `best-practice/claude-skills.md`                 |
| 5.2 | 理解实际实现                   | `implementation/claude-skills-implementation.md` |
| 5.3 | 研究 weather-fetcher skill | `.claude/skills/weather-fetcher/SKILL.md`        |

**学习要点：**

- Skill 的 YAML frontmatter 字段：`name`、`description`、`argument-hint`、`disable-model-invocation`、`context`、`agent` 等
- `description` 字段是触发条件而非摘要
- `context: fork` 的作用——在独立子智能体中运行
- Skill 的目录结构：`SKILL.md` + 子目录（`references/`、`scripts/`、`examples/`）

### Day 6：Skills（技能）— 下篇

| 步骤  | 内容                                    | 阅读材料                                                                                 |
| --- | ------------------------------------- | ------------------------------------------------------------------------------------ |
| 6.1 | Skills 的渐进式披露（progressive disclosure） | `.claude/skills/weather-svg-creator/` 下的 `SKILL.md` + `examples.md` + `reference.md` |
| 6.2 | Monorepo 中的 Skills 策略                 | `reports/claude-skills-for-larger-mono-repos.md`                                     |
| 6.3 | Agent vs Command vs Skill 三者对比        | `reports/claude-agent-command-skill.md`                                              |

**学习要点：**

- 渐进式披露：主文件给方向，子文件提供细节
- 在 Monorepo 中按子项目组织 Skills
- 三大原语的决策矩阵：何时用哪个

### Day 7：三大原语综合练习

| 步骤  | 内容                                             |
| --- | ---------------------------------------------- |
| 7.1 | 自己设计一个类似天气系统的小工作流（如获取 GitHub Trending 仓库并生成卡片） |
| 7.2 | 创建对应的 Command + Agent + Skill 文件               |

**练习要求：**

- 创建至少 1 个 Command 文件
- 创建至少 1 个 Agent 文件（含 `skills:` 字段引用）
- 创建至少 1 个 Skill 文件（含完整的 YAML frontmatter）

> 🎯 **检验点**：能独立创建 `command → agent → skill` 的工作流。**完成全部 Day 3-7 内容后使用 `/quiz 2` 进行阶段测验。**

---

## ⚙️ 第 3 阶段：Harness 配置体系（Day 8-11）

> **目标**：掌握 Claude Code 的配置层次、权限、钩子和设置体系。
> **完成标准**：使用 `/quiz 3` 通过测验（正确率 ≥ 70%）

### Day 8：Settings 与配置层次

| 步骤  | 内容            | 阅读材料                                                   |
| --- | ------------- | ------------------------------------------------------ |
| 8.1 | Settings 完整参考 | `best-practice/claude-settings.md`（重点阅读前 200 行，其余按需查阅） |
| 8.2 | 全局 vs 项目级配置   | `reports/claude-global-vs-project-settings.md`         |
| 8.3 | 理解配置优先级层级     | `CLAUDE.md` 中的 "Configuration Hierarchy" 部分            |

**学习要点：**

- 6 层配置优先级：Managed → CLI args → settings.local.json → settings.json → ~/.claude/settings.json → hooks-config
- settings.json 的核心字段：`permissions`、`hooks`、`model`、`statusLine`、`sandbox`
- `.gitignore` 的作用——`settings.local.json` 不提交

### Day 9：Memory 与 CLAUDE.md

| 步骤  | 内容                | 阅读材料                                                 |
| --- | ----------------- | ---------------------------------------------------- |
| 9.1 | Memory 体系完整理解     | `best-practice/claude-memory.md`                     |
| 9.2 | 剖析项目自身的 CLAUDE.md | `CLAUDE.md` — 作为"最佳实践的活样本"来读                         |
| 9.3 | Rules 系统          | `.claude/rules/markdown-docs.md` 和 `presentation.md` |

**学习要点：**

- CLAUDE.md 的 200 行建议限制
- `.claude/rules/*.md` 的懒加载机制（`paths:` YAML frontmatter）
- 多种 Memory 存储位置：CLAUDE.md、`.claude/rules/`、`~/.claude/rules/`、`~/.claude/projects/<project>/memory/`
- Rules 无 frontmatter 时自动加载到每个会话

### Day 10：Hooks 钩子系统

| 步骤   | 内容                             | 阅读材料                                     |
| ---- | ------------------------------ | ---------------------------------------- |
| 10.1 | Hooks 完整指南                     | `.claude/hooks/HOOKS-README.md`          |
| 10.2 | 理解 hooks.py 实现                 | `.claude/hooks/scripts/hooks.py`         |
| 10.3 | 理解 hooks 配置                    | `.claude/hooks/config/hooks-config.json` |
| 10.4 | 查看 settings.json 中的 hooks 事件绑定 | `.claude/settings.json`                  |

**学习要点：**

- Hooks 支持的事件类型：PreToolUse、PostToolUse、SessionStart、Stop 等 15+ 种
- hooks-config.json vs hooks-config.local.json
- 在 Skill 中嵌入 on-demand hooks
- PostToolUse hook 用于 auto-format 等自动化

### Day 11：MCP、Permissions 与其他配置

| 步骤   | 内容            | 阅读材料                                        |
| ---- | ------------- | ------------------------------------------- |
| 11.1 | MCP 配置        | `best-practice/claude-mcp.md` + `.mcp.json` |
| 11.2 | CLI 启动标志      | `best-practice/claude-cli-startup-flags.md` |
| 11.3 | Power-ups     | `best-practice/claude-power-ups.md`         |
| 11.4 | Harness 为什么重要 | `reports/why-harness-is-important.md`       |

**学习要点：**

- MCP（Model Context Protocol）在 Claude Code 中的配置方式
- 常用 CLI 标志：`--model`、`--permission-mode`、`--sandbox`、`--worktree`
- Permissions 的 allow/deny/ask 三种模式
- 为什么"配置优于提示"——harness 强制执行 vs memory 建议

> 🎯 **检验点**：能配置自己的 settings.json，理解配置生效层级，能写一个简单的 hook。**完成后使用 `/quiz 3` 进行阶段测验。**

---

## 🔀 第 4 阶段：工作流方法论（Day 12-15）

> **目标**：学习主流开发工作流模式，理解社区最佳实践。
> **完成标准**：使用 `/quiz 4` 通过测验（正确率 ≥ 70%）

### Day 12：Boris Cherny 工作流（Claude Code 创造者）— 上篇

按时间顺序阅读 Boris 的技巧集：

| 顺序  | 材料                                       | 重点主题                                |
| --- | ---------------------------------------- | ----------------------------------- |
| 1   | `tips/claude-boris-13-tips-03-jan-26.md` | 基础设置、plan mode、subagents、context 管理 |
| 2   | `tips/claude-boris-10-tips-01-feb-26.md` | 工作流选择、CLAUDE.md 建议、Skill vs Command |
| 3   | `tips/claude-boris-12-tips-12-feb-26.md` | 定制化、hooks、permissions、Stop hook     |

**学习要点：**

- Boris 的核心建议：plan mode 开始、用 commands 组织工作流、feature-specific agents
- context rot 的概念和应对策略
- "if you do something more than once a day, turn it into a skill or command"

### Day 13：Boris 进阶 + Thariq 工作流

| 顺序  | 材料                                       | 重点主题                                  |
| --- | ---------------------------------------- | ------------------------------------- |
| 4   | `tips/claude-boris-2-tips-25-mar-26.md`  | PR 策略、squash merge、行数分布               |
| 5   | `tips/claude-boris-15-tips-30-mar-26.md` | 隐藏功能、/focus、Effort levels             |
| 6   | `tips/claude-boris-6-tips-16-apr-26.md`  | 会话管理、/focus 模式、auto mode              |
| 7   | `tips/claude-thariq-tips-17-mar-26.md`   | Skills 深度编写指南（Thariq 是 Anthropic 工程师） |
| 8   | `tips/claude-thariq-tips-16-apr-26.md`   | Session 管理、1M Context 使用策略            |

**学习要点：**

- PR 最佳实践：小 PR（p50 = 118 行）、squash merge
- Effort levels：low → medium → high → xhigh → max
- Thariq 的 Skills 编写哲学：description 是触发器、don't railroad Claude、build Gotchas

### Day 14：社区工作流对比

| 步骤   | 内容                                      | 阅读材料                                                                               |
| ---- | --------------------------------------- | ---------------------------------------------------------------------------------- |
| 14.1 | 理解 RPI 工作流（Research → Plan → Implement） | `development-workflows/rpi/rpi-workflow.md` + 阅读 `.claude/agents/` 下的 8 个 agent 定义 |
| 14.2 | 跨模型工作流                                  | `development-workflows/cross-model-workflow/cross-model-workflow.md`               |
| 14.3 | 对比 README 中 10+ 种工作流的异同                 | `README.md` 的 DEVELOPMENT WORKFLOWS 表                                              |

**学习要点：**

- RPI 的完整流程和各阶段 agent 的职责
- 跨模型协作的三种方式：Plugin、MCP、Router
- 10+ 种工作流的共性：都遵循 Research → Plan → Execute → Review → Ship
- 黄色标签（子循环）的含义

### Day 15：视频学习（选看，但强烈推荐）

| 优先级 | 视频                                                          | 文件                                                    |
| --- | ----------------------------------------------------------- | ----------------------------------------------------- |
| ⭐⭐⭐ | Building Claude Code with Boris Cherny (Pragmatic Engineer) | `videos/claude-boris-pragmatic-engineer-04-mar-26.md` |
| ⭐⭐⭐ | Full Walkthrough: Workflow for AI Coding (Matt Pocock)      | `videos/claude-matt-pocock-24-apr-26.md`              |
| ⭐⭐  | Everything We Got Wrong About RPI (Dex)                     | `videos/claude-dex-mlops-community-24-mar-26.md`      |
| ⭐⭐  | Inside Claude Code With Its Creator (Y Combinator)          | `videos/claude-boris-y-combinator-17-feb-26.md`       |
| ⭐   | From Vibe Coding to Agentic Engineering (Karpathy)          | `videos/claude-karpathy-ai-engineer-02-may-26.md`     |

> 🎯 **检验点**：能对比至少 3 种工作流的异同，形成自己的偏好并说明理由。**完成后使用 `/quiz 4` 进行阶段测验。**

---

## 🔬 第 5 阶段：深度专题（Day 16-19）

> **目标**：攻克仓库中最深入的技术报告，形成专家级理解。
> **完成标准**：使用 `/quiz 5` 通过测验（正确率 ≥ 70%）

### Day 16：Agent SDK vs CLI + Advanced Tool Use

| 步骤   | 内容                       | 阅读材料                                                |
| ---- | ------------------------ | --------------------------------------------------- |
| 16.1 | Agent SDK 与 CLI 的系统提示词差异 | `reports/claude-agent-sdk-vs-cli-system-prompts.md` |
| 16.2 | Advanced Tool Use        | `reports/claude-advanced-tool-use.md`               |

**学习要点：**

- Agent SDK 和 CLI 在系统提示词层面的关键差异
- 高级工具使用模式

### Day 17：浏览器自动化 + 实用专题

| 步骤   | 内容                                      | 阅读材料                                                |
| ---- | --------------------------------------- | --------------------------------------------------- |
| 17.1 | Claude in Chrome vs Chrome DevTools MCP | `reports/claude-in-chrome-v-chrome-devtools-mcp.md` |
| 17.2 | Usage & Rate Limits                     | `reports/claude-usage-and-rate-limits.md`           |
| 17.3 | Spinner Verbs & Tips（CLI 二进制中的隐藏内容）     | `reports/claude-spinner-verbs-and-tips.md`          |

**学习要点：**

- 三种浏览器自动化方案的对比
- Token 用量和速率限制
- CLI 二进制中内嵌的提示信息

### Day 18：LLM 退化与学习历程

| 步骤   | 内容          | 阅读材料                                                    |
| ---- | ----------- | ------------------------------------------------------- |
| 18.1 | LLM 的日常退化现象 | `reports/llm-day-to-day-degradation.md`                 |
| 18.2 | 天气系统的重构学习历程 | `reports/learning-journey-weather-reporter-redesign.md` |

**学习要点：**

- LLM 输出质量随时间退化的现象和原因
- 通过重构天气系统学到的经验教训
- 如何应对模型退化

### Day 19：Agent Teams + Scheduled Tasks + Goal

| 步骤   | 内容                 | 阅读材料                                                      |
| ---- | ------------------ | --------------------------------------------------------- |
| 19.1 | Agent Teams 实现     | `implementation/claude-agent-teams-implementation.md`     |
| 19.2 | Scheduled Tasks 实现 | `implementation/claude-scheduled-tasks-implementation.md` |
| 19.3 | Goal 机制            | `implementation/claude-goal-implementation.md`            |

**学习要点：**

- Agent Teams 的工作原理和使用场景
- `/loop` vs `/schedule` 的区别（本地 vs 云端）
- Goal 机制的作用

> 🎯 **检验点**：能解释 Agent SDK vs CLI 的设计差异、理解上下文退化曲线、知道何时用 Agent Teams。**完成后使用 `/quiz 5` 进行阶段测验。**

---

## 🚀 第 6 阶段：实战输出 + 社区融入（Day 20-22）

> **目标**：将所学应用到自己的项目，融入社区。
> **完成标准**：使用 `/quiz 6` 通过最终测验（正确率 ≥ 70%）

### Day 20：配置自己的项目

| 步骤   | 内容                                                         |
| ---- | ---------------------------------------------------------- |
| 20.1 | 为你自己的项目编写 CLAUDE.md（参考本仓库的 CLAUDE.md 结构）                   |
| 20.2 | 创建 `.claude/rules/` 下的规则文件                                 |
| 20.3 | 配置 `.claude/settings.json` 和 `.claude/settings.local.json` |
| 20.4 | 创建至少 1 个 Command + 1 个 Skill                               |

**练习要求：**

- CLAUDE.md 控制在 200 行以内
- 至少设置 1 个 permission allow 规则
- 为你的项目目录创建 1 个带 `paths:` frontmatter 的 rule 文件

### Day 21：建立自己的工作流

| 步骤   | 内容                                            |
| ---- | --------------------------------------------- |
| 21.1 | 选择并适配一种工作流方法论（RPI / GSD / Spec Kit / 自建）      |
| 21.2 | 为常用操作创建 Commands（`/review`、`/ship`、`/plan` 等） |
| 21.3 | 设置 Hooks（如 auto-format、commit 声音提醒）           |

**练习要求：**

- 创建至少 2 个 Commands
- 配置至少 1 个 Hook（如 PostToolUse auto-format）

### Day 22：社区融入与持续学习

| 步骤   | 内容                                                                                                                 |
| ---- | ------------------------------------------------------------------------------------------------------------------ |
| 22.1 | 订阅 README 中列出的 Reddit、X、YouTube 频道                                                                                 |
| 22.2 | 关注 Boris Cherny、Thariq、Cat Wu 等核心人物                                                                                |
| 22.3 | 建立自己的更新习惯：每天 `claude --version` + 阅读 [changelog](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md) |

> 🎯 **最终检验点**：能独立用 Claude Code 完成一个完整的 feature 开发周期（Plan → Implement → Review → Ship）。**完成后使用 `/quiz 6` 进行最终测验。**

---

## 📊 学习进度总览

| 阶段      | 天数        | 核心能力                            | 测验命令      |
| ------- | --------- | ------------------------------- | --------- |
| 1. 环境搭建 | Day 1-2   | 安装、启动、运行示例                      | `/quiz 1` |
| 2. 三大原语 | Day 3-7   | Commands / Agents / Skills 设计能力 | `/quiz 2` |
| 3. 配置体系 | Day 8-11  | Settings / Memory / Hooks / MCP | `/quiz 3` |
| 4. 工作流  | Day 12-15 | 方法论选择与比较                        | `/quiz 4` |
| 5. 深度专题 | Day 16-19 | 专家级理解                           | `/quiz 5` |
| 6. 实战输出 | Day 20-22 | 应用到实际项目                         | `/quiz 6` |

---

## 💡 学习建议

1. **每阶段结束写笔记**：用 Claude 帮你总结该阶段的要点，形成自己的 Cheat Sheet
2. **边读边做**：不要只读文档，每个概念都在 Claude Code 中实际运行验证
3. **不要跳过天气系统**：它是理解整个架构的"Rosetta Stone"（罗塞塔石碑）——只用 4 个文件展示了所有核心概念
4. **Tips 可以常读**：83 条技巧不必一次读完，当作手册按需查阅
5. **视频优先级**：Boris 在 Pragmatic Engineer 的访谈是最高价值的内容
6. **测验机制**：每个阶段学完后使用 `/quiz <阶段号>` 验证学习效果，Quiz 会覆盖该阶段的所有关键概念
7. **上下文管理意识**：从 Day 1 就养成关注上下文使用量的习惯——这是区分新手和老手的关键指标

---

## 🔗 快速导航

- [官方文档](https://code.claude.com/docs)
- [Changelog](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
- [Boris Cherny on X](https://x.com/bcherny)
- [Thariq on X](https://x.com/trq212)
- [本仓库 README](README.md)
- [Billion-Dollar Questions](README.md#billion-dollar-questions) — 学完后思考这些问题
