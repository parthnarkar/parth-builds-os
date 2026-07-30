# OMNIROUTE RESOURCE GUIDE
## My Personal Workflow & Documentation Reference

---

## TABLE OF CONTENTS

1. What is OmniRoute
2. Installation & Setup
3. Connecting OmniRoute with Claude Code
4. My Document Structure
5. Essential Commands I Use
6. Free Providers & Auto-Routing
7. Best Practices & Tips

---

## 1. WHAT IS OMNIROUTE

OmniRoute is a free, open-source AI gateway that provides one unified endpoint for 290+ AI providers (90+ with free tiers) with auto-fallback and intelligent routing .

**Core Philosophy:**
- One endpoint for all AI tools
- Never hit limits through auto-fallback across providers
- Save 15-95% tokens with RTK + Caveman compression
- $0 to start with 90+ free providers 

**Key Features:**
- 290 AI providers through one endpoint
- 19 routing strategies (priority, weighted, cost-optimized, auto, fusion, pipeline)
- 12-engine compression pipeline
- MCP Server with 104 tools
- A2A protocol support
- Local-first with AES-256-GCM encrypted keys

**Repository:** https://github.com/diegosouzapw/OmniRoute

---

## 2. INSTALLATION & SETUP

### Prerequisites
- Node.js 22.22.2+ or 24.x
- npm or pnpm

### Global Installation
```bash
# Using npm
npm install -g omniroute

# Using pnpm (faster)
pnpm add -g omniroute@latest --allow-build=better-sqlite3 --allow-build=@swc/core

# Start the server
omniroute
```

### Docker Installation
```bash
docker run -d --name omniroute --restart unless-stopped \
  -p 127.0.0.1:20128:20128 \
  -v omniroute-data:/app/data \
  diegosouzapw/omniroute:latest
```

### Verify Installation
```bash
# Server runs on http://localhost:20128
# Dashboard at http://localhost:20128/dashboard

# Test the API
curl http://localhost:20128/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{"model":"auto","messages":[{"role":"user","content":"Hello!"}]}'
```

---

## 3. CONNECTING OMNIROUTE WITH CLAUDE CODE

### Overview
Claude Code connects to OmniRoute via the Anthropic Messages API, using environment variables since Claude Code has no `--base-url` flag .

### Step-by-Step Setup

#### Step 1: Start OmniRoute
```bash
omniroute
```
Server runs on `http://localhost:20128`

#### Step 2: Get an OmniRoute API Key
1. Open http://localhost:20128/dashboard/api-manager
2. Click "Create API Key"
3. Name it (e.g., `claude-code`)
4. Select all permissions
5. Copy the key (format: `sk-xxxxxxxxxxxxxxxx-xxxxxxxxx`)

#### Step 3: Install Claude Code
```bash
npm install -g @anthropic-ai/claude-code
```

#### Step 4: Configure Claude Code
**Method A: Quick Launch (Recommended)**
```bash
# Launch with OmniRoute (auto-detects local server)
omniroute launch

# Or specify remote server
omniroute launch --remote http://192.168.0.15:20128 --api-key sk-your-key
```

**Method B: Manual Configuration**
Create `~/.claude/settings.json`:
```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "http://localhost:20128",
    "ANTHROPIC_AUTH_TOKEN": "sk-your-omniroute-key"
  }
}
```
**Important:** Do NOT append `/v1` to the base URL .

**Method C: Environment Variables**
```bash
export ANTHROPIC_BASE_URL="http://localhost:20128"
export ANTHROPIC_AUTH_TOKEN="sk-your-omniroute-key"
export ANTHROPIC_MODEL="auto"

# Launch Claude Code
claude
```

#### Step 5: Optional - Create Per-Model Profiles
Generate multiple profiles for different models:
```bash
# Generate profiles
omniroute setup-claude

# Generate only specific providers
omniroute setup-claude --only glm,kimi

# Launch with a specific profile
omniroute launch --profile glm52
```

Profiles are stored at `~/.claude/profiles/<name>/settings.json` .

#### Step 6: Verify Connection
```bash
claude "say hello"
```

### Claude Code Environment Variables

| Variable | Purpose |
|----------|---------|
| `ANTHROPIC_BASE_URL` | Gateway root URL (no `/v1` suffix) |
| `ANTHROPIC_AUTH_TOKEN` | Authorization: Bearer token |
| `ANTHROPIC_API_KEY` | Alternative: x-api-key |
| `ANTHROPIC_MODEL` | Force specific model |
| `CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY` | 1 = list gateway models in `/model` picker |
| `CLAUDE_CODE_MAX_OUTPUT_TOKENS` | Cap output tokens |
| `CLAUDE_CODE_AUTO_COMPACT_WINDOW` | Auto-compaction threshold |

### Remote Setup
```bash
# Connect to remote OmniRoute
omniroute connect 192.168.0.15

# Launch Claude Code against remote server
omniroute launch

# Generate profiles for remote server
omniroute setup-claude --remote http://192.168.0.15:20128 --api-key oma_live_xxx
```

### Troubleshooting

| Issue | Solution |
|-------|----------|
| Claude Code ignores gateway | Confirm `ANTHROPIC_BASE_URL` has no `/v1`, restart claude |
| `/model` picker empty | Use Claude Code v2.1.129+, set `CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY=1` |
| Auth errors | Profile holds no token; use `omniroute launch --profile` or export `ANTHROPIC_AUTH_TOKEN` |
| Profiles don't isolate | Each profile is a distinct `CLAUDE_CONFIG_DIR` |

---

## 4. MY DOCUMENT STRUCTURE

### OmniRoute Project Structure
```
~/.omniroute/
├── data/
│   ├── omniroute.db          # SQLite database (providers, keys, usage)
│   └── settings.json          # User settings
├── claude/
│   └── profiles/
│       ├── glm52/
│       │   └── settings.json
│       ├── kimi-k27/
│       │   └── settings.json
│       └── deepseek-pro/
│           └── settings.json
├── codex/
│   └── config.yaml            # Codex configuration
└── opencode/
    └── opencode.json          # OpenCode configuration
```

### Key Documents I Maintain

**1. API Key Management**
- Dashboard → API Manager (`/dashboard/api-manager`)
- Create scoped keys per tool
- Track usage per key

**2. Provider Configuration**
- Dashboard → Providers (`/dashboard/providers`)
- Connect free providers (Kiro, Qoder, OpenCode, Pollinations)
- Add API key providers (DeepSeek, Groq, xAI)

**3. Combo Configuration**
- Dashboard → Combos (`/dashboard/combos`)
- Define fallback chains
- Set routing strategies

**4. CLI Tool Profiles**
- Generated via `omniroute setup-*` commands
- Stored in tool-specific directories
- Never store API keys in profiles (injected at launch)

---

## 5. ESSENTIAL COMMANDS I USE

### OmniRoute CLI Commands

| Command | Purpose |
|---------|---------|
| `omniroute` | Start server (dashboard + API on port 20128) |
| `omniroute launch` | Launch Claude Code with OmniRoute env injected |
| `omniroute launch-codex` | Launch Codex CLI with OmniRoute env injected |
| `omniroute setup-claude` | Generate Claude Code profiles |
| `omniroute setup-codex` | Generate Codex profiles |
| `omniroute setup-opencode` | Configure OpenCode |
| `omniroute setup-cline` | Configure Cline |
| `omniroute setup-continue` | Configure Continue |
| `omniroute setup-aider` | Configure Aider |
| `omniroute connect` | Connect to remote OmniRoute instance |
| `omniroute doctor` | Diagnose providers, ports, dependencies |
| `omniroute providers list` | List connected providers |
| `omniroute combo list` | List configured combos |
| `omniroute health` | Check server health |

### Claude Code Integration Commands
```bash
# Quick launch (no config)
omniroute launch

# Generate all model profiles
omniroute setup-claude

# Launch with specific profile
omniroute launch --profile glm52

# Preview profile generation
omniroute setup-claude --dry-run

# Generate profiles for specific providers
omniroute setup-claude --only glm,kimi

# Remote setup
omniroute setup-claude --remote http://192.168.0.15:20128 --api-key oma_live_xxx
```

---

## 6. FREE PROVIDERS & AUTO-ROUTING

### Free Forever Providers (No Card Required)

| Provider | Models | Notes |
|----------|--------|-------|
| OpenCode Zen | DeepSeek V4, Nemotron 3 | No token cap |
| Qoder AI | Qwen3-Max, Kimi-K2 | Unlimited |
| Pollinations | GPT, Llama, Claude | No key needed |
| SiliconFlow | DeepSeek V3.2/R1 | Free tier |
| Z.AI GLM | GLM-4.7, GLM-4.5-Flash | Free forever |
| Kilo Code | Auto-router | Free forever |
| Requesty | GPT-OSS 120B, Nemotron | Free forever |
| Baidu ERNIE | ERNIE 4.0 | Free forever |

### Auto-Routing Model IDs

| Model ID | Optimizes For |
|----------|---------------|
| `auto` | Balanced default (LKGP) |
| `auto/coding` | Quality-first for code generation |
| `auto/fast` | Lowest latency first |
| `auto/cheap` | Cheapest per token first |
| `auto/offline` | Most quota/rate-limit headroom |
| `auto/smart` | Quality-first + 10% exploration |

### 4-Tier Fallback System

```
Tier 1: SUBSCRIPTION → Claude Code, Codex, Copilot
                     ↓ quota exhausted
Tier 2: API KEY     → DeepSeek, Groq, xAI, Mistral
                     ↓ budget hit
Tier 3: CHEAP       → GLM ($0.5/1M), MiniMax ($0.2/1M)
                     ↓ budget hit
Tier 4: FREE        → Kiro, Qoder, Pollinations (always on)
```

---

## 7. BEST PRACTICES & TIPS

### Getting Started

1. **Start with auto model** - Use `model: "auto"` for zero-config smart routing
2. **Connect free providers first** - Add free providers from Dashboard → Providers
3. **Use launch commands** - `omniroute launch` handles env injection automatically
4. **Generate profiles once** - Run `omniroute setup-claude` to create all model profiles

### Performance Optimization

1. **Enable compression**: Set `"compression": "stacked"` for 15-95% token savings
2. **Use profiles**: Each profile isolates history and cache for that model
3. **Set model discovery**: `CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY=1`
4. **Cap output tokens**: Set `CLAUDE_CODE_MAX_OUTPUT_TOKENS=65536`

### Security Best Practices

1. **Never store API keys in profiles** - Use `omniroute launch` to inject them
2. **Create scoped API keys** - Per tool or per user keys
3. **Use remote mode securely** - `omniroute connect` creates scoped tokens
4. **Keep OmniRoute updated**: `npm update -g omniroute` or `pnpm update -g omniroute`

### Remote Usage

1. **Deploy OmniRoute on a VPS** - Use Docker or Railway
2. **Connect once**: `omniroute connect <server-ip>`
3. **All commands auto-target**: `omniroute launch`, `omniroute setup-claude`
4. **Persistent storage**: Use Railway Volume or Docker volume for SQLite

### Troubleshooting Quick Reference

| Symptom | Check | Fix |
|---------|-------|-----|
| Claude Code not connecting | `ANTHROPIC_BASE_URL` has no `/v1` | Remove `/v1` suffix |
| Model picker empty | Gateway model discovery disabled | Set `CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY=1` |
| Auth errors | Token not injected | Use `omniroute launch --profile` |
| Rate limits | Free tier quota exhausted | Add more providers or upgrade tier |
| Compression not working | Compression disabled | Enable in dashboard or via header |

---

## QUICK REFERENCE CARD

### OmniRoute Setup
```bash
# Install
npm install -g omniroute

# Start
omniroute

# Dashboard
http://localhost:20128/dashboard
```

### Claude Code Connection
```bash
# Quick launch (recommended)
omniroute launch

# Generate profiles
omniroute setup-claude

# Launch with profile
omniroute launch --profile glm52

# Manual env
export ANTHROPIC_BASE_URL="http://localhost:20128"
export ANTHROPIC_AUTH_TOKEN="sk-your-key"
export ANTHROPIC_MODEL="auto"
claude
```

### Key Environment Variables
```bash
ANTHROPIC_BASE_URL="http://localhost:20128"
ANTHROPIC_AUTH_TOKEN="sk-your-omniroute-key"
ANTHROPIC_MODEL="auto"
CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY=1
```

### Useful Commands
```bash
# Check server health
omniroute health

# List providers
omniroute providers list

# List combos
omniroute combo list

# Diagnose issues
omniroute doctor

# Remote connection
omniroute connect 192.168.0.15
```

---

*This resource guide reflects my personal OmniRoute workflow with Claude Code integration. The toolkit is continuously evolving with ~500 contributors and 34.5k stars on GitHub.*

*For complete documentation: https://github.com/diegosouzapw/OmniRoute*

---

## 🤝 Contributing

Found an amazing resource? Want to add your favorite course? Check out our [CONTRIBUTING.md](../CONTRIBUTING.md) to learn how you can help grow this collection!

## 🔗 Connect With Me

[![Instagram](https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white)](https://instagram.com/parth.builds)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/parthnarkar)
[![LinkedIn](https://img.shields.io/badge/-LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/parthnarkar)
[![LeetCode Profile](https://img.shields.io/badge/LeetCode-ParthNarkar-FFA116?style=for-the-badge&logo=leetcode)](https://leetcode.com/u/parthnarkar/)
[![Email](https://img.shields.io/badge/-Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:parthnarkarofficial@gmail.com)
[![Twitter](https://img.shields.io/badge/-Twitter-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white)](https://twitter.com/parthnarkar)
[![Discord](https://img.shields.io/badge/-Discord-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.com/users/parth_narkar)

### ⭐ Found this helpful? Give this Repo a STAR!

[![parth-builds-os Github Repo Footer](https://github.com/user-attachments/assets/4bef3a04-16ee-4484-a52c-4f31182e1916)](https://github.com/parthnarkar/parth-builds-os)
