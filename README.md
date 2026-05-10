# 🐳 Docker Setup Helper

Empower your AI assistant to guide beginners through the process of deploying Dockerized apps from GitHub repositories safely and predictably.

This repository provides a set of **Assistant Skills** (for Claude Code and OpenAI Codex) that transform a "blind" deployment into a structured, staged workflow.

## 🚀 What it Does

Instead of an AI guessing how to start a container, the `docker-setup-helper` forces a professional deployment lifecycle:
1. **Repo Inspection**: Analyzes READMEs, Compose files, and `.env.example` to understand the app.
2. **System Audit**: Checks if Docker is running and ports are available.
3. **Plain-English Translation**: Explains what `redis`, `postgres`, or `worker` actually do for the user.
4. **Safety First**: Validates configs using `docker compose config` and uses a mandatory safety checklist.
5. **Structured Runbook**: Generates a clear, step-by-step deployment plan.
6. **Starter Guide**: Creates a `summary.md` upon success with access URLs and default login credentials.

---

## 🛠️ Installation

### Claude Code (CLI)
Install as a personal skill:
```bash
git clone https://github.com/robifis/docker-setup-helper.git && mkdir -p "$HOME/.claude/skills" && cp -R docker-setup-helper/skills/docker-setup-helper "$HOME/.claude/skills/"
```
**Or via Plugin Marketplace:**
```text
/plugin marketplace add robifis/docker-setup-helper
/plugin install docker-setup-helper@docker-setup-helper
```

### OpenAI Codex
Install as a user-level skill:
```bash
git clone https://github.com/robifis/docker-setup-helper.git && mkdir -p "$HOME/.agents/skills" && cp -R docker-setup-helper/.agents/skills/docker-setup-helper "$HOME/.agents/skills/"
```

### Windows PowerShell (Install Both)
```powershell
git clone https://github.com/robifis/docker-setup-helper.git; New-Item -ItemType Directory -Force "$HOME\.agents\skills", "$HOME\.claude\skills"; Copy-Item ".\docker-setup-helper\.agents\skills\docker-setup-helper" "$HOME\.agents\skills\" -Recurse -Force; Copy-Item ".\docker-setup-helper\skills\docker-setup-helper" "$HOME\.claude\skills\" -Recurse -Force
```

### Claude.ai (Web Upload)
Create a zip where the root contains a `docker-setup-helper/` folder with `SKILL.md` inside.

---

## 📖 How to Use

Once installed, simply tell your assistant:

> "Use the **Docker Setup Helper** to help me deploy this repository: `https://github.com/username/repo`"

The assistant will then guide you through the stages, from initial inspection to the final `summary.md` starter guide.

## 🛡️ Safety Guarantees

The skill is hard-coded to follow these safety defaults:
- ✅ **No Magic**: Every file change and command is proposed and approved by the user.
- ✅ **Config Validation**: Always runs `docker compose config` before execution.
- ✅ **Data Protection**: Never overwrites existing `.env` files.
- ✅ **Zero Invention**: Never guesses API keys or secrets; identifies them as `USER_REQUIRED`.
- ✅ **Non-Destructive**: Never deletes containers, images, or volumes unless explicitly requested.
