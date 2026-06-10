# OpenClaw & Hermes Agent Task Refinement Loop Engineering Skill

![Loop Engineering Workflow](assets/loop-engineering-workflow.png)

**Task Refinement Loop Engineering** is a general-purpose OpenClaw & Hermes-Agent skill that turns vague, broad, or complex user requests into clear, phased, verifiable, and auditable execution plans.

It is designed for AI agents that should not simply answer a vague prompt immediately. Instead, the agent clarifies the task, defines the objective, breaks the work into phases, executes each phase, verifies the result, audits for risks, revises weak outputs, and only then finalizes the deliverable.

## SEO Keywords

OpenClaw skill, Hermes Agent skill, loop engineering, task refinement loop, AI agent workflow, agent planning skill, prompt refinement, self-verifying AI agent, AI task decomposition, multi-agent workflow, autonomous agent audit loop, AI workflow automation, clarify execute verify audit, AI agent quality gates.

## Why This Skill Exists

Many AI agent failures come from the same problem: the initial user request is too vague.

Examples:

- "Help me optimize this process."
- "Build an automation workflow."
- "Analyze this document."
- "Improve this sales system."
- "Create a plan for this project."

A normal agent may start too early and produce generic output. This skill forces a better operating pattern:

```text
Clarify → Define → Plan → Execute → Verify → Audit → Revise → Finalize
```

The result is more specific, more reliable, and easier to implement.

## Key Features

- Converts vague user requests into structured task definitions
- Uses targeted clarification questions instead of generic follow-up questions
- Supports auto-execution when the user asks the agent to use best judgment
- Breaks complex work into 3 to 7 execution phases
- Adds verification gates after each phase
- Adds stricter audit gates before final delivery
- Supports revision loops when output quality is weak
- Supports multi-agent routing for clarifier, planner, executor, verifier, auditor, refiner, and finalizer roles
- Reduces hallucination risk by separating assumptions from facts
- Produces final deliverables with assumptions, risks, and next actions clearly stated

## Best Use Cases

This OpenClaw skill is useful for tasks involving:

| Use Case | Example User Request |
|---|---|
| Workflow design | "Help me build a customer follow-up workflow." |
| Business automation | "Create an AI agent process for sales outreach." |
| Document review | "Check this file for formatting and logical errors." |
| Research planning | "Help me investigate this market." |
| Prompt engineering | "Turn this rough instruction into a reliable agent prompt." |
| Multi-agent execution | "Split this task across planner, executor, and auditor agents." |
| SOP creation | "Create a process my team can follow." |
| Quality control | "Verify and audit this output before final delivery." |

## How It Works

The skill follows a controlled execution loop.

```text
1. Task Intake
2. Clarification Questions
3. Auto-Execution Decision
4. Task Definition
5. Phase Planning
6. Phase Execution
7. Verification Gate
8. Audit Gate
9. Revision Loop
10. User Checkpoint
11. Final Output
12. Final Audit
13. Optional Multi-Agent Routing
```

## Core Workflow

The default loop is:

```text
User Task
  ↓
Clarify intent and missing information
  ↓
Define objective, scope, inputs, output, constraints, and success criteria
  ↓
Create a phased execution plan
  ↓
Execute one phase at a time
  ↓
Verify phase output
  ↓
Audit for deeper risks
  ↓
Revise when needed
  ↓
Finalize the deliverable
```

## Installation

Copy the skill folder into your OpenClaw skills directory.

```bash
mkdir -p skills/task-refinement-loop
cp skills/task-refinement-loop/SKILL.md ./skills/task-refinement-loop/SKILL.md
```

Recommended repository structure:

```text
your-openclaw-project/
└── skills/
    └── task-refinement-loop/
        └── SKILL.md
```

## File Structure in This Repository

```text
task-refinement-loop/
├── README.md
├── assets/
│   ├── loop-engineering-workflow.svg
│   └── loop-engineering-workflow.png
└── skills/
    └── task-refinement-loop/
        └── SKILL.md
```

## Basic Usage

Use this skill when the user gives a broad or unclear task.

Example:

```text
User:
Help me optimize our B2B customer development process.

Agent:
I understand the task as improving your customer development workflow.
Before execution, I need to clarify:
1. Is this for B2B export sales, domestic sales, or online leads?
2. Should the workflow be used by humans, AI agents, or both?
3. Which stage is the biggest problem now: lead sourcing, qualification, outreach, follow-up, CRM tracking, or conversion?
4. Should the final output be an SOP, checklist, automation prompt, or CRM pipeline?

If you do not answer, I will proceed with reasonable assumptions.
```

## Auto-Execution Mode

If the user says:

```text
直接做
不要问，按你的判断做
use your best judgment
proceed directly
no need to ask me
```

Then the agent should not ask clarification questions unless the task is impossible or unsafe.

Instead, it should:

1. State assumptions
2. Build the task definition
3. Create phases
4. Execute the work
5. Verify and audit
6. Deliver the final result

## Quality Gates

This skill includes six main quality gates.

| Gate | Purpose |
|---|---|
| Task Definition Gate | Checks whether the objective, scope, input, output, constraints, and success criteria are clear |
| Phase Plan Gate | Checks whether the task is broken into realistic phases |
| Verification Gate | Checks whether each phase output meets the phase objective |
| Audit Gate | Checks for deeper risks, contradictions, unsupported assumptions, and user-intent drift |
| Revision Gate | Forces correction when verification or audit fails |
| Final Audit Gate | Confirms the final deliverable is complete, usable, and aligned with the task |

## Multi-Agent Routing

If your OpenClaw setup supports multiple agents, this skill can route work across specialized roles.

| Agent Role | Responsibility |
|---|---|
| Clarifier Agent | Understands the user request and asks targeted clarification questions |
| Planner Agent | Builds the phased execution plan and success criteria |
| Executor Agent | Completes each phase of the task |
| Verifier Agent | Checks whether phase outputs meet requirements |
| Auditor Agent | Finds deeper risks, gaps, contradictions, and hallucination risks |
| Refiner Agent | Revises weak or incomplete outputs |
| Finalizer Agent | Produces the final deliverable and final audit summary |

Recommended multi-agent loop:

```text
Clarifier → Planner → Executor → Verifier → Auditor → Refiner → Finalizer
```

## Configuration Highlights

The YAML frontmatter defines the skill behavior.

```yaml
name: task-refinement-loop
title: Task Refinement Loop Engineering
category: workflow
default_mode: clarify_then_execute
max_clarification_questions: 7
default_clarification_questions: 5
default_phase_count_min: 3
default_phase_count_max: 7
status: active
```

## When Not to Use This Skill

Do not use this skill when:

- The task is simple and can be answered directly
- The user only asks for a short factual answer
- The user has already provided a clear, narrow task
- The task is unsafe, disallowed, or impossible
- The extra loop would slow down a small task unnecessarily

## Example: Document Audit Task

User request:

```text
Please use loop engineering to fully check the attached file for formatting errors and other issues.
```

Agent behavior:

```text
Phase 1: Parse the file structure
Phase 2: Check YAML frontmatter
Phase 3: Check Markdown formatting
Phase 4: Check logical consistency
Phase 5: Check execution rules and quality gates
Phase 6: Revise errors
Phase 7: Run final gate review
Phase 8: Deliver the corrected file
```

## FAQ

### What is loop engineering?

Loop engineering is a controlled AI agent execution pattern where the agent repeatedly plans, executes, verifies, audits, and revises work until the result meets the task objective.

### Is this a prompt engineering skill?

Yes, but it is broader than prompt engineering. It is a workflow skill for task refinement, execution control, quality assurance, and multi-agent coordination.

### Can this skill work with a single agent?

Yes. The skill can run inside a single agent by treating clarification, planning, execution, verification, audit, and refinement as internal roles.

### Can this skill work with multiple agents?

Yes. The skill includes an optional multi-agent routing module.

### Does this skill force the agent to ask questions every time?

No. The agent asks questions only when the missing information materially affects the result. If the user requests direct execution, the agent proceeds with stated assumptions.

### What problem does this skill solve?

It prevents AI agents from producing generic answers to vague tasks. It creates a disciplined path from unclear request to verified final deliverable.

## Recommended GitHub Topics

Add these topics to your GitHub repository:

```text
openclaw
ai-agent
loop-engineering
task-refinement
agent-workflow
prompt-engineering
multi-agent
self-verification
workflow-automation
quality-gates
```

## License

Use the license that fits your OpenClaw project. If this is an internal business skill, keep it private. If you publish it publicly, consider MIT or Apache-2.0.

## Maintainer Notes

This skill is especially useful as a default intake layer for complex OpenClaw tasks. It can be paired with specialized downstream skills such as research, document audit, code review, sales automation, or multi-agent orchestration.



## Star History

<a href="https://www.star-history.com/?repos=Moonlit-Pages%2Ftask-refinement-loop-engineering&type=date&legend=top-left">
 <picture>
   <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/chart?repos=Moonlit-Pages/task-refinement-loop-engineering&type=date&theme=dark&legend=top-left" />
   <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/chart?repos=Moonlit-Pages/task-refinement-loop-engineering&type=date&legend=top-left" />
   <img alt="Star History Chart" src="https://api.star-history.com/chart?repos=Moonlit-Pages/task-refinement-loop-engineering&type=date&legend=top-left" />
 </picture>
</a>
