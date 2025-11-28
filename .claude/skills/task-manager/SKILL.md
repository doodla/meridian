---
name: task-manager
description: Create and manage development tasks after the user approves a plan. Initializes folders/files, updates the backlog, and keeps progress notes synchronized.
---
<task_manager>
# Task Manager Skill

## When to Use
Immediately after a plan is approved. The script scaffolds the task folder, locks the plan, and registers the work in the backlog. Skip this skill for speculative ideas or unapproved work.

---

## Workflow
Stay concise. Record only the facts another engineer needs; skip filler or repetition. Avoid verbosity.

### Create the task
```bash
python3 $CLAUDE_PROJECT_DIR/.claude/skills/task-manager/scripts/create-task.py
```
The helper creates `.meridian/tasks/TASK-###/` with three files:
- `TASK-###.yaml` — brief (objective, scope, acceptance, risks, links)
- `TASK-###-plan.md` — exact approved plan (changes need re-approval)
- `TASK-###-context.md` — running log of decisions, links, blockers

IDs are zero-padded (`TASK-001`). Read each file before editing (System limitation, you cannot edit the files before you read them).

### Populate files
- Complete the YAML template in `TASK-###.yaml`
- Paste the approved plan into `TASK-###-plan.md`
- Add an initial context entry with date, summary, and first steps in `TASK-###-context.md`

### Register in the backlog
Add an item to `.meridian/task-backlog.yaml`:
- `id`, `title`, `status: todo`, `priority`, `path: ".meridian/tasks/TASK-###/"`
Only edit `status` and `priority` later; never rename IDs or delete entries.

Allowed values:
- `status`: `todo | in_progress | blocked | done`
- `priority`: `P0 | P1 | P2 | P3`

---

## During Execution
- Switch backlog status to `in_progress` when coding starts; use `blocked` with a note in context if waiting.
- Update `TASK-###-context.md` with timestamped notes for decisions, tradeoffs, blockers, and “MEMORY:” candidates (then call `memory-curator`).
- Keep `TASK-###.yaml` accurate if scope, risks, or acceptance criteria shift (with approval).
- Use `memory-curator` for durable facts (architecture shifts, lessons learned, traps to avoid later). Never edit `.meridian/memory.jsonl` manually.

---

## Finishing
Mark `done` only when all conditions hold:
- Code builds, lint/tests pass, migrations applied.
- Docs updated (README, API refs, etc.) if behavior changed.
- Backlog entry set to `done`.
- Durable insights recorded via `memory-curator`.

---

## Plan or Scope Changes
- Re-seek approval for any material change.
- In `TASK-###-plan.md`, add `Amendment YYYY-MM-DD` with the new plan.
- Log rationale + links in `TASK-###-context.md`.

---

## Split / Merge / Cancel
- **Split**: create new TASK IDs, move sections, add “Superseded by …” note in originals.
- **Merge**: keep a primary task; mark others `done` with “Merged into …”.
- **Cancel**: mark `done` with `resolution: canceled`; context must explain why.
</task_manager>