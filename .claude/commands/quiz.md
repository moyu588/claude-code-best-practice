---
description: Quiz yourself on Claude Code knowledge after completing a learning stage
argument-hint: [stage-number]
model: sonnet
---

Quiz the user on Claude Code best practices based on the learning stages defined in LEARNING-PLAN.md and the quiz questions in the learning-quiz skill.

Use the Skill tool to invoke `learning-quiz` with the stage number argument provided by the user.

If no stage number is provided, ask the user which stage (1-6) they want to be quizzed on:
- Stage 1: Environment Setup & Warmup
- Stage 2: Commands, Agents & Skills
- Stage 3: Harness Configuration System
- Stage 4: Workflow Methodologies
- Stage 5: Advanced Topics
- Stage 6: Practical Application & Community
