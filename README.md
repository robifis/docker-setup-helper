# Docker Setup Helper

Assistant skills for helping beginners set up Docker apps from GitHub repositories.

This repo ships the same Docker setup workflow in two platform-specific layouts:

- [`skills/docker-setup-helper/SKILL.md`](skills/docker-setup-helper/SKILL.md) follows the [Anthropic skills repository](https://github.com/anthropics/skills) pattern, with [`/.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json) for Claude Code plugin discovery.
- [`.agents/skills/docker-setup-helper/SKILL.md`](.agents/skills/docker-setup-helper/SKILL.md) follows the [OpenAI Codex skill docs](https://developers.openai.com/codex/skills#create-a-skill) pattern, with optional Codex UI metadata in `.agents/skills/docker-setup-helper/agents/openai.yaml`.

Both versions follow the same staged workflow: inspect first, explain what was found, ask simple questions when decisions are needed, and propose file changes before applying anything.

## Quick Start

Paste this into Codex, Claude, or another assistant:

```text
Use the Docker Setup Helper workflow to help me deploy this GitHub repo:
<paste repo URL>

Inspect first, explain each stage in beginner-friendly terms, ask before making decisions, and produce a runbook plus proposed patches before changing files or starting containers.
```

## Install For Codex

Codex supports user-level skills under `~/.agents/skills`. Run this from any folder:

```bash
git clone https://github.com/robifis/docker-setup-helper.git && mkdir -p "$HOME/.agents/skills" && cp -R docker-setup-helper/.agents/skills/docker-setup-helper "$HOME/.agents/skills/"
```

Then invoke it with:

```text
Use $docker-setup-helper to help me deploy https://github.com/example/app
```

## Install For Claude Code

Claude Code supports personal skills under `~/.claude/skills`. Run this from any folder:

```bash
git clone https://github.com/robifis/docker-setup-helper.git && mkdir -p "$HOME/.claude/skills" && cp -R docker-setup-helper/skills/docker-setup-helper "$HOME/.claude/skills/"
```

Claude expects the skill folder name to match the `name` field, so the file is copied to:

```bash
~/.claude/skills/docker-setup-helper/SKILL.md
```

## Install Both

```bash
git clone https://github.com/robifis/docker-setup-helper.git && mkdir -p "$HOME/.agents/skills" "$HOME/.claude/skills" && cp -R docker-setup-helper/.agents/skills/docker-setup-helper "$HOME/.agents/skills/" && cp -R docker-setup-helper/skills/docker-setup-helper "$HOME/.claude/skills/"
```

## Windows PowerShell Install Both

```powershell
git clone https://github.com/robifis/docker-setup-helper.git; New-Item -ItemType Directory -Force "$HOME\.agents\skills", "$HOME\.claude\skills"; Copy-Item ".\docker-setup-helper\.agents\skills\docker-setup-helper" "$HOME\.agents\skills\" -Recurse -Force; Copy-Item ".\docker-setup-helper\skills\docker-setup-helper" "$HOME\.claude\skills\" -Recurse -Force
```

## Claude.ai Upload

For Claude.ai custom skill upload, create a zip whose root contains a `docker-setup-helper/` folder with `SKILL.md` inside:

```bash
git clone https://github.com/robifis/docker-setup-helper.git && mkdir -p docker-setup-helper-upload && cp -R docker-setup-helper/skills/docker-setup-helper docker-setup-helper-upload/ && cd docker-setup-helper-upload && zip -r docker-setup-helper-claude.zip docker-setup-helper
```

## Claude Code Plugin Marketplace

This repository includes `.claude-plugin/marketplace.json`, mirroring Anthropic's example repository pattern. To use it as a Claude Code plugin marketplace, add this repository and install the plugin from Claude Code:

```text
/plugin marketplace add robifis/docker-setup-helper
/plugin install docker-setup-helper@docker-setup-helper
```

## Safety Defaults

- Inspect, explain, and propose first.
- Ask before editing files.
- Ask before starting containers or running migrations.
- Ask before changing ports or exposing services.
- Never overwrite `.env`.
- Never invent third-party API keys.
- Never delete Docker containers, images, networks, or volumes unless explicitly asked.
