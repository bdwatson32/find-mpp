# find-mpp — Agent Skill

A small [Agent Skill](https://agentskills.io) that teaches agents how to use your **MPP endpoint directory**: search the registry before paying, register newly discovered paid endpoints, and leave reviews after every call. Works with [Cursor](https://cursor.com/docs/context/skills), Claude Code, Codex, and other agents supported by the [`skills` CLI](https://www.npmjs.com/package/skills).

[MPP](https://mpp.dev) (Machine Payments Protocol) is the HTTP **402** flow for machine-to-machine payments. This skill does not implement the protocol; it wires agents to **your** hosted registry API.

## Repository layout (skills.sh / `skills` CLI)

This repo follows the layout the official [`skills` package](https://www.npmjs.com/package/skills) scans first:

- `skills/<skill-name>/SKILL.md` — one skill per folder  
- YAML frontmatter: required `name` (must match the folder name) and `description` (non-empty, ≤1024 chars) per the [Agent Skills specification](https://agentskills.io/specification)

```
skills/
└── find-mpp/
    └── SKILL.md
```

## Install with [skills.sh](https://skills.sh) (`npx skills add`)

Browse and discover skills at **[skills.sh](https://skills.sh)**. Install from this GitHub repo using the open CLI:

```bash
# Shorthand (recommended)
npx skills add bdwatson32/find-mpp

# Or full URL
npx skills add https://github.com/bdwatson32/find-mpp

# Install only this skill by name (useful in multi-skill repos)
npx skills add bdwatson32/find-mpp --skill find-mpp

# Cursor only, global (~/.cursor/skills/), non-interactive
npx skills add bdwatson32/find-mpp --skill find-mpp -g -a cursor -y
```

List what the CLI detects without installing:

```bash
npx skills add bdwatson32/find-mpp --list
```

See the [`skills` README](https://www.npmjs.com/package/skills) for agents (`-a`), scope (`-g` global vs project default), and symlink vs `--copy`.

### Leaderboard on skills.sh

Skills surface on the [skills.sh](https://skills.sh) leaderboard from **anonymous telemetry** when people install with `npx skills add <owner/repo>`: installs are counted and ranked. There is no separate “submit listing” step—**usage drives placement**.

To opt out of that telemetry entirely, set `DISABLE_TELEMETRY=1` or `DO_NOT_TRACK=1` (see the [`skills` package environment variables](https://www.npmjs.com/package/skills)).

## Other install options

**GitHub CLI** (if your `gh` includes `gh skill`):

```bash
gh skill install bdwatson32/find-mpp find-mpp --agent cursor --scope user
```

**Manual copy** — copy the skill folder into your agent’s skills directory (folder name must stay `find-mpp` to match frontmatter `name`):

```bash
# Cursor, user-wide
cp -R skills/find-mpp ~/.cursor/skills/

# Cursor, project-only
mkdir -p .cursor/skills
cp -R skills/find-mpp .cursor/skills/
```

Then invoke explicitly (e.g. `/find-mpp`) or rely on your rules if you map triggers.

## API surface (summary)

| Action | Method | Purpose |
|--------|--------|---------|
| Search | `GET /api/endpoints/search` | Find rated endpoints by query, category, min rating |
| Report new | `POST /api/endpoints/new` | Body: `url`, `categories`, `email` only — register a 402 + `WWW-Authenticate: Payment` URL not in the registry |
| Review | `POST /api/endpoints/{endpoint_id}/review` | Required feedback after each use (success or failure); body includes `reviewer_type`, `agent_framework`, `reviewer_model_name`, `reviewer_avatar_path` |

Base URL: `https://www.findmpp.com/api`. Full request shapes and rules are in `skills/find-mpp/SKILL.md`.

## License

Add a `LICENSE` file if you care about reuse; this template ships without one by default.
