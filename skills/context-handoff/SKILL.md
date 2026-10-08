---
name: context-handoff
description: Prepare a long session for context compaction. Use when the context window is around 70% full, auto-compact is coming, the user asks for 交接文档, 任务交接, 上下文压缩, 会话记忆, HANDOFF.md, compact handoff, or session memory management. Inventory and classify workspace scripts, files, and docs, tidy session artifacts, then write a durable handoff so work survives the lossy summary. Also use after compaction to read the handoff back before continuing. Do not use for ordinary commits, generic cleanup unrelated to context pressure, or rewriting a healthy product repo layout.
---

# Context Handoff

Context compaction is lossy. Before it runs, persist enough workspace structure and a handoff document that a fresh context can resume from disk alone.

This session's Grok CLI auto-compacts at **75%**. Trigger this skill at **~70%** so the handoff exists before that cut.

## Occupancy

You usually **cannot** see a live context percentage in the model prompt.

Do this instead:

1. If the user pasted `/context` or `/session-info` output, trust those numbers.
2. If they did not, ask them to run `/context` (or `/status`) once. Do not guess a fake percentage.
3. If they already asked to 交接 / 压缩前整理 / write HANDOFF, run the workflow even without a number.
4. Treat a long session with large tool dumps, many files read, or repeated failed loops as approaching the limit, and offer this workflow.

Never invent "we are at 70%". Say whether occupancy is measured, reported by the user, or inferred.

## Workflow

Do the steps in order. Stop after the handoff is on disk and summarized to the user.

### 1. Scope the workspace

Work in the current project workspace only.

- Classify **product files** (the real repo) vs **session artifacts** (notes, dumps, one-off scripts, diagnostic markdown created this session).
- Do **not** restructure a healthy git repo, move `src/`, rename packages, or "organize" third-party trees.
- Do tidy session artifacts that are scattered in the workspace root or a junk folder.

### 2. Inventory and classify

Scan the workspace (and obvious session folders) and group every relevant path into:

| Category | What belongs |
| --- | --- |
| `source` | Product code the task is changing |
| `scripts` | Helper scripts, one-shot tools, repair/import jobs |
| `docs` | Enduring documentation |
| `notes` | Session notes, diagnoses, postmortems |
| `config` | Config, env examples, agent rules |
| `data` | Logs, dumps, fixtures, exports |
| `tmp` | Disposable scratch |
| `handoff` | `HANDOFF.md` and related memory files |

Record owner, purpose, and whether the file is durable or disposable. Prefer evidence from the files themselves over chat memory.

### 3. Tidy session artifacts

Only after the inventory:

- Put durable session notes under `notes/` (create it if needed).
- Put one-off scripts under `scripts/` if they are not already in a package.
- Leave product code where the repo already expects it.
- Delete nothing unless the user asked and the file is clearly disposable scratch.
- Do not commit unless the user asked.

If a move would surprise git (tracked files jumping directories), stop and list the proposed moves instead of doing them.

### 4. Write `HANDOFF.md`

Write or overwrite `HANDOFF.md` at the workspace root.

If the workspace is a git repo the user does not want polluted, write `.grok/HANDOFF.md` instead and say so. Default is `HANDOFF.md` at the root.

The document must be complete enough that a new session can continue **without** this chat. Follow [references/handoff-template.md](references/handoff-template.md).

Required sections:

1. Objective and success criteria
2. Current status (done / in progress / blocked)
3. Workspace map (classified paths)
4. Concrete findings (exact values, error strings, commands that worked)
5. Decisions and why
6. Exact next step
7. Verification (what to run)
8. Open risks

Overwrite the previous handoff. Keep one rolling document, not a stack of dated copies, unless the user asked for history.

### 5. Confirm before compact

Tell the user:

- occupancy source (measured / user-reported / inferred)
- where the handoff lives
- what was classified or moved
- that they can `/compact` now, or wait for auto-compact at 75%

Do not run `/compact` yourself unless they asked.

### 6. After compaction

If this skill runs after a compact, or the user says the conversation was just compacted:

1. Read the handoff file first.
2. Resume from **Exact next step**. Do not restart the task from scratch.
3. Update the handoff when the next milestone lands.

## Decision rule

| Situation | Action |
| --- | --- |
| User asked for 交接 / HANDOFF / 压缩前整理 | Run the full workflow |
| `/context` shows ~70% or higher | Run the full workflow |
| Auto-compact is about to fire (threshold 75% here) | Run the full workflow immediately |
| Occupancy unknown, session still short | Do not fire this skill |
| After compact, handoff exists | Read it back, then continue |

## Safety

- Do not invent findings, file purposes, or occupancy percentages.
- Do not dump the whole chat into `HANDOFF.md`. Persist decisions, artifacts, and the next action.
- Do not put secrets, tokens, or `.env` values into the handoff.
- Do not reorganize unrelated projects in the parent directory.
