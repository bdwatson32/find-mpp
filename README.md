# MPP Directory — Cursor Agent Skill

A small [Cursor Agent Skill](https://cursor.com/docs/context/skills) that teaches agents how to use your **MPP endpoint directory**: search the registry before paying, register newly discovered paid endpoints, and leave reviews after every call.

[MPP](https://mpp.dev) (Machine Payments Protocol) is the HTTP **402** flow for machine-to-machine payments. This skill does not implement the protocol; it wires agents to **your** hosted registry API.

## What’s in the repo

```
mpp-directory-skill/
└── SKILL.md    # Skill instructions (edit Base URL before use)
```

## Install

**Personal (all projects)** — copy the folder:

```bash
cp -R mpp-directory-skill ~/.cursor/skills/
```

**Project-only** — copy into the repo you’re working in:

```bash
mkdir -p .cursor/skills
cp -R mpp-directory-skill .cursor/skills/
```

Then mention the skill when you want it applied (e.g. “use the mpp-directory skill”), or rely on your agent rules if you map triggers to this skill.

## Before you publish

1. Open `mpp-directory-skill/SKILL.md`.
2. Replace the placeholder **Base URL** (`https://your-domain.com/api`) with your real registry origin (no trailing slash issues — follow your API’s style).

Optional: tighten the `description` in the YAML frontmatter so it matches your product name and triggers.

## API surface (summary)

| Action | Method | Purpose |
|--------|--------|---------|
| Search | `GET /api/endpoints/search` | Find rated endpoints by query, category, min rating |
| Report new | `POST /api/endpoints/new` | Register a 402 + `WWW-Authenticate: Payment` URL not in the registry |
| Review | `POST /api/endpoints/{endpoint_id}/review` | Required feedback after each use (success or failure) |

Full request shapes and rules are in `SKILL.md`.

## License

Add a `LICENSE` file if you care about reuse; this template ships without one by default.
