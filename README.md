# Docker Setup Helper

A single-file assistant workflow for helping beginners set up Docker apps from GitHub repositories.

The main file is [`SKILL.md`](SKILL.md). It is self-contained and can be used as:

- a Codex skill,
- Claude project instructions,
- or a normal Markdown checklist.

The workflow is intentionally staged. It inspects first, explains what it found, asks simple questions when decisions are needed, and proposes file changes before applying anything.

## Quick Start

Paste this into Codex, Claude, or another assistant:

```text
Use the Docker Setup Helper workflow to help me deploy this GitHub repo:
<paste repo URL>

Inspect first, explain each stage in beginner-friendly terms, ask before making decisions, and produce a runbook plus proposed patches before changing files or starting containers.
```

## Codex Install

Copy this repository folder into your Codex skills directory:

```bash
cp -r docker-setup-helper "$HOME/.codex/skills/docker-setup-helper"
```

Then invoke it with:

```text
Use $docker-setup-helper to help me deploy https://github.com/example/app
```

## Claude Use

Paste [`SKILL.md`](SKILL.md) into Claude project instructions, or attach it to the project where you want Docker setup help.

## Safety Defaults

- Inspect, explain, and propose first.
- Ask before editing files.
- Ask before starting containers or running migrations.
- Ask before changing ports or exposing services.
- Never overwrite `.env`.
- Never invent third-party API keys.
- Never delete Docker containers, images, networks, or volumes unless explicitly asked.
