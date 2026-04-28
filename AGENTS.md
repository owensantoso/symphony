# Symphony Console - Agent Index

Short index. Read the file for your task.

## Start here

1. `docs/orientation/CURRENT_STATE.md` - what is built today. Read first, always.
2. `README.md` - upstream Symphony project overview.
3. `SPEC.md` - upstream language-agnostic Symphony service specification.
4. `elixir/README.md` - current Elixir implementation and run instructions.
5. `docs/orientation/ARCHITECTURE.md` - local fork architecture overview and console direction.
6. `docs/orientation/ROADMAP.md` - local fork roadmap and rationale.
7. `docs/product/plans/` - plan guide and examples for new plans and briefs.
8. `docs/product/ideas/IDEA-0001-symphony-console.md` - captured Symphony Console product idea.
9. `docs/repo-health/session-logs/README.md` - timestamped session receipts.
10. `elixir/AGENTS.md` - Elixir surface rules and validation commands.

## By task

| Task | Read |
|---|---|
| Any code change | `docs/orientation/CURRENT_STATE.md` + relevant surface `AGENTS.md` |
| Elixir implementation change | `docs/orientation/CURRENT_STATE.md` + `SPEC.md` + `elixir/AGENTS.md` |
| Symphony Console idea/spec work | `docs/product/ideas/IDEA-0001-symphony-console.md` + `docs/orientation/ARCHITECTURE.md` |
| Implementing a plan | relevant `<domain>/plans/README.md` + parent `PLAN-*` doc + relevant `IMPL-*` brief(s) |
| Closing a meaningful work session | `docs/repo-health/session-logs/README.md` |
| Resuming or delegating todo-backed work | `scripts/docs-meta todos` + parent plan/brief |
| Surface codebase orientation | `docs/orientation/CURRENT_STATE.md` + `elixir/AGENTS.md` |
| Model / orchestration seam question | `docs/orientation/CURRENT_STATE.md` + `docs/orientation/ARCHITECTURE.md` + `SPEC.md` |
| Verification / test command question | `elixir/AGENTS.md` |
| Future feature request | `docs/IDEAS.md` + relevant `docs/<domain>/ideas/IDEA-####-<slug>.md` |
| Concept / ontology / naming model | `docs/CONCEPTS.md` + relevant `docs/<domain>/concepts/CONC-####-<slug>.md` |
| Research / evaluation / diagnostic question | `docs/README.md` "Doc Type Workflow" + `docs/research/README.md`, `docs/repo-health/evaluations/README.md`, or `docs/repo-health/debugging/README.md` |
| Repeated confusion or corrected understanding | `docs/LEARNINGS.md` + relevant `docs/decisions/learnings/LRN-####-<slug>.md` |
| Human-facing concept explanation | `docs/EXPLAINERS.md` + relevant `docs/orientation/explainers/EXPL-####-<slug>.md` |
| Durable open question | `docs/QUESTIONS.md` + relevant `docs/decisions/questions/QST-####-<slug>.md` |

If the repo includes `skills/structured-docs-workflow/SKILL.md`, read it before implementation when the repo uses plans, briefs, structured `TODO-*`, or `docs-meta`.

## Rules

- Never invent scope. If it is not in a spec, plan, or captured idea, ask.
- Update `docs/orientation/CURRENT_STATE.md` at the end of any plan or multi-task change.
- Keep `docs/orientation/CURRENT_STATE.md` short; move detailed history to state snapshots.
- Add a timestamped session log for meaningful implementation, planning, or debugging sessions.
- Use commit trailers for meaningful commits: `Plan:`, `Brief:`, `Spec:`, `ADR:`, `Todo:`, `Session:`, `Area:`, and `Verification:` as applicable.
- Capture early, fuzzy, or future-facing thoughts as `IDEA-*` docs instead of bloating specs or plans.
- Capture semi-mature domain models, taxonomy, ontology, naming, or source-of-truth sketches as `CONC-*` docs before promoting them into specs, ADRs, plans, architecture docs, or explainers.
- Capture lessons learned as `LRN-*` records instead of leaving them trapped in chat.
- Capture reusable human-facing explanations as `EXPL-*` docs. Use visualization-pass-style diagrams when structure, flow, state, ownership, or behavior is clearer visually.
- Capture durable unresolved questions as `QST-*` docs only when they need status, ownership, links, or resolution history across sessions. Ask ordinary clarification questions in chat.
- If the repo uses `docs/architecture/areas/AREA-*.md`, use those exact `AREA-*` IDs in plan frontmatter, session logs, PRs, issues, and `Area:` commit trailers.
- Planning doc hierarchy matters.
  - Parent `PLAN-*` docs define scope, architecture, invariants, and non-goals.
  - `IMPL-*` briefs define task-level execution details for one bounded task or grouped task.
  - Parent plans win on intent and boundaries.
  - Implementation briefs win on execution order and verification details.
- Do not implement from an implementation brief alone without first reading its parent plan.
- Parent plans and implementation briefs should explicitly call out dependencies and safe parallelization.
- Use structured `TODO-*` items only for durable coordination across sessions, agents, skills, or reviews; keep tiny checklist items as local checkboxes.
- `TODO-*` IDs coordinate progress, but parent plans and implementation briefs still define scope and verification.
- When claiming a `TODO-*`, add `owner:`, `agent:`, and `updated:` on the source checkbox line so future agents can find the exact runner/session in chat logs.
- Cite todo-backed work in session logs, commit trailers, PRs, and issues with `Todo: TODO-####` or equivalent references.
- Keep source-code TODO comments local; promote them to structured Markdown `TODO-*` items when they need ownership, delegation, review, or cross-session tracking.
- When completing durable todo-backed work, update the source checkbox line and run the repo's todo checks before closeout.
- When the human asks for an explanation, answer in chat first. Create or update a `CONC-*`, `EXPL-*`, `LRN-*`, or `QST-*` only when it will reduce future confusion.
- Do not write that the human understands something unless they confirmed it. Use agent-inferred wording when unsure.
- When feedback reveals a missed rule, wrong assumption, ambiguous docs, or workflow failure, use `reflect-and-improve` and update the smallest durable source that would prevent a repeat.

## Behavioral guidelines

### Think before coding

- State assumptions explicitly.
- If multiple interpretations exist, surface them instead of choosing silently.
- Prefer the simpler approach when it still satisfies the plan.

### Simplicity first

- Minimum code that solves the problem.
- No abstractions for single-use code unless the plan requires them.
- No speculative configurability.

### Surgical changes

- Touch only what you must.
- Match local style and structure unless the task is explicitly a refactor.
- Clean up orphans created by your own work.

### Goal-driven execution

- Turn vague tasks into verifiable outcomes.
- Prefer tasks that can be tested or otherwise checked explicitly.

### Reflect and improve

- Treat corrections, contradictions, frustration signals, and missed rules as chances to improve the shared system.
- Name what happened, classify the source, update the smallest useful doc/template/check/skill, and record a session-log note when the lesson should survive the chat.

### Planning workflow

- Use the parent plan to understand intent and boundaries.
- Use `impl-*` briefs for bounded execution and delegation.
- When drafting a parent plan, make dependencies and safe parallelization explicit.
- When drafting a brief, make ownership, write-conflict notes, and verification explicit.
- New durable docs should use YAML frontmatter with status, exact local timestamps, domain, and relevant links where useful.

## Repository Layout

```text
elixir/      Reference Symphony Elixir implementation
docs/        Local fork documentation and future Symphony Console planning
scripts/     docs-meta helper
tests/       docs-meta smoke test
SPEC.md      Upstream language-agnostic service specification
README.md    Upstream project overview
```
