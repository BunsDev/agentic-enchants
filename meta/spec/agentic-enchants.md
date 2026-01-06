# agentic-enchants

## Background

**agentic-enchants** is envisioned as an open-source, developer-centric knowledge and workflow system for systematically optimizing how humans and AI agents collaborate when building software systems.

The core motivation is to externalize and standardize _how_ developers interact with AI tools (LLMs, agents, MCPs, IDE agents like Cursor, etc.) by:

- Capturing reusable prompt templates (architecture design, system design, blockchain, infra, etc.)
- Storing behavioral rules (e.g., environment-specific agent behavior, language constraints, IDE rules)
- Organizing agent workflows and documentation templates
- Enabling both humans and agents to query and evolve this knowledge base over time

This effectively acts as a **meta-agentic layer**: a system that governs _how agents are instructed, composed, and reused_ across codebases and teams.

The system is intended to be:

- Programmatic (machine-readable, composable)
- Human-friendly (docs, templates, examples)
- Open-source and extensible

---

## Requirements

### Must Have

- Central repository for **agent templates** (prompts, instructions, roles)
- Support for **documentation templates** (architecture, system design, ADRs, etc.)
- Ability to store **behavioral rules** (Cursor rules, environment/language-specific constraints)
- Simple **query interface** to retrieve templates/rules
- Version-controlled and open-source

### Should Have

- Structured metadata for templates (domain, language, stack, maturity)
- Agent-readable format (YAML/JSON/Markdown hybrid)
- CLI or minimal UI for querying and adding content
- Contribution guidelines for external developers

### Could Have

- Agent self-referencing (agents suggest enchants to use)
- MCP registry and installer mappings
- Scoring or tagging system for template effectiveness

### Won’t Have (for MVP)

- Fully autonomous agent orchestration
- Complex UI/dashboard

---

## Method

### Atomic Enchant Model

An **Enchant** is the smallest reusable, composable unit of agentic knowledge. It must be:

- Human-readable
- Agent-readable
- Git-native
- Independently useful

Each enchant represents **one intent** (not a bundle).

#### Design Principles

- **Atomic**: does one thing well
- **Declarative**: describes _what_, not _how_
- **Context-aware**: scoped via metadata, not prose
- **Composable**: can be layered with other enchants

---

### Enchant File Structure

Each enchant is stored as a single file:

```
<domain>/<type>/<name>.enchant.md
```

Example:

```
architecture/prompts/system-design-api.enchant.md
```

---

### Enchant Schema (Frontmatter)

Each enchant uses YAML frontmatter as its constitutional schema:

```yaml
id: architecture.prompt.system-design-api
version: 0.1.0
type: prompt
status: draft

metadata:
  domain: architecture
  audience: [ai-agent, human]
  skill_level: intermediate
  applicable_stacks: [web, api]
  tools: [chatgpt, cursor, claude]

behavior:
  tone: precise
  verbosity: low
  assumptions_allowed: true

inputs:
  - name: system_context
    required: true
  - name: constraints
    required: false

outputs:
  format: markdown
  sections:
    - overview
    - components
    - tradeoffs
```

---

### Enchant Body

The body contains the _payload_:

```markdown
# System Design Prompt

You are a senior software architect...

<instructions here>
```

No metadata appears outside the frontmatter.

---

### Enchant Types (Initial)

- `prompt` – reusable LLM prompts
- `rule` – behavioral constraints (e.g., Cursor rules)
- `template` – documentation or spec templates
- `workflow` – ordered references to other enchants
- `mcp` – MCP install/config references

---

### Composition Model

Enchants do **not** import each other directly.

Composition happens via:

- Query results
- Workflow enchants (references by `id`)

This avoids tight coupling.

---

## Workflow Enchants

### Purpose

A **Workflow Enchant** is a declarative composition layer that references atomic enchants in a specific order to achieve a higher-level agentic outcome.

Workflows are the **only construct allowed to reference other enchants**.

They represent:

- Agent sessions
- Repeatable reasoning flows
- Meta-agent behavior

---

### Workflow Design Principles

- **Declarative**: no logic, only sequence and intent
- **Composable**: workflows reference atomic enchants by `id`
- **Readable**: understandable by humans without execution context
- **Executable by convention**: tools/agents decide _how_ to run them

---

### Workflow File Structure

```
workflows/<domain>/<name>.workflow.enchant.md
```

Example:

```
workflows/architecture/system-design-session.workflow.enchant.md
```

---

### Workflow Schema (Frontmatter)

```yaml
id: workflow.architecture.system-design-session
version: 0.1.0
type: workflow
status: stable

metadata:
  domain: architecture
  outcome: system-design-document
  estimated_steps: 5

steps:
  - id: architecture.prompt.system-design-api
    role: primary
  - id: architecture.template.system-design-doc
    role: output
  - id: rules.cursor.architecture-strict
    role: constraint

execution:
  mode: sequential
  stop_on_failure: true
```

---

### Workflow Body (Optional Narrative)

```markdown
# System Design Session Workflow

This workflow guides an AI agent through a structured system design session, producing a clear, tradeoff-aware architecture document.
```

Narrative is **optional** and non-authoritative.

---

### Workflow Semantics

- `steps` order matters
- `role` provides semantic hints (not execution rules)
- Agents decide how to:

  - Inject steps into context
  - Enforce constraints
  - Render outputs

The workflow **describes intent, not control flow**.

---

### Why Workflows Are Not Agents

Workflows:

- Do not store state
- Do not execute logic
- Do not branch

They are **recipes**, not programs.

---

## Implementation

_(Pending)_

## Milestones

_(Pending)_

## Gathering Results

_(Pending)_
