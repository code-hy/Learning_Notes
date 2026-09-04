# AI-Native Development Cheat Sheet

## 1. Spec-Driven Development
* **Core Concept:** Refine vague ideas into clear, explicit specifications before generating code to prevent agents from making unintended assumptions.
* **Workflow:**
  1. **Brainstorming:** Use a chat assistant (e.g., via dictation/voice) asking targeted, one-by-one questions to establish project scope, audience, features, and constraints.
  2. **Plan Creation:** Export the finalized vision to a single Markdown file (e.g., `_docs/plan.md`).
  3. **Stack Selection:** Ask the coding agent to propose technology stacks based on `plan.md` and select the one best suited to your review capabilities.
  4. **Backlog Generation:** Prompt the agent to break the spec into modular tasks (`_docs/tasks.md`) with a strict goal/description template, then sync them as GitHub Issues (the single source of truth).

---

## 2. Context Engineering
* **Core Concept:** Standardize project knowledge across all agent sessions using dedicated Markdown documents.
* **Key Files:**
  * `AGENTS.md` / `CLAUDE.md`: Central entry point containing initial test/build commands and top-level execution rules.
  * `_docs/process.md`: Defines operational rules, role responsibilities, and issue workflows.
  * **Domain Docs:** Specialized instructions like `testing-guidelines.md` or `design-system.md` loaded dynamically by the agent only when pertinent to the task.
* **Continuous Feedback:** Prompt agents to update these context documents whenever corrections are made during a session to prevent recurring mistakes.

---

## 3. Loop Engineering
* **Core Concept:** Automate multi-step tasks by running an agent iteratively inside a system harness (e.g., using `/goal` commands) until explicit, verifiable conditions are satisfied.
* **Key Components:**
  * **Goal Prompts:** Commands like `/goal groom all issues` that instruct the agent to continue executing without asking for manual confirmation between items.
  * **Checkable Stop Conditions:** Criteria that can be programmatically verified (e.g., "all unit tests pass" or "all issues groomed") rather than subjective outcomes (e.g., "make the code cleaner").

---

## 4. Graph Engineering & Multi-Agent Orchestration
* **Core Concept:** Assign specialized personas to distinct agents and route tasks between them through defined state transitions.
* **Agent Persona Roles:**
  * **Product Manager (`_docs/team/pm.md`):** Grooms issues into concrete goals, checkable acceptance criteria, constraints, and explicit out-of-scope items using `_docs/task-template.md`. Writes no code.
  * **Software Engineer (`_docs/team/software-engineer.md`):** Implements code exclusively against the acceptance criteria, writes unit tests, and commits progress without closing the issue.
  * **QA Engineer (`_docs/team/qa-engineer.md`):** Independently tests implementation against the criteria and outputs a strict verdict (`PASS` or `FAIL` with diagnostic logs). Writes no code fixes.
* **Orchestration Lifecycle:**
  ```text
  Pick Backlog Issue ➔ PM Grooms ➔ Engineer Implements ➔ QA Verifies
                                           ▲                   │
                                           └─── [ On FAIL ] ───┴─── [ On PASS ] ➔ Close Issue
