---
name: learning-quiz
description: Quiz the user on their Claude Code learning progress. Use when the user asks for a quiz, test, or exam on their Claude Code knowledge, or explicitly invokes /quiz. Covers 6 stages from basic setup to advanced topics.
argument-hint: [stage-number]
disable-model-invocation: false
user-invocable: true
---

# Learning Quiz — Claude Code Best Practice

You are a strict but fair examiner. Your job is to test the user's knowledge of Claude Code based on the learning plan in `LEARNING-PLAN.md`.

## How the Quiz Works

1. **Identify the stage**: The user will specify which stage they want to be quizzed on (1-6).

2. **Ask questions** from the corresponding stage section below, one at a time.

3. **Evaluate answers**: Be strict but fair. If the answer is partially correct, say so and give partial credit.

4. **Track score**: Keep a running tally of correct answers.

5. **Provide feedback**: After each answer, give a brief explanation of the correct answer.

6. **Final report**: At the end, give a score (X/Y), percentage, and verdict:
   
   - **≥80%**: 🎉 Excellent! You're ready for the next stage.
   - **70-79%**: ✅ Good! Review the weak areas and move on.
   - **50-69%**: ⚠️ Fair. Re-read the relevant sections before proceeding.
   - **<50%**: 📖 Needs more study. Go through the stage materials again.

7. **Ask 5 questions per quiz session**. Pick randomly from the pool, ensuring coverage of different sub-topics within the stage. If the stage has fewer than 5 questions, ask them all.

## Quiz Rules

- Do NOT give hints unless the user explicitly asks for one (deduct 0.5 points for hints).
- Accept answers that capture the essential meaning, not exact wording.
- After the quiz, suggest which specific files to re-read for wrong answers.
- If the user says "skip" or "I don't know", mark it wrong and move on.

---

## Stage 1: Environment Setup & Warmup

### Q1: What is Claude Code, and how does it differ from claude.ai?

**Answer**: Claude Code is a CLI (command-line interface) tool that runs in the terminal, while claude.ai is a web-based chat interface. Claude Code is an agentic coding tool that can read/write files, run shell commands, and interact with the codebase directly. It's designed for software engineering workflows rather than general conversation.

### Q2: Where is the primary configuration file for a Claude Code project stored?

**Answer**: `.claude/settings.json` for team-shared settings, and `.claude/settings.local.json` for personal overrides (git-ignored).

### Q3: What is the architecture pattern demonstrated by the weather orchestrator?

**Answer**: Command → Agent → Skill. The command (`/weather-orchestrator`) is the entry point. It invokes the weather-agent, which uses the weather-fetcher skill (preloaded) to fetch data. Then the command invokes the weather-svg-creator skill to generate an SVG output.

### Q4: What are the two types of Skill patterns demonstrated in this repository?

**Answer**: (1) Agent Skills — preloaded into agent context via the `skills:` field in agent frontmatter (e.g., weather-fetcher). (2) Independent Skills — invoked manually via the `Skill` tool by a command or agent (e.g., weather-svg-creator).

### Q5: What are the main directories in this repository and their purposes?

**Answer**: `best-practice/` — concept documentation and best practices; `implementation/` — concrete implementation examples; `reports/` — deep-dive research reports; `tips/` — community and team tips; `.claude/` — actual working Claude Code configuration (agents, commands, skills, hooks, settings); `orchestration-workflow/` — the weather demo system; `tutorial/` — getting started guides; `videos/` — transcribed video content; `development-workflows/` — workflow pattern examples.

### Q6: What file extensions are used for Commands, Agents, and Skills?

**Answer**: All use `.md` (Markdown) files. Commands: `.claude/commands/<name>.md`; Agents: `.claude/agents/<name>.md`; Skills: `.claude/skills/<name>/SKILL.md`.

---

## Stage 2: Commands, Agents & Skills

### Q1: What YAML frontmatter fields are available for Agent definitions?

**Answer**: `name`, `description`, `tools`, `disallowedTools`, `model`, `permissionMode`, `maxTurns`, `skills`, `mcpServers`, `hooks`, `memory`, `background`, `effort`, `isolation`, `color`.

### Q2: What is the difference between `tools` and `disallowedTools` in an agent definition?

**Answer**: `tools` is an allowlist — only specified tools are available. `disallowedTools` removes tools from the inherited or specified set. If `tools` is omitted, the agent inherits all available tools.

### Q3: What is the purpose of the `skills` field in an agent's YAML frontmatter?

**Answer**: It preloads one or more skill files into the agent's context at startup. This means the agent has immediate access to the skill's instructions without needing the Skill tool. This is the "Agent Skill" pattern.

### Q4: What is "progressive disclosure" in the context of Skills?

**Answer**: Skills are folders, not just single files. The main `SKILL.md` gives high-level instructions, while subdirectories like `references/`, `scripts/`, and `examples/` contain detailed information that Claude loads only when needed. This keeps the main context lean while making detailed knowledge available on demand.

### Q5: What should a Skill's `description` field contain, and why?

**Answer**: The `description` field should be a trigger condition written for the model — "when should I fire?" — NOT a human-readable summary. It's used by the auto-discovery system to decide when to invoke the skill automatically.

### Q6: What does `context: fork` do in a Skill definition?

**Answer**: It runs the skill in an isolated subagent context. The main session only sees the final result, not the intermediate tool calls. The `agent` field can specify which subagent type to use (default: `general-purpose`).

### Q7: What is the `disable-model-invocation` field for in a Skill definition?

**Answer**: When set to `true`, it prevents automatic invocation of the skill by the model. The skill can only be invoked manually by the user via `/skill-name`.

### Q8: According to the repository, should you prefer general-purpose agents or feature-specific agents?

**Answer**: Feature-specific agents with skills (progressive disclosure). Boris explicitly recommends this over general-purpose agents like "backend engineer" or "qa". Each agent should have focused context and specific skills.

### Q9: What is the `permissionMode` field in agent definitions and what values can it take?

**Answer**: It controls the permission behavior for the agent. Values include: `"acceptEdits"`, `"plan"`, `"bypassPermissions"`, and the default (inherits from session). `"acceptEdits"` auto-approves file edits; `"plan"` runs in plan mode; `"bypassPermissions"` skips all permission prompts.

### Q10: What is Agent Memory and what scopes does it support?

**Answer**: Agent Memory provides persistent memory for subagents across sessions. Three scopes: `user` (shared across all projects), `project` (shared within the project), `local` (specific to that agent instance). Configured via the `memory` field in agent frontmatter.

---

## Stage 3: Harness Configuration System

### Q1: What is the configuration hierarchy for Claude Code (from highest priority to lowest)?

**Answer**: (1) Managed settings (MDM/plist/Registry) → (2) Command line arguments → (3) `.claude/settings.local.json` → (4) `.claude/settings.json` → (5) `~/.claude/settings.json` → (6) `hooks-config.local.json` overrides `hooks-config.json`.

### Q2: Why should you put deterministic behaviors in settings.json instead of CLAUDE.md?

**Answer**: The harness (settings.json) enforces behavior deterministically — it's a system-level constraint. CLAUDE.md is advisory; the model can ignore it. For behaviors that MUST happen (like "NEVER add Co-Authored-By"), use settings.json's `attribution.commit: ""`.

### Q3: What is the recommended maximum length for CLAUDE.md and why?

**Answer**: Under 200 lines per file. Longer files cause the model to ignore instructions, especially as files grow. For content exceeding 200 lines, split into `.claude/rules/*.md` files.

### Q4: How do `.claude/rules/*.md` files with and without `paths:` frontmatter differ?

**Answer**: Without `paths:` frontmatter, rule files auto-load into every session like CLAUDE.md. With `paths:` YAML frontmatter (containing glob patterns), the rule is lazy-loaded only when Claude touches files matching the specified paths.

### Q5: Name at least 5 hook events that Claude Code supports.

**Answer**: PreToolUse, PostToolUse, UserPromptSubmit, Notification, Stop, SubagentStart, SubagentStop, PreCompact, SessionStart, SessionEnd, Setup, PermissionRequest, TeammateIdle, TaskCompleted, ConfigChange, PostToolUseFailure.

### Q6: What is the purpose of a PostToolUse hook?

**Answer**: It fires after a tool is successfully used. Common use cases include: auto-formatting code (handle the last 10% of formatting to avoid CI failures), logging tool usage, triggering notifications, or running validation scripts.

### Q7: What is MCP and how is it configured in Claude Code?

**Answer**: MCP (Model Context Protocol) allows Claude Code to connect to external servers that provide additional tools. Configured in `.claude/settings.json` under `mcpServers` or in `.mcp.json` at the project root. Each server entry specifies a command and optional arguments.

### Q8: What is the difference between `.claude/settings.json` and `.claude/settings.local.json`?

**Answer**: `settings.json` is team-shared and checked into git. `settings.local.json` is personal overrides, git-ignored, never committed. Local settings override shared settings for the same keys.

### Q9: What are the three permission modes for tool access?

**Answer**: `allow` (auto-approve), `deny` (auto-deny), and `ask` (prompt user each time, the default). Wildcards are supported, e.g., `Bash(npm run *)`, `Edit(/docs/**)`.

### Q10: What is the "sandbox" feature in Claude Code?

**Answer**: Sandboxing provides file and network isolation, reducing permission prompts. Boris reported an 84% reduction in permission prompts internally. Configured via `/sandbox` or settings.json.

---

## Stage 4: Workflow Methodologies

### Q1: What is Boris Cherny's most repeated advice for starting any task?

**Answer**: Always start with plan mode. Use `/plan` or set `permissionMode: "plan"` to have Claude design an approach before writing any code.

### Q2: What is "context rot" and at what thresholds does it become problematic?

**Answer**: Context rot is the degradation of model performance as the conversation context grows. It kicks in around 300-400k tokens on the 1M model. The "dumb zone" starts around 40% context usage. Best practice: keep under 40%, push to 60% only on simple tasks, aggressively keep below 30% for experienced users.

### Q3: What is the difference between `/compact` and `/clear`?

**Answer**: `/compact` summarizes the conversation to save context — lossy but momentum-friendly (mid-task, fuzzy details ok). `/clear` wipes the conversation entirely — more work but you control exactly what carries forward (high-stakes next step).

### Q4: According to Boris, what should you do if you find yourself doing something more than once a day?

**Answer**: Turn it into a Skill or Command. Everything in `.claude/commands/` and `.claude/skills/` is checked into git, making workflows shareable across the team.

### Q5: What are the key characteristics of effective PRs according to Boris?

**Answer**: Small and focused — p50 of 118 lines changed, one feature per PR. Always squash merge for clean linear history (easier git revert and git bisect). Commit at least once per hour.

### Q6: What is the `rewind > correct` principle?

**Answer**: Instead of leaving failed attempts and corrections that pollute context, use `/rewind` (double-Esc) to go back to before the failed attempt, then re-prompt with what you learned. This keeps context clean and model performance high.

### Q7: What is the common pattern shared by all 10+ development workflows listed in the README?

**Answer**: Research → Plan → Execute → Review → Ship. Every workflow converges on this same architectural pattern, with variations in naming, granularity, and sub-loops (yellow-tagged sub-steps that repeat within a parent step).

### Q8: According to Thariq, what makes a good Skill description?

**Answer**: The description is a trigger for the model, not a summary for humans. Write it as "when should I fire?" Include the conditions that should cause automatic invocation. Don't state the obvious; focus on what pushes Claude out of default behavior.

### Q9: What are "tracer bullets" and why are they recommended over horizontal phasing?

**Answer**: From the Pragmatic Programmer concept cited by Matt Pocock: vertical slices that cross all layers (DB + service + UI) in one go. AI defaults to horizontal phasing (DB phase → API phase → frontend phase), which delays end-to-end feedback until the last phase. Tracer bullets give working feedback immediately.

### Q10: What are the three cross-model integration mechanisms?

**Answer**: (1) Plugin — another model's CLI runs inside Claude Code via slash commands. (2) MCP — Claude Code calls another model as a tool through Model Context Protocol. (3) Router — Claude Code's API endpoint is swapped to a different provider.

---

## Stage 5: Advanced Topics

### Q1: What is the difference between the Agent SDK and CLI in terms of system prompts?

**Answer**: The key difference is in how they communicate with the model. The CLI uses a richer harness with more tool definitions and system-level context. The SDK is a programmatic interface for building custom agents, using the same underlying API but with different system prompt structures. The report in `reports/claude-agent-sdk-vs-cli-system-prompts.md` provides a detailed comparison.

### Q2: What is LLM "day-to-day degradation"?

**Answer**: The observed phenomenon where LLM output quality varies from day to day, even with identical prompts. This can manifest as changes in code style, accuracy, verbosity, or adherence to instructions. The report `reports/llm-day-to-day-degradation.md` documents observed patterns.

### Q3: What are Agent Teams and how do they work?

**Answer**: Agent Teams allow multiple Claude Code instances to work in parallel, coordinated through a central session. They use tmux for terminal multiplexing and git worktrees for isolated workspaces. Each agent can work on a different task simultaneously. Controlled via the `CLAUDE_CODE_AGENT_TEAM` environment variable.

### Q4: What is the difference between `/loop` and `/schedule`?

**Answer**: `/loop` is for local recurring tasks (runs on your machine, up to 7 days auto-expiry). `/schedule` (Routines) is for cloud-based recurring tasks that run even when your machine is off. Use `/loop` for active development monitoring; use `/schedule` for ongoing recurring checks.

### Q5: What is the Goal mechanism (`/goal`) in Claude Code?

**Answer**: `/goal <condition>` sets a target condition that Claude works toward achieving. `/goal clear` removes it. It provides a persistent objective across turns, helping Claude stay focused on a long-running task.

### Q6: What three browser automation approaches are compared in the reports?

**Answer**: (1) Claude in Chrome — Claude Code's built-in Chrome integration via `--chrome` flag and Chrome extension. (2) Playwright MCP — Microsoft's Playwright browser automation as an MCP server. (3) Chrome DevTools MCP — Google's Chrome DevTools protocol via MCP. The report compares their strengths, weaknesses, and use cases.

### Q7: What is the "Spinner Verbs" report about?

**Answer**: It documents the spinner/status messages and tips extracted from the Claude Code CLI binary itself. These are internal messages that Claude displays during various operations, providing insights into what the CLI is doing internally.

### Q8: What are Scheduled Tasks and where can they be used?

**Answer**: Scheduled Tasks allow recurring prompts to execute at specified intervals. Two variants: `/loop` for in-session recurring tasks (dynamic or fixed interval), and `/schedule` for cloud-based Routines. Desktop scheduled tasks also available. Use cases include: monitoring CI pipelines, periodic PR checks, scheduled code reviews.

### Q9: What is Agent Memory and how does it differ from CLAUDE.md?

**Answer**: Agent Memory (`~/.claude/projects/<project>/memory/`) is a per-project, file-based persistent memory system where Claude stores and retrieves facts. Each memory is one file with YAML frontmatter (`name`, `description`, `metadata.type`). Unlike CLAUDE.md (static instructions), Agent Memory is dynamic — Claude writes, updates, and recalls memories as it learns. Supports three scopes: `user`, `project`, `local`.

### Q10: What does the repository say about "why harness is important"?

**Answer**: The harness (Claude Code's runtime) provides deterministic enforcement of behaviors that CLAUDE.md can only suggest. Settings, permissions, hooks, sandboxing — these are system-level constraints that the model cannot ignore. The report argues that many "Claude ignored my instructions" problems are actually harness configuration problems.

---

## Stage 6: Practical Application & Community

### Q1: What should a well-structured CLAUDE.md contain?

**Answer**: Repository overview, key components/critical patterns, setup/build/test commands, and project-specific conventions. Should target under 200 lines. Should enable any developer to launch Claude and run tests on the first try. Avoid putting harness-enforceable behaviors in CLAUDE.md.

### Q2: What is the recommended daily routine for a Claude Code user?

**Answer**: (1) Update Claude Code daily (`claude --version` to check). (2) Read the changelog at the start of each day. (3) Check context usage frequently with `/context`. (4) Use `/compact` proactively before auto-compact fires.

### Q3: What are the key channels for staying up-to-date with Claude Code?

**Answer**: r/ClaudeAI and r/ClaudeCode on Reddit; @claudeai, @ClaudeDevs, @bcherny, @trq212, @_catwu on X; Anthropic's YouTube channel; the official changelog on GitHub; the official docs at code.claude.com/docs.

### Q4: What is the `/new task = new session` principle?

**Answer**: Related tasks (like writing docs for what was just built) can reuse context for efficiency. But genuinely new, unrelated tasks deserve a fresh session to avoid context pollution and model confusion from irrelevant conversation history.

### Q5: What is the "use subagents" principle and when should you apply it?

**Answer**: Offload complex subtasks to subagents to keep the main context clean. Ask yourself: "Will I need this tool output again, or just the conclusion?" If you only need the conclusion, use a subagent. This way 20 file reads + 12 greps + 3 dead ends stay in the child's context, and only the final report returns.

### Q6: What tools does Boris recommend for Claude Code development (vs IDEs)?

**Answer**: Terminal-based tools: iTerm, Ghostty, or tmux instead of VS Code or Cursor. Voice dictation via `/voice` or Wispr Flow for 10x productivity. Status line for context awareness and fast compacting.

### Q7: What are the Privacy & Security considerations mentioned in the repository?

**Answer**: Use `/permissions` with wildcard syntax instead of `dangerously-skip-permissions`. Use auto mode instead of bypassing permissions. Use `/sandbox` to reduce prompts with file/network isolation. Route permission requests to Opus via a hook for attack scanning before auto-approving.

### Q8: According to the text, what happened to certain startups/businesses when Claude Code added their features natively?

**Answer**: The README's "Startups/Businesses" table documents cases where Claude Code's built-in features replaced third-party tools: Code Review (replaced Greptile, CodeRabbit, Devin Review), Voice Dictation (replaced Wispr Flow, SuperWhisper), Remote Control (replaced OpenClaw), Claude in Chrome (replaced Playwright MCP, Chrome DevTools MCP as standalone products), Agent SDK (replaced LangChain, CrewAI, AutoGen), Design (replaced Figma, Framer), Skills/Plugins (replaced YC AI wrapper startups).

### Q9: What should you do after completing this learning plan?

**Answer**: Apply knowledge to a real project, join the community, stay updated daily, consider the "Billion-Dollar Questions" in the README, and continuously refine your CLAUDE.md, skills, and workflows as Claude Code evolves.

### Q10: What are the "Billion-Dollar Questions" in the README?

**Answer**: Open questions organized in four categories: Memory & Instructions (4 questions about CLAUDE.md), Agents/Skills/Workflows (6 questions about when to use which primitive), Specs & Documentation (3 questions about spec maintenance), and one philosophical question: "Does code matter?" These represent unsolved problems in the AI-assisted development workflow.
