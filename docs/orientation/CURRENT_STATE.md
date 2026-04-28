---
type: current-state
title: Current State
domain: orientation
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

# Current State

Fast truth page for what exists now and where to look next. Keep this file short. It should fan out to durable docs instead of becoming a session journal.

## What Exists Today

This fork starts from OpenAI's public `openai/symphony` repository. The upstream project provides a language-agnostic Symphony service specification plus an Elixir reference implementation that polls Linear, creates per-issue workspaces, launches Codex in app-server mode, and exposes a basic observability dashboard/API.

Built and usable:

- Upstream Symphony spec in `SPEC.md`.
- Elixir reference implementation in `elixir/`.
- Existing dashboard/API for active runtime observability.
- Local AGENT-DOCS scaffold with docs-meta installed.
- Captured product idea for a richer Symphony Console in `docs/product/ideas/IDEA-0001-symphony-console.md`.

## Not Yet Built

- Full Codex chat/session inventory across `~/.codex`.
- Drilldown into historical and active chat transcripts.
- Operator controls for continue, stop, compact, or queued follow-up prompts.
- Auth, permission, and audit model for any UI that can mutate live agent sessions.

## Current Product / System Model

Use this mental model:

- Upstream Symphony is the orchestration daemon and issue-tracker runner.
- The Elixir implementation is the current executable surface.
- The dashboard is currently observability-first, not a full command center.
- Symphony Console is a proposed extension layer over Symphony runtime state plus Codex session history/control APIs.

Source docs:

- Architecture: `docs/orientation/ARCHITECTURE.md`
- Product/spec baseline: `SPEC.md`
- Console idea: `docs/product/ideas/IDEA-0001-symphony-console.md`

## Roadmap Position

The fork is at documentation/bootstrap stage. No implementation changes have been made yet beyond installing AGENT-DOCS and capturing the first product idea.

Source docs:

- Roadmap: `docs/orientation/ROADMAP.md`
- Plans: `docs/product/plans/`
- Upstream spec: `SPEC.md`

## Key Gotchas

- `elixir/AGENTS.md` owns implementation validation; use `make all` before handoff for Elixir changes.
- The current Symphony service is a trusted-environment preview; control-plane features must treat session mutation as high impact.
- Codex session history and active app-server control are not the same surface; design should verify protocol support before promising actions like compact.

## Verification Baseline

Recent meaningful verification has included:

- `agent-docs-init --profile growing --write /Users/macintoso/Documents/VSCode/symphony-console`

For current commands, use:

- `elixir/AGENTS.md`
- `tests/docs-meta-smoke.sh`

## Where To Look

| Need | Read |
|---|---|
| First orientation | `README.md` |
| Current architecture | `docs/orientation/ARCHITECTURE.md` |
| Roadmap order | `docs/orientation/ROADMAP.md` |
| Product/system model | `SPEC.md` |
| Console concept | `docs/product/ideas/IDEA-0001-symphony-console.md` |
| Durable decisions | `docs/decisions/adr/` |
| Session paper trail | `docs/repo-health/session-logs/` |

## Maintenance Rule

When a plan or multi-task change finishes, update this file only with:

- new current truth
- changed roadmap position
- new gotchas
- links to deeper source docs
