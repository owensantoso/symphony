---
type: roadmap
title: Roadmap
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

# Roadmap

Ordered roadmap for what has shipped, what is being shaped now, and why work sits where it does.

## Status Legend

- `Complete`
- `Captured`
- `Drafted`
- `Not started`
- `Backlog`

## Roadmap Snapshot

| Band | Status | Why it sits here |
|---|---|---|
| Foundation | Complete | Fork is cloned and docs scaffold is installed. |
| Mainline | Captured | Symphony Console is captured as `IDEA-0001`; no spec or plan yet. |
| Expansion | Backlog | Mutating controls need protocol research and safety design first. |
| Later | Backlog | Multi-repo, remote-worker, and hosted control-plane work are out of scope until local console shape is proven. |

## 1. Completed Foundation

| Milestone | Status | Why it matters |
|---|---|---|
| Fork and docs scaffold | Complete | Establishes a safe place to design and implement Symphony Console. |

## 2. Current Mainline

The key principle is:

> Make runtime and history visible before adding controls that mutate live agent sessions.

| Order | Milestone | Status | Why it sits here |
|---|---|---|---|
| 1 | `IDEA-0001-symphony-console` | Captured | Preserves the product direction without pretending the protocol and safety design are settled. |
| 2 | Console research/spec | Not started | Needs a focused pass on Codex app-server actions, local session-history structure, and safe controls. |
| 3 | Read-only console prototype | Not started | Lowest-risk implementation slice: active runs, historical sessions, per-session drilldown. |
| 4 | Controlled action prototype | Not started | Continue/stop/queue/compact should come only after action semantics are verified. |

## 3. Side Branches And Polish Tracks

| Milestone | Status | Why it is not ahead of the mainline |
|---|---|---|
| Dashboard polish | Backlog | Useful, but secondary to clarifying the read/control model. |
| Multi-worker control | Backlog | Depends on the local console and trust model. |

## 4. Post-Mainline Expansion

| Work | Status | Why it belongs later |
|---|---|---|
| Hosted/team control plane | Backlog | Requires auth, tenancy, audit, and secrets boundaries beyond this local fork's first slice. |
| Cross-agent review and scheduling intelligence | Backlog | Needs reliable session inventory and action primitives first. |

## 5. How To Use This Roadmap

- Start with `CURRENT_STATE.md` for what already exists.
- Use this roadmap for ordering and rationale.
- Use `docs/product/ideas/IDEA-0001-symphony-console.md` for the current product spark.
- Promote the idea to a `SPEC-*` before implementation.
- Add a `PLAN-*` only after the first spec or research note resolves the protocol and safety questions.
