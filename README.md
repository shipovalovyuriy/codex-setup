# Codex Setup

This repository contains a local Codex setup bundle: agent definitions, reusable skills, and workspace-level instructions for how Codex should operate in this environment.

It is intended to be read by both humans and Codex. Keep the files clear, explicit, and easy to audit.

## Contents

- `AGENTS.md` - workspace instructions that define routing rules, planning expectations, and when agent delegation is allowed.
- `agents/` - TOML definitions for specialized Codex agents.
- `skills/` - reusable skill packages. Each skill is anchored by a `SKILL.md` file and may include scripts, references, assets, and agent metadata.
- `version.json` - local version metadata for this setup.

## Agent Definitions

Agent configs live in `agents/*.toml`.

Current agent roles include:

- `architect`
- `architect-deep`
- `backend-worker`
- `cybersec`
- `debugger`
- `docs`
- `explorer`
- `frontend-worker`
- `qa`
- `reviewer`
- `supervisor`
- `triage`
- `uiux-designer`
- `worker`

Each agent file should define the model, reasoning effort, sandbox behavior, and role-specific developer instructions. Keep agent responsibilities narrow so routing remains predictable.

## Skills

Skill packages live in `skills/<skill-name>/`.

Current skills include support for:

- Figma workflows and design-system generation
- image generation
- Playwright/browser validation
- screenshots
- spreadsheets
- PDFs
- Vercel deployment
- security assistance, best practices, threat modeling, and ownership maps
- Atlas tooling
- public frontend/backend guidance skills

A skill should normally include:

- `SKILL.md` - the entrypoint and usage instructions.
- `agents/openai.yaml` - optional agent metadata.
- `scripts/` - optional executable helpers.
- `references/` - optional deeper documentation.
- `assets/` - optional icons or static assets.

## Operating Rules

The main behavior rules are in `AGENTS.md`. The most important ones are:

- Do not delegate to agents unless the user explicitly asks for delegation, sub-agents, or parallel agent work.
- Use lightweight direct handling for small or tightly scoped tasks.
- Use `architect` for normal non-trivial design work that needs an implementation brief.
- Use `architect-deep` only for complex features, significant refactors, migrations, contract changes, or unresolved high-impact tradeoffs.
- Use the OpenAI developer documentation MCP server when working with OpenAI APIs, Codex, ChatGPT Apps SDK, or related developer tooling.

## Maintenance Guidelines

When changing this repository:

- Keep instructions concrete and testable.
- Prefer small, focused changes over broad rewrites.
- Keep each skill self-contained and document any required environment variables or external tools.
- Keep scripts reusable; avoid one-off logic hidden in instructions.
- Do not commit local secrets, API keys, private config, or machine-specific files.
- Validate TOML syntax after editing agent definitions.
- Check that every new skill has a clear `name`, `description`, and workflow in `SKILL.md`.

## Git Hygiene

The `.gitignore` excludes local Codex config, secret-bearing files, environment files, private keys, certificates, local backups, and OS noise.

Before committing, check:

```sh
git status --short
```

For agent config changes, also inspect the edited TOML files carefully before committing.

## Version Metadata

`version.json` stores local setup version information, including the latest known version and when it was last checked. Treat it as metadata for this bundle, not as application runtime state.
