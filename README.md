# Docker Setup Helper

Assistant skills for helping beginners set up Docker apps from GitHub repositories.

This repo contains two installable skill files:

- [`codex/SKILL.md`](codex/SKILL.md) for Codex.
- [`claude/SKILL.md`](claude/SKILL.md) for Claude / Claude Code.

Both versions follow the same staged workflow: inspect first, explain what was found, ask simple questions when decisions are needed, and propose file changes before applying anything.

## Quick Start

Paste this into Codex, Claude, or another assistant:

```text
Use the Docker Setup Helper workflow to help me deploy this GitHub repo:
<paste repo URL>

Inspect first, explain each stage in beginner-friendly terms, ask before making decisions, and produce a runbook plus proposed patches before changing files or starting containers.
```

## Install For Codex

Run this from any folder:

```bash
git clone https://github.com/robifis/docker-setup-helper.git && mkdir -p "$HOME/.codex/skills/docker-setup-helper" && cp docker-setup-helper/codex/SKILL.md "$HOME/.codex/skills/docker-setup-helper/SKILL.md"
```

Then invoke it with:

```text
Use $docker-setup-helper to help me deploy https://github.com/example/app
```

## Install For Claude Code

```bash
git clone https://github.com/robifis/docker-setup-helper.git && mkdir -p "$HOME/.claude/skills/docker-setup-helper" && cp docker-setup-helper/claude/SKILL.md "$HOME/.claude/skills/docker-setup-helper/SKILL.md"
```

Claude expects the skill folder name to match the `name` field, so the file is copied to:

```bash
~/.claude/skills/docker-setup-helper/SKILL.md
```

## Install Both

```bash
git clone https://github.com/robifis/docker-setup-helper.git && mkdir -p "$HOME/.codex/skills/docker-setup-helper" "$HOME/.claude/skills/docker-setup-helper" && cp docker-setup-helper/codex/SKILL.md "$HOME/.codex/skills/docker-setup-helper/SKILL.md" && cp docker-setup-helper/claude/SKILL.md "$HOME/.claude/skills/docker-setup-helper/SKILL.md"
```

## Windows PowerShell Install Both

```powershell
git clone https://github.com/robifis/docker-setup-helper.git; New-Item -ItemType Directory -Force "$HOME\.codex\skills\docker-setup-helper", "$HOME\.claude\skills\docker-setup-helper"; Copy-Item ".\docker-setup-helper\codex\SKILL.md" "$HOME\.codex\skills\docker-setup-helper\SKILL.md" -Force; Copy-Item ".\docker-setup-helper\claude\SKILL.md" "$HOME\.claude\skills\docker-setup-helper\SKILL.md" -Force
```

## Claude.ai Upload

For Claude.ai custom skill upload, create a zip whose root contains a `docker-setup-helper/` folder with `SKILL.md` inside:

```bash
git clone https://github.com/robifis/docker-setup-helper.git && mkdir -p docker-setup-helper-upload/docker-setup-helper && cp docker-setup-helper/claude/SKILL.md docker-setup-helper-upload/docker-setup-helper/SKILL.md && cd docker-setup-helper-upload && zip -r docker-setup-helper-claude.zip docker-setup-helper
```

## Safety Defaults

- Inspect, explain, and propose first.
- Ask before editing files.
- Ask before starting containers or running migrations.
- Ask before changing ports or exposing services.
- Never overwrite `.env`.
- Never invent third-party API keys.
- Never delete Docker containers, images, networks, or volumes unless explicitly asked.
