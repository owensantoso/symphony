---
type: idea
id: IDEA-0001
title: Symphony Console
domain: product
status: captured
created_at: "2026-04-29 02:54:14 JST +0900"
updated_at: "2026-04-29 02:54:14 JST +0900"
owner: 
source:
  type: conversation
  link: 
  notes:
areas: []
related_specs: []
related_research: []
related_issues: []
related_prs: []
related_sessions: []
linked_paths:
  - docs/orientation/ARCHITECTURE.md
  - docs/orientation/CURRENT_STATE.md
  - docs/orientation/ROADMAP.md
promoted_to: []
repo_state:
  based_on_commit: 58cf97da06d556c019ccea20c67f4f77da124bf3
  last_reviewed_commit: 58cf97da06d556c019ccea20c67f4f77da124bf3
---

# IDEA-0001 - Symphony Console

## Raw Thought

Extend Symphony from a Linear-driven unattended agent runner into a local operator console for all relevant Codex work.

The console should let a user see:

- all active Symphony-managed issue runs
- which Codex sessions are running right now
- historical Codex chats/session threads
- per-chat metadata such as workspace, repo, issue, status, model, timestamps, token usage, and last activity
- the transcript or event stream for an individual chat/session
- queued or pending follow-up work

The console might eventually let a user act on a session:

- continue a chat with a follow-up prompt
- queue a prompt to run after the current turn
- stop or cancel an active turn/session
- compact a session when supported
- mark a session as archived, ignored, or attached to a ticket
- open the workspace, PR, Linear issue, or local transcript from one place

## Why It Might Matter

OpenAI's Symphony shifts work management from supervising individual Codex sessions to managing issue execution. That is useful, but the lived operator problem is still partly session-shaped: multiple chats run in the Codex UI, some are active, some are historical, some belong to Symphony issue workspaces, and some are unrelated exploratory sessions.

A richer console could make Symphony feel less like a background daemon and more like a command center:

- less confusion about what is currently running
- faster debugging when an agent stalls or loops
- better continuity across long-running or compacted chats
- safer operations because mutating controls are explicit, visible, and auditable
- easier handoff between manual Codex Desktop work and Symphony-managed runs

## Possible Shape

### Phase 1: Read-Only Session Inventory

Build a local read-only view that combines Symphony runtime state with Codex session metadata.

Candidate views:

- `Running`: active Symphony issue runs and active Codex app-server sessions.
- `History`: searchable Codex session/thread list from local Codex indexes.
- `Issue Detail`: Linear issue, workspace path, current session ID, recent events, retry state, token usage, and logs.
- `Chat Detail`: transcript/event timeline, commands, tool calls, model, workspace, and related issue/PR links.

Data sources to investigate:

- Symphony orchestrator snapshot/API (`/api/v1/state`, `/api/v1/:issue_identifier`)
- Symphony logs under configured logs root
- Codex local session index and JSONL session files under `~/.codex`
- Codex app-server notifications for live sessions

### Phase 2: Session Linking And Search

Add correlation between issue runs, workspaces, Codex sessions, and local chat history.

Useful joins:

- Symphony `issue_identifier` to workspace path
- Symphony `session_id` to Codex thread/turn IDs
- Codex `cwd` to workspace path or source repo
- branch/PR/Linear issue identifiers from transcripts and workpads

Search/filter ideas:

- running vs completed vs failed
- repo/workspace
- issue identifier
- status/state
- model
- last updated
- text search across chat summaries or transcript bodies

### Phase 3: Safe Operator Controls

Add mutating controls only after app-server semantics and safety constraints are verified.

Candidate controls:

- stop/cancel current turn
- request orchestrator refresh
- continue a session with a new prompt
- queue follow-up prompt for after the active turn
- retry a failed issue run
- detach or archive local session metadata
- compact a session if Codex exposes a supported action

Every control should have:

- permission/availability checks
- clear disabled states when unsupported
- confirmation for destructive or interrupting operations
- audit log entry with operator, timestamp, session, issue, action, and result
- failure handling that does not corrupt orchestrator state

### Phase 4: Workflow Integration

Make the console useful for real work loops:

- open Linear issue, GitHub PR, workspace, and transcript from one place
- attach notes or summaries to a session
- show stalled/looping sessions and suggested next actions
- show review-ready sessions and proof-of-work artifacts
- generate a handoff summary for a run

## Questions

- Which Codex app-server actions are available for stop, continue, and compact?
- Can an existing Codex Desktop chat be resumed through app-server, or are app-server sessions separate?
- What is the stable schema of `~/.codex/session_index.jsonl` and session JSONL files?
- How should the console handle privacy and secrets in transcripts and command output?
- Should Symphony own session-history indexing, or should a separate local service own it?
- How should the UI distinguish "Symphony-managed" sessions from ordinary Codex Desktop chats?
- Is compaction a protocol feature, a UI-only action, or unavailable to external clients?
- What is the safest default: read-only console first, then explicit opt-in controls?
- Should queued prompts live in Symphony state, Codex state, or an external queue?
- How should remote worker sessions be represented when their transcripts/logs are not local?

## Promotion Criteria

What would need to be true before this becomes a spec, research note, ADR, or plan?

- Confirm app-server protocol support for live actions such as cancel/continue/compact.
- Map local Codex session-history files well enough to build a read-only index.
- Decide whether the first implementation lives inside the existing Phoenix dashboard or as a separate console surface.
- Define the safety model for mutating controls.
- Write a focused `SPEC-*` for the read-only console slice before implementation.
- Write an ADR before adding any mutating control path.

## Reference Links

- Upstream Symphony repo: https://github.com/openai/symphony
- OpenAI Symphony announcement: https://openai.com/index/open-source-codex-orchestration-symphony/
- Upstream service spec: `../../../SPEC.md`
- Elixir implementation docs: `../../../elixir/README.md`
