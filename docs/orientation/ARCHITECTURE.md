---
type: architecture-overview
title: Architecture
domain: architecture
status: draft
created_at: "2026-04-29 02:54:20 JST +0900"
updated_at: "2026-04-29 02:54:20 JST +0900"
owner:
areas: []
related_specs: []
related_plans: []
related_adrs: []
related_sessions: []
repo_state:
  based_on_commit: 58cf97da06d556c019ccea20c67f4f77da124bf3
  last_reviewed_commit: 58cf97da06d556c019ccea20c67f4f77da124bf3
---

# Architecture

System overview for this Symphony fork. This document describes the current upstream architecture and the intended forward direction for Symphony Console. For what is actually built today, see `docs/orientation/CURRENT_STATE.md`.

## Product

Symphony is an orchestration service for coding-agent work. It watches a tracker such as Linear, creates isolated workspaces for eligible issues, launches Codex in app-server mode, and keeps the agent working until the workflow reaches a handoff or terminal state.

Symphony Console is the proposed next layer: an operator-facing control room for active Symphony runs and broader Codex chat/session history. It should make active agents visible, historical conversations inspectable, and carefully scoped controls available where the Codex app-server protocol supports them.

## Reality Check

- Today this fork contains upstream Symphony plus local docs scaffold.
- Current upstream dashboard/API is observability-first.
- Console work should extend observability before adding mutating controls.
- If this document and `CURRENT_STATE.md` disagree, treat `CURRENT_STATE.md` as truth for what exists now.

## Decision Provenance

| Area | Decision source |
|---|---|
| Current shipped state | `docs/orientation/CURRENT_STATE.md` |
| Roadmap order and rationale | `docs/orientation/ROADMAP.md` |
| Upstream service baseline | `SPEC.md` |
| Console idea | `docs/product/ideas/IDEA-0001-symphony-console.md` |
| Durable architecture decisions | `docs/decisions/adr/` |
| Session-to-commit traceability | `docs/repo-health/session-logs/` |

## Architecture Areas

No split area docs yet. Add `AREA-*` docs only when implementation work needs stable subsystem ownership.

## Tech Stack

| Layer | Tech |
|---|---|
| Orchestrator | Elixir/OTP reference implementation |
| Tracker integration | Linear GraphQL |
| Agent runtime | Codex app-server over stdio |
| Dashboard/API | Phoenix LiveView + JSON endpoints |
| Local docs | AGENT-DOCS growing profile + `scripts/docs-meta` |

## System Boundaries

- Symphony owns dispatch, runtime state, workspace lifecycle, retries, and observability.
- Codex owns the actual agent conversation, tool execution, and code changes inside each workspace.
- Linear owns issue state and work intake.
- A future Console must distinguish read-only session inspection from mutating session control.

## Canonical Model Or Core System Seams

- `Issue` is the tracker-normalized work unit.
- `Workspace` is the per-issue filesystem execution boundary.
- `Run Attempt` is the execution lifecycle for one issue and workspace.
- `Codex Session` is the chat/thread/turn state exposed by app-server and local Codex history.
- `Console Action` should become an explicit audited operation before any mutating UI is added.

## Working Rules

- Never run Codex turns in the source repo; run them in per-issue workspaces.
- Keep upstream `SPEC.md` and implementation docs aligned when changing behavior.
- Treat stop, continue, compact, and queued prompts as high-impact controls that require protocol verification and audit logging.

## Critical Flows

### Existing Run Path

1. Symphony polls Linear for eligible issues.
2. The orchestrator creates or reuses a workspace.
3. The agent runner starts a Codex app-server session and sends the rendered workflow prompt.
4. Runtime updates feed logs, dashboard state, and API payloads.

### Proposed Console Read Path

1. Load active Symphony runtime snapshot from orchestrator state/API.
2. Load historical Codex session metadata from local Codex indexes.
3. Link active Symphony issue/session IDs to stored Codex transcripts where possible.
4. Render list, detail, timeline, and transcript views.

### Proposed Console Control Path

1. Operator selects an active or resumable session.
2. Console validates that the action is supported and allowed.
3. Console records an audit event.
4. Console invokes the app-server or orchestrator command.
5. Console updates runtime state and records success/failure.

## Replaceability Targets

These layers should change the most if implementation plumbing is swapped later:

- Tracker adapter.
- Console UI surface.
- Session-history indexer.
- App-server control adapter.

These layers should change the least:

- Orchestrator state model.
- Workspace safety invariants.
- Audit model for mutating controls.

## Module Layout

```text
README.md
SPEC.md
elixir/
docs/
scripts/
tests/
```
