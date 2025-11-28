# Changelog

## [0.0.2] - 2025-11-20

### Added
- `startup-prune-completed-tasks.py` hook, triggered on startup/clear, that automatically archives older `done/completed` tasks. Only the 10 most recent completed tasks stay in `task-backlog.yaml`, and pruned tasks move to `task-backlog-archive.yaml` plus `.meridian/tasks/archive/`.
- `edit_memory_entry.py` and `delete_memory_entry.py` scripts for the memory-curator skill, enabling safe updates/removals in `.meridian/memory.jsonl`. `SKILL.md` now documents how to run both scripts.
- `permission-auto-approver.py`, a PermissionRequest hook that silently whitelists known-safe Meridian actions (memory/task skills, backlog edits, etc.). `settings.json` now invokes this hook for every PermissionRequest event.

### Changed
- Task-manager and memory-curator SKILL guides were heavily trimmed to remove noisy guidance. Both are now shorter, strictly action-focused, and avoid repeating information that bloated the context.
- `relevant-docs.md` now describes section-based doc requirements (“when working on X, read Y”) instead of forcing Claude to read every listed file on startup. Hooks will prompt Claude to read only the docs tied to the area it’s actively working on.

