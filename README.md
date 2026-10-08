# agent-skills

Personal Agent Skills for [StackFerry](https://github.com/Ninthless/StackFerry), Grok CLI, Claude Code, Codex, and other Agent Skills compatible tools.

StackFerry indexes this repository as `m4rr5er/agent-skills` and loads every directory under `skills/` that contains a `SKILL.md`.

## Layout

```text
skills/
  <skill-name>/
    SKILL.md          required
    references/       optional extra docs
    scripts/          optional helper scripts
```

Each `SKILL.md` starts with YAML frontmatter:

```markdown
---
name: my-skill
description: One or two sentences. Say what it does and when to use it.
---

# My Skill

Instructions the agent should follow.
```

`name` is lowercase letters, digits, and hyphens. `description` is what StackFerry and Grok show in catalogs and use for auto-trigger.

## Add this source in StackFerry

1. Open StackFerry → Skills.
2. Add a GitHub source: `m4rr5er/agent-skills`, branch `main`.
3. Install the skills you want onto Grok / Claude / Codex.

StackFerry downloads `https://github.com/m4rr5er/agent-skills/archive/refs/heads/main.zip`, so this repository stays public.

## Use it from Grok CLI without StackFerry

Clone once, then point Grok at the `skills/` directory:

```toml
# ~/.grok/config.toml
[skills]
paths = ["C:/projects/agent-skills/skills"]
```

Or copy a single skill into `~/.grok/skills/<name>/`.

## Add a skill

1. Create `skills/<name>/SKILL.md`.
2. Write a specific `description` with trigger phrases.
3. Commit and push to `main`.
4. Refresh the source in StackFerry.

Keep secrets, tokens, and machine-specific paths out of this repository.

## Skills

| Skill | Purpose |
| --- | --- |
| [`context-handoff`](./skills/context-handoff/) | At ~70% context, classify workspace scripts/files/docs and write `HANDOFF.md` before auto-compact. |
