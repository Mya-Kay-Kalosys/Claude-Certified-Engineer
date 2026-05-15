# Claude Certified Architect — Foundations
## 4-Week Team Study Plan

This plan follows a **narrative learning order** — concepts build on each other rather than mirroring the guide's chapter sequence. Each week has a central theme. Daily sessions are designed for ~60–90 minutes of focused study, plus optional hands-on exercises.

---

## Week 1: Foundations — Talking to Claude and Getting Reliable Output

**Theme:** Before building anything complex, master the fundamentals of how Claude receives instructions, decides what to do, and returns structured, usable results. These skills underpin every other topic in the course.

---

### Day 1 — How Claude Actually Works (API Fundamentals)
**Guide reference:** Chapter 1

The starting point: Claude is a stateless function. Every call is independent, and you must send the full conversation history every time. This session reframes Claude from "chatbot" to "API endpoint with reasoning capabilities."

Key concepts:
- The anatomy of an API request: `model`, `system`, `messages`, `tools`, `max_tokens`
- Message roles: `user`, `assistant`, and how `tool_result` fits in
- The `stop_reason` field — the signal that controls all agent behavior (`end_turn`, `tool_use`, `max_tokens`, `stop_sequence`)
- The system prompt: what it is, how it differs from user messages, and why its wording matters more than you'd expect
- The context window: what goes in it, why it fills up, and the "lost-in-the-middle" effect

**Discussion question:** If Claude has no memory between calls, what are the implications for building a multi-turn customer support agent?

---

### Day 2 — Giving Claude Abilities: Tools and `tool_use`
**Guide reference:** Chapter 2 (sections 2.1–2.3)

Tools are how Claude takes action in the world. This session covers the mechanics of tool definitions and the critical insight that **tool descriptions are the primary selection mechanism** — not code, not routing logic.

Key concepts:
- What `tool_use` is and what it isn't (Claude generates a call request; your code executes it)
- Anatomy of a tool definition: `name`, `description`, `input_schema`
- Why vague descriptions cause misrouting — and how to write descriptions that clearly distinguish tools from one another
- The `tool_choice` parameter: `auto`, `any`, and forced selection — when and why to use each

**Discussion question:** You have two tools: `get_customer` and `lookup_order`. Their descriptions are nearly identical. What specific changes would you make to each description to eliminate confusion?

---

### Day 3 — Prompt Engineering: Making Claude Do It Well
**Guide reference:** Chapter 6 (sections 6.1–6.4)

You can't build reliable systems without knowing how to instruct Claude precisely. This session covers the core prompt engineering techniques that separate professionals from beginners.

Key concepts:
- **Few-shot prompting**: why 2–4 concrete examples outperform lengthy textual descriptions
- The five types of few-shot examples: ambiguous scenarios, output formatting, acceptable vs. problematic code, extraction from different document formats, informal measurements
- **Explicit criteria vs. vague instructions**: defining exact flag/no-flag boundaries rather than "be conservative"
- **Prompt chaining**: breaking complex tasks into focused sequential steps to avoid attention dilution
- **The interview pattern**: Claude asks clarifying questions before acting on underspecified requests

**Exercise:** Take a vague instruction ("review this code carefully") and rewrite it with explicit criteria, severity definitions with examples, and 2 few-shot examples.

---

### Day 4 — Structured Output: Getting Reliable Data Back
**Guide reference:** Chapter 2 (sections 2.4–2.5), Chapter 6 (sections 6.5–6.6)

This session ties tool use and prompt engineering together into a complete pattern: extracting structured, validated JSON from Claude that your code can actually consume.

Key concepts:
- JSON schemas as the most reliable structured output mechanism — what they guarantee (syntax) and what they don't (semantics)
- Schema design rules: required vs. optional fields, nullable fields, `enum` with `"other"` and `"unclear"` options
- The syntax vs. semantic error distinction — and why both matter
- **Retry-with-feedback loops**: when a validation error occurs, re-prompt with the original document + the specific error
- **Self-correction schemas**: extracting `stated_total` and `calculated_total` side-by-side to detect discrepancies automatically
- When retries help (format errors, structural mistakes) vs. when they don't (data simply isn't in the source)

**Discussion question:** A Pydantic validator catches that `sum(line_items) ≠ total`. What exactly do you include in the retry prompt, and what do you leave out?

---

### Day 5 — Week 1 Review and Practice
**Guide reference:** Exam Questions 1–3, Domain 4 notes (sections 4.1–4.4)

Consolidate the week's material by working through exam-style scenarios together. Focus on the reasoning behind each answer, not just the correct choice.

- Work through exam questions 1–3 as a group
- Review Domain 4 notes (Prompt Engineering and Structured Output) in Part II
- Each team member identifies one concept they want to revisit before Week 2

---

## Week 2: Building Agents — Systems That Act Autonomously

**Theme:** Move from single API calls to multi-step autonomous agents and multi-agent systems. This week introduces the Agent SDK, the coordinator/subagent pattern, hooks, and MCP for connecting Claude to external systems.

---

### Day 6 — The Agentic Loop
**Guide reference:** Chapter 3 (sections 3.1–3.2)

The agentic loop is the core pattern that makes Claude autonomous. This session builds intuition for how agents actually run and establishes the correct mental model for all the complexity that follows.

Key concepts:
- The loop in detail: send request → check `stop_reason` → execute tool → append result → repeat
- Why `stop_reason == "end_turn"` is the only reliable completion signal (not text parsing, not iteration limits)
- `AgentDefinition`: `name`, `description`, `system_prompt`, `allowed_tools`
- The principle of least privilege: only give an agent the tools it actually needs
- Anti-patterns to memorize: parsing assistant text for completion, arbitrary `max_iterations` as a stop condition

**Exercise:** Sketch the full message history (as a JSON array) after a 2-tool-call agent loop. What does each message look like?

---

### Day 7 — Multi-Agent Systems: Coordinator and Subagents
**Guide reference:** Chapter 3 (sections 3.3–3.4)

One agent is powerful. A team of agents — each specialized, each with isolated context — is how you tackle complex real-world tasks. This session covers the hub-and-spoke architecture in depth.

Key concepts:
- Hub-and-spoke topology: the coordinator owns all routing, error handling, and aggregation
- **Critical principle:** subagents have isolated context — they do not inherit the coordinator's history
- How to pass context explicitly: the difference between `Task: "Analyze the document"` (bad) and including the full document, prior results, and output format in the prompt (good)
- Parallel subagent spawning: multiple `Task` calls in a single coordinator response run concurrently
- The coordinator's responsibilities: task decomposition, delegation, result aggregation, error handling, communication

**Discussion question:** A coordinator decomposes "research AI impact on creative industries" into three subtasks — all about visual art. What went wrong, and how would you fix the coordinator's system prompt?

---

### Day 8 — Hooks: Deterministic Control Over Agent Behavior
**Guide reference:** Chapter 3 (section 3.5), Domain 1 (section 1.5)

Hooks are the mechanism for guaranteeing behavior — not hoping for it. This is one of the most important distinctions in the entire exam.

Key concepts:
- `PostToolUse` hooks: intercept and transform tool results before Claude sees them (e.g., normalize date formats from inconsistent MCP tools)
- `PreToolUse` hooks: intercept and block outgoing tool calls (e.g., block refunds above $500 and redirect to escalation)
- **The fundamental rule:** hooks give **deterministic** guarantees; prompt instructions give **probabilistic** compliance
- When to use hooks vs. prompts: financial operations, compliance rules, and safety constraints require hooks

**Discussion question:** Your system prompt says "always verify the customer before processing a refund." Incidents show this is violated 8% of the time. What is the correct fix, and why won't improving the prompt solve it?

---

### Day 9 — Connecting Claude to the World: MCP
**Guide reference:** Chapter 4

MCP (Model Context Protocol) is how you connect Claude to your real systems — databases, APIs, internal tools. This session covers the protocol's structure and the configuration decisions that affect security and team workflows.

Key concepts:
- MCP's three resource types: **Tools** (actions), **Resources** (read-only context), **Prompts** (templates)
- MCP servers: how they're discovered and why all connected tools are available simultaneously
- `.mcp.json` (project scope, version-controlled, shared with team) vs. `~/.claude.json` (personal, not shared)
- Environment variable substitution for secrets: `${GITHUB_TOKEN}` — never commit tokens
- The `isError` flag and why structured error responses are critical for agent recovery
- MCP Resources as "maps": providing schemas and catalogs so the agent doesn't need exploratory tool calls

**Exercise:** Design the `.mcp.json` for a team project connecting to GitHub and Jira. Where do tokens go, and why?

---

### Day 10 — Week 2 Review and Practice
**Guide reference:** Exam Questions 7–9, Domain 1 and Domain 2 notes

- Work through exam questions 7–9 as a group (all multi-agent scenarios)
- Review Domain 1 (Agent Architecture) and Domain 2 (Tool Design and MCP) notes in Part II
- Each team member writes one hook — either a `PreToolUse` or `PostToolUse` — for a scenario they've encountered at work

---

## Week 3: Developer Workflows and Reliability

**Theme:** Apply everything from Weeks 1–2 to real engineering workflows using Claude Code. Then tackle the hard problems: what happens when things go wrong, when users need a human, and when context degrades over long sessions.

---

### Day 11 — Claude Code: Configuration and Developer Workflows
**Guide reference:** Chapter 5 (sections 5.1–5.6)

Claude Code is where these concepts become daily tools for engineers. This session covers the configuration system that makes Claude Code consistent and team-friendly.

Key concepts:
- The CLAUDE.md hierarchy: user-level (`~/.claude/CLAUDE.md`) vs. project-level (`.claude/CLAUDE.md`) vs. directory-level — and why the wrong level means teammates don't get the instructions
- `@path` syntax: referencing external files to keep CLAUDE.md modular
- `.claude/rules/` with YAML frontmatter `paths`: loading conventions only when editing matching files (ideal for tests scattered across a codebase)
- Custom slash commands and skills: project commands in `.claude/commands/` (shared via VCS) vs. personal commands in `~/.claude/commands/`
- Skill frontmatter: `context: fork` (isolated subagent), `allowed-tools` (security), `argument-hint` (UX)

**Discussion question:** A new team member clones the repo but Claude Code doesn't follow any of the project conventions. What's the most likely cause?

---

### Day 12 — Claude Code: Planning Mode, Session Management, and CI/CD
**Guide reference:** Chapter 5 (sections 5.6–5.10)

The second half of Claude Code covers the strategic decisions: when to plan vs. act, how to manage long sessions, and how to wire Claude into automated pipelines.

Key concepts:
- **Planning mode vs. direct execution**: plan for large changes, multiple viable approaches, architectural decisions; execute directly for single-file fixes with a clear stack trace
- `/compact` for compressing context, `/memory` for persisting notes across sessions
- `--resume <session-name>` for continuing named sessions; `fork_session` for branching exploration
- When to start a new session rather than resuming (stale tool results, degraded context)
- The `-p` flag for CI/CD: non-interactive mode, `--output-format json`, `--json-schema`
- Session isolation for review: the same instance that wrote the code is less effective at reviewing it

**Exercise:** Write the Claude Code CI step for a GitHub Actions workflow that reviews every pull request and outputs structured JSON for inline PR comments.

---

### Day 13 — Escalation and Human-in-the-Loop
**Guide reference:** Chapter 9

Agents must know when to stop trying and hand off to a human. This session covers escalation design — one of the most nuanced judgment areas on the exam.

Key concepts:
- Clear escalation triggers: explicit request for a human, policy gap, inability to make progress, financial threshold (enforced via hook)
- Unreliable escalation proxies: sentiment analysis, model self-rated confidence scores — both fail as proxies for case complexity
- Three escalation patterns: immediate (customer explicitly asks), attempt-then-escalate (try within scope first), acknowledge-resolve-escalate on reiteration
- **Structured handoff protocols**: what the JSON summary passed to a human agent must contain — and why it must be self-contained
- Handling ambiguity: multiple customer matches → ask for additional identifiers, never guess

**Discussion question:** A customer says "this is outrageous, I'm furious!" Should the agent escalate immediately? Walk through the correct decision logic step by step.

---

### Day 14 — Error Handling in Multi-Agent Systems
**Guide reference:** Chapter 10

When a subagent fails, what the coordinator knows determines whether the system recovers gracefully or collapses entirely. This session is about designing for failure.

Key concepts:
- The four error categories: transient (retry with backoff), validation (fix input and retry), business (explain and offer alternatives), permission (escalate)
- Why generic errors ("search unavailable") are an anti-pattern — the coordinator can't decide what to do
- Distinguishing "no results found" from "search failed" — these require different coordinator responses
- Anti-patterns: aborting the whole workflow on one failure, infinite retries inside a subagent, silent suppression (returning empty results as success)
- The structured subagent error: `failure_type`, `attempted_query`, `partial_results`, `alternative_approaches`, `coverage_impact`
- Coverage annotations in final synthesis: label what is well-supported vs. what has gaps

**Exercise:** Design a structured error response for a document analysis subagent that failed because a PDF was password-protected. What does the coordinator need to recover intelligently?

---

### Day 15 — Week 3 Review and Practice
**Guide reference:** Exam Questions 4–6, 10–11, Domain 3 and Domain 5 notes

- Work through exam questions 4–6 (Claude Code scenarios) and 10–11 (CI/CD scenarios)
- Review Domain 3 (Claude Code) and Domain 5 (Context Management and Reliability) notes in Part II
- Team exercise: map your current work processes to Claude Code workflows — where would planning mode apply? Where would CI integration add value?

---

## Week 4: Advanced Architecture and Exam Preparation

**Theme:** Tackle the "systems thinking" topics — context management over long sessions, task decomposition strategy, and batch processing at scale — then consolidate everything through the full practice exam.

---

### Day 16 — Context Management in Production Systems
**Guide reference:** Chapters 11 and 12

Systems that work in demos often degrade in production. This session is about what goes wrong with context at scale and the patterns that prevent it.

Key concepts:
- Progressive summarization risk: numeric values, percentages, and dates get condensed into "roughly" and "about"
- The lost-in-the-middle effect: place critical findings at the beginning and action items at the end
- Extracting transactional facts into a persistent "case facts" block outside summarized history
- Trimming tool results with `PostToolUse` hooks: if `lookup_order` returns 40 fields and you need 5, trim before Claude sees it
- Scratchpad files: writing key findings to a file that survives context resets
- Delegating to subagents to protect the coordinator's context window

**Provenance (Chapter 12):**
- The attribution loss problem: claims without sources are useless in multi-source synthesis
- Preserving `claim → source` mappings through aggregation and summarization
- Handling conflicting data: annotate both values with attribution, let the coordinator reconcile
- Including publication dates to avoid reading temporal differences as contradictions
- Render by content type: financial data as tables, news as prose, technical findings as lists

---

### Day 17 — Task Decomposition: Designing the Shape of Workflows
**Guide reference:** Chapter 8

With all the building blocks in place, this session takes the architect's perspective: how do you decide what shape a workflow should take?

Key concepts:
- **Fixed pipelines (prompt chaining)**: each step defined in advance, predictable structure, stable and reproducible
  - Best for: code reviews, document extraction, migrations with a known template
- **Dynamic adaptive decomposition**: subtasks emerge from intermediate results
  - Best for: open-ended investigations, legacy codebases with unknown scope, when "what to do next" depends on what you just found
- **Multi-pass code review**: per-file local analysis first, then a separate integration pass for cross-file dependencies — why single-pass over 14 files produces inconsistent results
- When the coordinator over-decomposes: assigning too-narrow subtasks misses coverage (the "only visual art" problem)

**Discussion question:** You're asked to add test coverage to a legacy codebase with no tests. Walk through how you'd design this as a dynamic adaptive decomposition. What does each phase discover, and how does it shape the next phase?

---

### Day 18 — Scale and Optimization: Batch Processing and Built-in Tools
**Guide reference:** Chapters 7 and 13

Two efficiency topics that round out the architect's toolkit: the Batches API for cost-effective bulk processing, and the built-in tool reference for precise codebase investigation.

Key concepts from Chapter 7 (Message Batches API):
- 50% cost savings, up to 24-hour processing window, no latency SLA
- When to use batch vs. synchronous: batch for overnight reports and weekly audits; synchronous for anything a developer is actively waiting on
- `custom_id` for correlating results and re-submitting only failed documents
- SLA planning: if you need results in 30 hours, batches must be submitted 6 hours in advance
- Batch API does not support multi-turn tool calling within a single request

Key concepts from Chapter 13 (Built-in Tools):
- Glob for finding files by pattern; Grep for finding content within files — don't conflate them
- Read for full file access; Edit for precise targeted changes; Bash for running commands
- Incremental investigation: Grep entry points → Read found files → Grep usages → Read consumers → repeat
- Edit fallback: when text match is non-unique, fall back to Read → modify → Write

---

### Day 19 — Full Practice Exam (60 Questions)
**Guide reference:** Practice Test section

Work through the full 60-question practice test individually or in small groups. Time yourselves: the real exam is paced at roughly 1.5 minutes per question.

After completing the test:
- Identify which domains you scored lowest in
- Return to the relevant Domain notes in Part II for targeted review
- Note which scenarios (Customer Support, Multi-agent Research, Claude Code, CI/CD, Structured Data) feel weakest

**Exam logistics reminder:**
- Score of 720/1000 to pass
- No guessing penalty — answer every question
- 4 of 8 scenarios are randomly selected on the real exam

---

### Day 20 — Final Review and Exam Readiness
**Guide reference:** Full guide review; focus areas based on Day 19 results

The final session is learner-driven based on what the practice exam revealed.

Suggested structure:
- First 30 minutes: each team member shares their two weakest areas and one thing they'll do differently as a result of this course
- Next 45 minutes: work through remaining exam questions (Questions 5–12 from the "Examples" section and any missed practice test questions)
- Final 15 minutes: review the exam format, scenario list, and confirm logistics

**Key reminders for exam day:**
- Hooks → deterministic; prompts → probabilistic. This distinction appears in multiple scenarios.
- `stop_reason == "end_turn"` is the only reliable completion signal.
- Tool descriptions are a selection mechanism, not documentation.
- Subagents do not inherit coordinator context — always pass it explicitly.
- Batch API = overnight/non-blocking only. Never for developer-blocking workflows.
- `.mcp.json` = team/VCS; `~/.claude.json` = personal/experimental.

---

## Appendix: Concept-to-Chapter Quick Reference

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
| Error categories and propagation | Ch 10 | Domain 2, 5 |
| Context management and scratchpads | Ch 11 | Domain 5 |
| Provenance and conflicting data | Ch 12 | Domain 5 |
| Built-in tools (Grep, Glob, Read, Edit) | Ch 13 | Domain 2 |
