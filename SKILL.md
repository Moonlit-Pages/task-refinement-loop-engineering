---
name: task-refinement-loop-engineering
title: Task Refinement Loop Engineering
description: >
  A general-purpose loop engineering skill for OpenClaw. It helps agents convert
  vague or broad user task descriptions into clear, phased, verifiable, and
  auditable execution plans, then iteratively complete the task through
  clarification, planning, execution, verification, audit, and revision loops.
version: 1.0.1
author: JACC / OpenClaw
category: workflow
tags:
  - task-refinement
  - loop-engineering
  - clarification
  - planning
  - execution
  - verification
  - audit
  - self-correction
  - multi-agent
  - openclaw

trigger:
  type: semantic
  conditions:
    - The user gives a broad, vague, incomplete, or complex task.
    - The task requires clarification before execution.
    - The task involves multiple stages or deliverables.
    - The task benefits from phased execution, verification, audit, and revision.
    - The user asks to improve, optimize, analyze, research, build, design, rewrite, plan, or automate something.

avoid_when:
  - The task is simple and can be answered directly.
  - The user only asks for a short factual answer.
  - The user explicitly requests no clarification and the task is already clear.
  - The task is unsafe, disallowed, or impossible to perform.

inputs:
  - user_task_description
  - optional_context
  - optional_files
  - optional_constraints
  - optional_success_criteria

outputs:
  - clarified_task_definition
  - clarification_questions
  - assumptions
  - phased_execution_plan
  - phase_outputs
  - verification_results
  - audit_results
  - revised_outputs
  - final_deliverable
  - final_audit_summary

default_mode: clarify_then_execute

modes:
  clarify_then_execute:
    description: Ask targeted clarification questions before building the execution plan.
  auto_execute:
    description: Proceed with reasonable assumptions when the user asks the agent to use best judgment.
  multi_agent:
    description: Route clarification, planning, execution, verification, audit, refinement, and finalization to specialized agents if available.

max_clarification_questions: 7
default_clarification_questions: 5
default_phase_count_min: 3
default_phase_count_max: 7

quality_gates:
  - task_definition_gate
  - phase_plan_gate
  - verification_gate
  - audit_gate
  - revision_gate
  - final_audit_gate

requires:
  - reasoning
  - task_decomposition
  - self_verification
  - output_audit

compatible_agents:
  - clarifier
  - planner
  - executor
  - verifier
  - auditor
  - refiner
  - finalizer

risk_controls:
  - Do not treat assumptions as facts.
  - Do not over-ask clarification questions.
  - Do not ask clarification questions when the user explicitly requests auto-execution, unless the task is impossible or unsafe.
  - Do not stop when reasonable assumptions can be made.
  - Do not skip verification for complex tasks.
  - Do not skip audit before final delivery.
  - Do not expose hidden reasoning.
  - Disclose unresolved assumptions and limitations.

status: active
---

# Loop Engineering Skill

## Purpose

This skill is used when the user gives a broad, unclear, complex, multi-step, or open-ended task.

The skill helps the agent convert a vague task description into a clear execution plan, then complete the task through an iterative loop:

Clarify → Define → Plan → Execute → Verify → Audit → Revise → Continue → Finalize

The goal is not to answer immediately, but to progressively engineer the task until the final result is complete, reliable, and aligned with the user's real intent.

---

## When to Use This Skill

Use this skill when the user's request is:

- Broad or vague
- Missing important constraints
- Multi-step or complex
- Related to research, writing, coding, automation, business planning, product analysis, process design, or document creation
- Likely to require multiple stages
- Likely to produce a better result after clarification and staged verification
- Asking for "help me build", "help me improve", "help me analyze", "make a plan", "create a workflow", "optimize", "audit", "rewrite", "research", or similar tasks

Do not use this skill for very simple tasks that can be answered directly.

---

## Core Principle

Do not treat the user's first task description as complete.

The first user input is only an initial task signal.

The agent must refine the task before execution unless the task is already sufficiently clear.

The agent should reduce ambiguity before doing heavy work. If the missing information is not critical, the agent should proceed with stated assumptions instead of asking unnecessary questions.

---

## Operating Loop

The agent must follow this loop:

1. Understand the user's initial task.
2. Identify missing information and ambiguity.
3. Ask only necessary clarification questions.
4. Convert the task into a structured objective.
5. Break the task into phases.
6. Execute one phase at a time.
7. Verify the output of each phase.
8. Audit for errors, gaps, contradictions, and user-fit.
9. Revise if needed.
10. Continue to the next phase.
11. Produce a final answer or deliverable.
12. Include a final audit summary.

---

## Step 1: Task Intake

When the user gives a task, extract the following:

- Main goal
- Expected output
- Target audience
- Use case
- Required format
- Deadline or urgency, if mentioned
- Constraints
- Available inputs
- Missing information
- Success criteria
- Risk areas

If any of these are unclear and important, ask the user targeted questions. If the unclear information is not critical, proceed using stated assumptions.

---

## Step 2: Clarification Questions

The agent should ask clarification questions only when the answers would materially improve the result.

Do not ask too many questions.

Default limit: 3 to 5 questions. Maximum: 7 questions.

The questions should be practical and task-specific.

Avoid generic questions such as:
- "Can you provide more details?"
- "What do you want?"
- "Please clarify."

Instead, ask precise questions.

Example:

Bad:
"What style do you want?"

Good:
"Should the output be written for internal execution, customer-facing use, or investor-facing use?"

Bad:
"What format do you need?"

Good:
"Do you want the result as a checklist, SOP, prompt, table, email draft, code structure, or full document?"

---
---

## Auto-Execution Mode

If the user says:

* "直接做"
* "不要问，按你的判断做"
* "use your best judgment"
* "proceed directly"
* "no need to ask me"

Then the agent should not ask clarification questions unless the task is impossible or unsafe.

Instead, the agent should:

1. State assumptions.
2. Build the task definition.
3. Create phases.
4. Execute the task.
5. Verify and audit internally.
6. Deliver the final result.

Format:

```text
I will proceed with these assumptions:
1. ...
2. ...
3. ...

Task Definition:
...

Phase Plan:
...

Execution:
...
```


## Step 3: Clarification Question Types

Use these question categories when needed:

### A. Goal Clarification

Ask what the user wants to achieve.

Examples:
- "Is the goal to make a decision, create a deliverable, automate a process, or diagnose a problem?"
- "Should the result be strategic, operational, technical, or customer-facing?"

### B. Output Format

Ask what the final deliverable should look like.

Examples:
- "Should I produce a final report, step-by-step workflow, implementation prompt, spreadsheet structure, code, or checklist?"
- "Should the output be concise or detailed enough for direct execution?"

### C. Audience

Ask who will use or read the output.

Examples:
- "Is this for yourself, your team, a customer, a supplier, or an AI agent?"
- "Should the language be business-friendly, technical, academic, or informal?"

### D. Constraints

Ask about limits.

Examples:
- "Are there any tools, platforms, budget limits, file formats, compliance rules, or style requirements I must follow?"
- "Should I avoid certain assumptions or methods?"

### E. Inputs

Ask what materials are available.

Examples:
- "Do you already have source documents, examples, previous versions, data, or screenshots?"
- "Should I work only from the information you provide, or may I use external research?"

### F. Success Criteria

Ask how the user will judge the result.

Examples:
- "What would make the final result useful enough to implement immediately?"
- "Should I prioritize speed, accuracy, completeness, cost reduction, or simplicity?"

---

## Step 4: If the User Does Not Answer

If the user does not answer clarification questions, the agent must not stop.

The agent should proceed using reasonable assumptions.

The agent must clearly state the assumptions before execution.

Format:

```text
I will proceed with these assumptions:
1. ...
2. ...
3. ...
```

Then continue the task.

---

## Step 5: Task Definition

After clarification or assumption-setting, rewrite the user's task into a clear task definition.

The task definition must include:

* Objective
* Scope
* Inputs
* Output
* Constraints
* Success criteria

Example format:

```text
Task Definition

Objective:
...

Scope:
...

Inputs:
...

Output:
...

Constraints:
...

Success Criteria:
...
```

Do not skip this step for complex tasks.

---

## Step 6: Phase Planning

Break the task into phases.

Each phase must have:

* Phase name
* Purpose
* Actions
* Expected output
* Verification method
* Exit criteria

Example:

```text
Phase 1: Requirement Refinement
Purpose:
Actions:
Expected Output:
Verification:
Exit Criteria:
```

The agent should not create too many phases.

Default: 3 to 7 phases.

For small tasks, compress phase metadata into a short checklist instead of producing an unnecessarily heavy framework.

---

## Step 7: Execution Rules

The agent must execute phase by phase.

For each phase:

1. State the phase being executed.
2. Produce the phase output.
3. Self-check the output.
4. Audit the result.
5. Decide whether to:

   * proceed,
   * revise,
   * ask the user,
   * or stop due to insufficient information.

The agent should avoid jumping directly to the final answer when the task is complex.

---

## Step 8: Verification Gate

After each phase, the agent must verify the output against the phase objective.

Use this checklist:

* Does the output answer the phase objective?
* Is anything missing?
* Are there contradictions?
* Are there unsupported assumptions?
* Is the output too vague?
* Is the output usable for the next phase?
* Does it still match the user's intent?

If the answer is weak, revise before moving forward.

---

## Step 9: Audit Gate

The audit gate is stricter than verification.

The agent must audit for:

* Logic gaps
* Hallucinated details
* Unsupported claims
* Missing constraints
* Format mismatch
* User-intent drift
* Over-engineering
* Under-specification
* Repetition
* Conflicting instructions
* Failure to produce an actionable result

If problems are found, the agent must fix them before continuing.

---

## Step 10: Revision Loop

If verification or audit fails, the agent must revise.

The revision loop is:

1. Identify the issue.
2. Explain the issue briefly.
3. Revise the output.
4. Re-check the revised version.
5. Continue only after the issue is resolved.

Example:

```text
Issue found:
The output is too general and does not define measurable success criteria.

Revision:
...

Re-check:
The revised version now includes measurable criteria and can be used in the next phase.
```

---

## Step 11: User Checkpoint

For long or high-impact tasks, the agent should create checkpoints.

A checkpoint is used when:

* The task direction may affect the whole result
* Multiple valid approaches exist
* The user may need to approve scope
* The next step requires important assumptions
* The result may be expensive, risky, or time-consuming to implement

At checkpoints, the agent should ask a focused question.

Do not ask for approval after every small step.

Good checkpoint:

```text
Before I continue, there are two possible directions:
A. Build a simple workflow that is easy to execute manually.
B. Build a more automated workflow suitable for an AI agent.

Which direction should I use?
```

If the user has already made the direction clear, do not ask again.

---

## Step 12: Final Output

The final output must include:

1. Final deliverable
2. Explanation of how it satisfies the task
3. Assumptions made
4. Remaining risks or limitations
5. Optional next-step recommendation

Format:

```text
Final Deliverable

...

Why this meets the task

...

Assumptions

...

Remaining risks

...

Recommended next step

...
```

If the user requested only a direct deliverable, keep the explanation short.

---

## Step 13: Final Audit

Before sending the final result, the agent must run a final audit.

Final audit checklist:

* Did I answer the actual task?
* Did I refine vague instructions before execution?
* Did I avoid unsupported claims?
* Did I maintain the required format?
* Did I resolve contradictions?
* Did I produce something actionable?
* Did I avoid unnecessary complexity?
* Did I clearly state assumptions?
* Did I include unresolved limitations?

The final answer should not expose hidden reasoning, but it may include a short visible audit summary.

Example:

```text
Final Audit Summary:
- Task scope clarified.
- Output format aligned with requested use.
- Main risks and assumptions stated.
- Result is ready for direct implementation.
```

---

## Default Response Pattern

When this skill is triggered, use this structure:

```text
I understand the task as: ...

Before execution, I need to clarify a few points:
1. ...
2. ...
3. ...

If you do not answer, I will proceed with the following default assumptions:
1. ...
2. ...
3. ...
```

After the user answers, continue with:

```text
Task Definition
...

Execution Plan
Phase 1: ...
Phase 2: ...
Phase 3: ...

Now I will start Phase 1.
```

---

## Error Handling

If the task cannot be completed due to missing information, the agent should not simply stop.

It should provide the best possible partial result and clearly state what is missing.

Use this format:

```text
I can complete part of the task now, but the following information is missing:
1. ...
2. ...

Partial result:
...

To complete the remaining part, I need:
...
```

---

## Anti-Patterns

The agent must avoid:

* Answering a vague complex task immediately without clarification
* Asking too many broad questions
* Creating a plan but not executing
* Executing without verification
* Treating assumptions as facts
* Hiding uncertainty
* Producing generic output
* Repeating the user's wording without refinement
* Asking the same question again after the user already answered
* Over-polishing the process instead of completing the task
* Ending with only "let me know if you want me to continue"

---

## Quality Standard

The result should be:

* Clear
* Specific
* Actionable
* Phased
* Verifiable
* Audited
* Aligned with the user's real goal
* Ready for implementation or further execution

The agent should behave like a task engineer, not just a chatbot.

---

## Minimal Example

User:
"帮我优化客户开发流程"

Agent should not immediately write a generic process.

Agent should ask:

```text
I understand the task as: you want to improve your customer development workflow.

Before I design the workflow, I need to clarify:
1. Is this for B2B export sales, domestic sales, or online leads?
2. Should the workflow be used by humans, AI agents, or both?
3. Which stage is the biggest problem now: finding leads, qualifying leads, writing emails, follow-up, CRM tracking, or conversion?
4. Do you want the final output as an SOP, checklist, automation prompt, or CRM pipeline?
5. Should I optimize for speed, personalization, conversion rate, or low manual workload?

If you do not answer, I will assume:
1. The workflow is for B2B export sales.
2. It will be used by both sales staff and AI agents.
3. The final output should be an SOP plus automation checklist.
```

Then continue only after user response or proceed using assumptions.

---

## Advanced Loop Template

For complex tasks, use this repeatable structure:

```text
Phase [Number]: [Name]

Goal:
...

Actions:
...

Output:
...

Verification:
- ...
- ...

Audit:
- ...
- ...

Decision:
Proceed / Revise / Ask User / Stop
```

---

## Completion Criteria

A task is complete only when:

* The final deliverable is produced
* The output matches the refined task definition
* All major assumptions are stated
* The result has passed verification
* The result has passed audit
* Remaining risks are disclosed
* The next action is clear


---

## Optional Multi-Agent Routing Module

If the OpenClaw system supports multiple agents, the Loop Engineering Skill may assign phases to specialized agents.

Possible roles:

### 1. Clarifier Agent

Responsible for:
- Understanding the user's initial request
- Identifying ambiguity
- Asking clarification questions
- Creating the refined task definition

### 2. Planner Agent

Responsible for:
- Breaking the task into phases
- Setting success criteria
- Defining verification gates
- Defining audit gates

### 3. Executor Agent

Responsible for:
- Completing each phase
- Producing concrete outputs
- Following the phase plan

### 4. Verifier Agent

Responsible for:
- Checking whether each phase output meets requirements
- Detecting missing information
- Checking factual, logical, and format accuracy

### 5. Auditor Agent

Responsible for:
- Finding deeper risks
- Detecting contradictions
- Checking user-intent alignment
- Identifying hallucination risks
- Checking whether the output is actually usable

### 6. Refiner Agent

Responsible for:
- Revising weak outputs
- Improving structure
- Removing vagueness
- Making the result more actionable

### 7. Finalizer Agent

Responsible for:
- Creating the final deliverable
- Summarizing assumptions
- Listing risks
- Producing final next steps

---

## Multi-Agent Loop

The default multi-agent loop is:

```text
User Task
↓
Clarifier Agent
↓
Planner Agent
↓
Executor Agent
↓
Verifier Agent
↓
Auditor Agent
↓
Refiner Agent if needed
↓
Executor Agent continues next phase
↓
Finalizer Agent
```

If verification or audit fails, return to the Refiner Agent or Planner Agent.

---

## Agent Handoff Format

When one agent hands work to another, use this format:

```text
Handoff To:
[Agent Name]

Context:
...

Current Phase:
...

Completed Work:
...

Open Issues:
...

Required Action:
...

Success Criteria:
...
```

---

## Escalation Rules

Escalate back to the user only when:

* The task has multiple valid directions and choosing wrong would waste work
* Required information is missing and cannot be reasonably assumed
* The task involves legal, financial, medical, compliance, or safety-sensitive decisions
* The user’s instruction conflicts with known constraints
* The agent cannot verify a key assumption

Otherwise, continue with best-effort assumptions.

---

## Multi-Agent Audit Requirement

Before final delivery, the Auditor Agent must check:

* Did the Clarifier Agent define the task correctly?
* Did the Planner Agent create realistic phases?
* Did the Executor Agent complete all required parts?
* Did the Verifier Agent catch surface-level issues?
* Did the Refiner Agent fix weak sections?
* Does the final output match the user's real business or technical goal?

Only after this audit may the Finalizer Agent produce the final response.
