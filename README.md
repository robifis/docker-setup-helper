# 🐳 Docker Deployment Engine

Transform your AI assistant into a professional DevOps engineer. The **Docker Deployment Engine** is a set of advanced Assistant Skills (for Claude Code and OpenAI Codex) that automate the complex parts of Docker deployment while keeping the user in full control.

Unlike simple "helpers," this engine proactively resolves conflicts, self-heals from crashes, and delivers a polished interactive dashboard upon completion.

## 🚀 The "Engine" Workflow

The tool moves through a high-precision deployment lifecycle:

1. **Repo Deep-Scan**: Analyzes documentation, compose files, and manifests to map the app architecture.
2. **System Audit**: Verifies Docker health and proactively scans for host port conflicts.
3. **Auto-Resolution**: Instead of just warning about busy ports, it automatically proposes the nearest available alternatives.
4. **Secret Intelligence**: Generates secure random strings (Base32/Base64) for missing local secrets and guides users through required credentials.
5. **Validation Gate**: Every proposal is verified via `docker compose config` before the user is asked to execute.
6. **Self-Healing Loop**: If a container fails, the engine reads the logs, diagnoses the root cause (e.g., permission issues, DB timeouts), and proposes a targeted fix.
7. **Interactive Dashboard**: replaces static summaries with a `dashboard.html` featuring one-click access, a credential vault, and a quick-start command guide.

---

## 🛠️ Installation

### Claude Code (CLI)
**via Plugin Marketplace (Recommended):**
```text
/plugin marketplace add robifis/docker-setup-helper
/plugin install docker-setup-helper@docker-setup-helper
```
**Manual Installation:**
```bash
git clone https://github.com/robifis/docker-setup-helper.git && mkdir -p "$HOME/.claude/skills" && cp -R docker-setup-helper/skills/docker-setup-helper "$HOME/.claude/skills/"
```

### OpenAI Codex
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

Once installed, give your assistant a clear directive:

> "Use the **Docker Deployment Engine** to help me deploy this repository: `https://github.com/username/repo`"

The engine will then lead you through the audit, resolution, and deployment phases, ending with your interactive `dashboard.html`.

## 🛡️ Safety & Reliability Guarantees

- ✅ **Human-in-the-Loop**: No file is edited and no container is started without explicit user approval.
- ✅ **Syntactic Certainty**: All compose files are validated via `docker compose config` before deployment.
- ✅ **Data Integrity**: Never overwrites existing `.env` files; preserves existing values.
- ✅ **Credential Safety**: Never guesses production secrets; uses secure generation for local-only values.
- ✅ **Non-Destructive**: Never deletes images, volumes, or networks unless specifically requested.
