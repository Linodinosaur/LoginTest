# INSTRUCTIONS.md — Agent Operating Guide

This file governs how AI coding agents initialize, build, and maintain software projects. Follow it as the single source of operational truth.

**Intended workflow:** The user primes the agent with a general idea (e.g. "addon based on my previous application"). The agent then asks for more details before creating or changing project files, and the project is built from there.

---

## 1. Required Project Files

Every project must contain the following **six** files at its root. If any are missing when you begin work, create them immediately. The structure is stack- and domain-agnostic (e.g. web app, CLI, game addon); adapt the *contents* of each file to the project type, not the file names or roles.

### AGENTS.md

- **Purpose:** Defines which agents operate in this repo, their roles, tool access, and coordination rules.
- **Contents:**
  - Agent identifiers and their responsibilities (e.g., "coding agent," "review agent").
  - Tool and environment constraints (e.g., allowed CLI commands, API access).
  - Handoff protocols between agents if multiple are involved.
  - Rules for conflict resolution (e.g., which agent owns architectural decisions).
- **Update when:** A new agent role is introduced, tool access changes, or coordination rules shift.
- **Detail level:** Brief and functional. Think "role card," not essay.
- **Avoid:** Describing agent internals or capabilities already documented elsewhere.

### PRD.md (Product Requirements Document)

- **Purpose:** Captures *what* is being built and *why*. This is the product-level source of truth.
- **Contents:**
  - Problem statement (1–3 sentences).
  - Target users and core use cases.
  - Functional requirements (what the system must do).
  - Non-functional requirements (performance, security, accessibility constraints).
  - Out-of-scope items (explicit boundaries).
  - Success criteria or acceptance conditions.
- **Update when:** Requirements change, scope shifts, or new features are formally added.
- **Detail level:** Precise enough that an agent can determine whether a feature request is in scope without asking.
- **Avoid:** Implementation details. That belongs in ARCHITECTURE.md or PLAN.md.

### PLAN.md

- **Purpose:** Defines *how* and *in what order* the work gets done. This is the execution roadmap.
- **Contents:**
  - Ordered list of milestones or phases.
  - Current phase clearly marked.
  - Task breakdown within the current phase (checklist format).
  - Dependencies between tasks.
  - Decisions made and their rationale (brief).
  - Known blockers or open questions.
- **Update when:** A task is completed, a phase changes, priorities shift, or a blocker is resolved.
- **Detail level:** Granular for the current phase; high-level for future phases.
- **Avoid:** Duplicating requirements from PRD.md. Reference it instead.

### ARCHITECTURE.md

- **Purpose:** Describes the system's technical design — the structural "map" of the codebase.
- **Contents:**
  - High-level system diagram (described textually or with Mermaid/ASCII if helpful).
  - Core components and their responsibilities.
  - Data flow between components.
  - Technology choices with brief justification.
  - Directory/module structure overview.
  - Key interfaces, contracts, or API boundaries.
  - External service integrations.
- **Update when:** A new component is added, a technology choice changes, or the data flow is restructured.
- **Detail level:** Enough for an agent to know where to put new code and how it connects to the rest.
- **Avoid:** Line-level code documentation. That belongs in DOCUMENTATION.md or inline comments.

### DOCUMENTATION.md

- **Purpose:** Central reference for developer-facing docs — setup, conventions, and operational knowledge.
- **Contents:**
  - Setup and installation instructions.
  - Environment variables and configuration.
  - Build, test, and deploy commands.
  - Code conventions and style rules.
  - Error handling patterns used in the project.
  - Common pitfalls or gotchas.
  - Links to external documentation (APIs, libraries) if relevant.
- **Update when:** Setup steps change, new conventions are adopted, or a recurring mistake warrants a note.
- **Detail level:** Step-by-step where precision matters (setup), concise elsewhere.
- **Avoid:** Tutorials or onboarding narratives. Write for agents and experienced developers.

### TIMELINE.md

- **Purpose:** Progress and cost log for reporting to a boss, colleague, or stakeholder. Shows how the project is being made, when things happened, and (if available) what each step cost.
- **Contents:**
  - Chronological log of significant events (one entry per line or block).
  - Each entry: **date/time (timestamp)**, **what happened** (e.g. "Phase 1 started", "Task X completed", "Decision: use technology Y"), and optionally **cost** (e.g. token usage, API cost, or "N/A" if the environment does not provide it).
  - Summary or section headers by phase/milestone if the log gets long.
- **Update when:** A task is completed, a phase starts or ends, a significant decision is made, or at the end of a work session. Add an entry as soon as the event occurs; do not batch arbitrarily.
- **Detail level:** One line per small step is fine; group minor updates into a single entry if preferred. Enough for someone external to see progress and cost at a glance.
- **Cost:** If the environment exposes token usage, API cost, or similar, record it per step. Otherwise leave cost blank or "N/A". The user or a separate process may fill cost in later.
- **Avoid:** Duplicating full task descriptions from PLAN.md. Reference tasks by name or ID; keep TIMELINE.md a log, not a second plan.
- **Suggested entry format:** `YYYY-MM-DD HH:MM | What happened | Cost (or N/A)` or a short table with columns: Date/Time, Event, Cost.

---

## 2. Project Lifecycle

### A. Starting a New Project

Execute the following in order:

1. **Read INSTRUCTIONS.md and all existing project files.** If any of the six required files exist, absorb them before creating or changing anything.
2. **Ask for more details.** Before creating or overwriting the required files, ask the user for more details. *New project:* scope, target users, constraints, priorities, context from a previous application or repo, must-haves vs. nice-to-haves. *Existing project:* what to work on next, what has changed, or what to focus on. The user has primed with a general idea; the agent must seek clarification. Do not create or overwrite the required files until the user has provided sufficient context or explicitly asked to proceed with defaults (e.g. "just use sensible defaults").
3. **Create missing files.** For each absent file, generate it using the context gathered in step 2 (and any existing code or repo structure).
4. **If critical context is still missing after step 2:** Ask concisely. For non-critical gaps, use a sensible default and mark it with `<!-- TODO: Confirm -->`.
5. **Bootstrap order (create in this order):**
   - PRD.md first (defines *what*).
   - ARCHITECTURE.md second (defines *how* it's structured).
   - PLAN.md third (defines *what to do next*).
   - DOCUMENTATION.md fourth (defines *how to work in the repo*).
   - TIMELINE.md fifth (first entry: project started at [timestamp]; optional cost if available).
   - AGENTS.md last (defines *who does what*).
6. **Validate coherence.** After creation, confirm that PLAN.md tasks align with PRD.md requirements and that ARCHITECTURE.md supports the planned features. Optionally add a TIMELINE.md entry for "Bootstrap completed".

### B. During Development and Maintenance

1. **Update incrementally.** When completing a task, update only the affected files and only the affected sections. Do not rewrite entire documents.
2. **Keep PLAN.md current.** Mark completed tasks. Advance the current phase marker when appropriate. Add new tasks only if they stem from a real change, not speculation.
3. **Update TIMELINE.md.** When a task is completed, a phase starts or ends, or a significant decision is made, add an entry with timestamp and (if available) cost for that step. This keeps the log useful for reporting to a boss or colleague.
4. **Sync ARCHITECTURE.md with reality.** If you add a new module, service, or integration, reflect it in ARCHITECTURE.md in the same work session. Do not defer this.
5. **Extend DOCUMENTATION.md on meaningful change.** New setup steps, new environment variables, new conventions — document them when they're introduced, not later.
6. **Touch PRD.md only on confirmed scope changes.** Do not edit PRD.md to match implementation drift. If requirements and implementation diverge, flag it and ask.
7. **Never silently change AGENTS.md.** Role or access changes require explicit confirmation.

---

## 3. Agent Behavior Principles

These rules apply at all times, across all tasks.

**Minimal changes.** Modify only what is necessary. Do not refactor, reorganize, or "improve" code or docs unless explicitly asked or required by the current task.

**Follow existing patterns.** Before writing new code or documentation, study what already exists. Match the style, structure, and conventions already in use.

**Ask before major decisions.** Architectural changes, new dependencies, scope changes, or technology swaps require user confirmation. Do not proceed on assumption.

**No speculative documentation.** Do not document features that don't exist yet, unless they're in the active phase of PLAN.md. Remove or flag documentation for features that have been cut.

**Documentation is source of truth.** If documentation contradicts the codebase, investigate which is correct. Do not silently align one to the other — flag the discrepancy.

**Optimize for both human and AI readability.** Use clear headings, consistent formatting, and precise language. Avoid jargon that only makes sense in one context. Write so that a different agent — or a human six months from now — can act on the information without guessing.

**Preserve decision history.** When a significant decision is made (technology choice, architectural pattern, scope cut), record the *what* and *why* in the relevant file. Future agents need context, not just outcomes.

**Fail visibly.** If you're uncertain, blocked, or operating outside your defined role, say so explicitly. Silent failure or guesswork causes compounding errors.

**Start by asking.** At project start, always ask for more details before creating or overwriting the required files. The user primes with the general idea; the agent refines it through questions.

---

## 4. Quick Reference — When to Update What

| Event | Files to Update |
|---|---|
| New feature added to scope | PRD.md, PLAN.md |
| Task completed | PLAN.md, TIMELINE.md |
| Phase started or ended | PLAN.md, TIMELINE.md |
| Significant decision made | ARCHITECTURE.md or PLAN.md (rationale), TIMELINE.md (log) |
| New module or service created | ARCHITECTURE.md, DOCUMENTATION.md, TIMELINE.md (optional entry) |
| Dependency added or removed | DOCUMENTATION.md, ARCHITECTURE.md |
| Setup process changed | DOCUMENTATION.md |
| New agent role introduced | AGENTS.md |
| Bug fix (no structural change) | PLAN.md (mark task done), TIMELINE.md (optional) |
| Technology or pattern decision made | ARCHITECTURE.md (record rationale), TIMELINE.md |
| Scope reduced or feature cut | PRD.md, PLAN.md |
