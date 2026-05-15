# Claude Certified Architect — Foundations
## 4-Week Team Study Plan

This plan follows a **narrative learning order** — concepts build on each other rather than mirroring the guide's chapter sequence. Each week has a central theme, with daily sessions designed for ~60–90 minutes of focused study. Each week has its own detailed guide with per-concept exercises, discussion questions, and a capstone project.

---

## Overview

| Week | Theme | Exam Domains | Detail |
|---|---|---|---|
| 1 | Foundations — Talking to Claude and Getting Reliable Output | Domain 4 (20%), Domain 5 (15%) | [Week1.md](Week1.md) |
| 2 | Building Agents — Systems That Act Autonomously | Domain 1 (27%), Domain 2 (18%) | [Week2.md](Week2.md) |
| 3 | Developer Workflows and Reliability | Domain 3 (20%), Domain 5 (15%) | [Week3.md](Week3.md) |
| 4 | Advanced Architecture and Exam Preparation | Domain 1 (27%), Domain 4 (20%), Domain 5 (15%) | [Week4.md](Week4.md) |

---

## Week 1: Foundations
**[→ Full week guide](Week1.md)**

Before building anything complex, master how Claude receives instructions, decides what to do, and returns structured, usable results.

| Day | Topic | Guide chapters |
|---|---|---|
| 1 | How Claude Actually Works (API Fundamentals) | Ch 1 |
| 2 | Giving Claude Abilities: Tools and `tool_use` | Ch 2 (2.1–2.3) |
| 3 | Prompt Engineering: Making Claude Do It Well | Ch 6 (6.1–6.4) |
| 4 | Structured Output: Getting Reliable Data Back | Ch 2 (2.4–2.5), Ch 6 (6.5–6.6) |
| 5 | Review and Practice | Exam Qs 1–3, Domain 4 notes |

**Capstone:** End-to-end invoice extraction pipeline — schema design, tool definition, few-shot examples, retry prompts, and failure analysis.

---

## Week 2: Building Agents
**[→ Full week guide](Week2.md)**

Move from single API calls to multi-step autonomous agents and multi-agent systems using the Agent SDK, hooks, and MCP.

| Day | Topic | Guide chapters |
|---|---|---|
| 6 | The Agentic Loop | Ch 3 (3.1–3.2) |
| 7 | Multi-Agent Systems: Coordinator and Subagents | Ch 3 (3.3–3.4) |
| 8 | Hooks: Deterministic Control Over Agent Behavior | Ch 3 (3.5), Domain 1 (1.5) |
| 9 | Connecting Claude to the World: MCP | Ch 4 |
| 10 | Review and Practice | Exam Qs 7–9, Domain 1 & 2 notes |

**Capstone:** Multi-agent customer support system — agent design, context passing, hook implementation, escalation logic, and failure analysis.

---

## Week 3: Developer Workflows and Reliability
**[→ Full week guide](Week3.md)**

Apply the Week 1–2 foundations to real engineering workflows with Claude Code, then tackle escalation, error handling, and context degradation.

| Day | Topic | Guide chapters |
|---|---|---|
| 11 | Claude Code: Configuration and Developer Workflows | Ch 5 (5.1–5.6) |
| 12 | Claude Code: Planning Mode, Session Management, and CI/CD | Ch 5 (5.6–5.10) |
| 13 | Escalation and Human-in-the-Loop | Ch 9 |
| 14 | Error Handling in Multi-Agent Systems | Ch 10 |
| 15 | Review and Practice | Exam Qs 4–6, 10–11, Domain 3 & 5 notes |

**Capstone:** Production-grade CI/CD code review pipeline — configuration design, CI commands, false positive management, re-review logic, and uncertainty routing.

---

## Week 4: Advanced Architecture and Exam Preparation
**[→ Full week guide](Week4.md)**

Context management at scale, task decomposition strategy, batch processing — then consolidate with the full 60-question practice exam.

| Day | Topic | Guide chapters |
|---|---|---|
| 16 | Context Management in Production Systems | Ch 11, 12 |
| 17 | Task Decomposition: Designing the Shape of Workflows | Ch 8 |
| 18 | Scale and Optimization: Batch Processing and Built-in Tools | Ch 7, 13 |
| 19 | Full Practice Exam (60 questions) | Practice Test section |
| 20 | Final Review and Exam Readiness | Full guide, targeted by Day 19 results |

**Capstone:** AI-powered research platform — full architecture design covering multi-agent topology, decomposition strategy, provenance, batch processing, and error handling.

---

## Concept-to-Chapter Quick Reference

| Concept | Chapter | Exam Domain |
|---|---|---|
| API request structure, `stop_reason` | Ch 1 | Domains 1, 5 |
| Tool definitions and `tool_choice` | Ch 2 | Domains 2, 4 |
| JSON schemas and structured output | Ch 2 | Domain 4 |
| Agentic loop and `AgentDefinition` | Ch 3 | Domain 1 |
| Coordinator/subagent and `Task` tool | Ch 3 | Domain 1 |
| Hooks (`PreToolUse`, `PostToolUse`) | Ch 3 | Domains 1, 5 |
| MCP: tools, resources, servers | Ch 4 | Domain 2 |
| CLAUDE.md hierarchy and `@path` | Ch 5 | Domain 3 |
| Skills, slash commands, planning mode | Ch 5 | Domain 3 |
| Claude Code CI/CD (`-p` flag) | Ch 5 | Domain 3 |
| Few-shot prompting and explicit criteria | Ch 6 | Domain 4 |
| Prompt chaining and retry loops | Ch 6 | Domains 1, 4 |
| Message Batches API | Ch 7 | Domain 4 |
| Fixed pipelines vs. dynamic decomposition | Ch 8 | Domain 1 |
| Escalation and handoff protocols | Ch 9 | Domains 1, 5 |
| Error categories and propagation | Ch 10 | Domains 2, 5 |
| Context management and scratchpads | Ch 11 | Domain 5 |
| Provenance and conflicting data | Ch 12 | Domain 5 |
| Built-in tools (Grep, Glob, Read, Edit) | Ch 13 | Domain 2 |
