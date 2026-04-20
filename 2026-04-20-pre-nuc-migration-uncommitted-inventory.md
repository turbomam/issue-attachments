# Pre-NUC-migration uncommitted file inventory

**Date:** 2026-04-20
**Source machine:** LBL MacBook Pro (MAM-M74)
**Target machine:** Ubuntu NUC (192.168.0.204)
**Scope:** files modified in the past 7 days that are *not* tracked in any git repo, and that would be lost if not transferred before switching machines.

Note: the files themselves are **not** committed here — this is just the list.

---

## LLM / Claude Code configuration and authored content

### `~/.claude/` user-level (uncommitted)
- `~/.claude/CLAUDE.md` — Apr 17
- `~/.claude/settings.local.json` — Apr 16
- `~/.claude/shared/kg-microbe-agent-coordination.md` — recent
- `~/.claude/skills/sync-tidy-repo/SKILL.md` — recent

### All `~/.claude/skills/` (only 3 total, full inventory)
- `gog-search/` — Apr 1
- `gog-slack/` — Apr 1
- `sync-tidy-repo/` — modified within past week

No `~/.claude/agents/` directory exists. `~/.codex/skills/` contains only checked-out plugin marketplace content (not user-authored).

### Memory files, `~/.claude/projects/-Users-MAM/memory/` (modified in past 7 days)
- `MEMORY.md`
- `feedback_double_check_every_claim.md`
- `feedback_no_institutional_hosts_for_personal_tests.md`
- `feedback_agent_tier_default.md`
- `feedback_no_unauthorized_remote_actions.md`
- `project_wan_ssh_issue.md`
- `feedback_no_fabricated_examples.md`

---

## Home directory root (`~/`) — recent files

- `~/synchronized.kdbx` — **KeePass DB, important**
- `~/isa_closure_query,json` — odd filename with comma; may be stray
- `~/Claude.dmg` — installer, probably skip
- `~/.claude.json`
- `~/.gitconfig`
- `~/.zsh_history`
- `~/.zcompdump`

---

## Desktop / Documents / Downloads

- `~/Desktop/` — only `screenshots/` subfolder and `.DS_Store`; no user files at top level
- `~/Documents/` — only `gitrepos/` and `.DS_Store`; no loose files
- `~/Downloads/` — empty aside from `.DS_Store`

---

## Open questions before migration

1. Initial recollection was that "we wrote a lot of skills" — only 3 skills exist in `~/.claude/skills/`. Possibly confused with memory files, sub-skill reference files inside each skill, or skills authored inside a specific project repo.
2. `~/isa_closure_query,json` — keep or delete?
3. `~/synchronized.kdbx` — confirm current sync method to NUC.
4. Uncommitted/unpushed state across the ~90 repos in `~/Documents/gitrepos/` was **not** checked in this pass; worth a sweep before migration.
