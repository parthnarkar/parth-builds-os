# SPEC-KIT RESOURCE GUIDE
## My Personal Workflow & Documentation Reference

---

## TABLE OF CONTENTS

1. Introduction to Spec-Kit
2. Installation & Setup
3. My Core Workflow
4. Essential Commands I Use
5. My Document Structure
6. Extensions & Presets I Leverage
7. Integration with AI Coding Agents
8. Best Practices & Tips

---

## 1. INTRODUCTION TO SPEC-KIT

Spec-Kit is an open source toolkit that implements Spec-Driven Development (SDD) - a methodology where specifications become executable and directly generate working implementations.

**Core Philosophy:**
- Intent-driven development (specify the "what" before the "how")
- Rich specification creation using guardrails and organizational principles
- Multi-step refinement rather than one-shot code generation
- Heavy reliance on advanced AI model capabilities

---

## 2. INSTALLATION & SETUP

### Prerequisites
- Python 3.11+
- Git
- uv (recommended) or pipx
- Supported AI coding agent

### Installation Command
```bash
# Using uv (recommended)
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git@vX.Y.Z

# Or from PyPI
uv tool install specify-cli
```

### Project Initialization
```bash
specify init my-project --integration copilot
cd my-project
```

### Upgrade Management
```bash
# Check for updates
specify self check

# Preview upgrade
specify self upgrade --dry-run

# Execute upgrade
specify self upgrade

# Pin specific version
specify self upgrade --tag vX.Y.Z
```

---

## 3. MY CORE WORKFLOW

I follow a structured 7-step process for every project:

| Step | Phase | Command | Purpose |
|------|-------|---------|---------|
| 1 | Principles | `/speckit.constitution` | Define governing principles & guidelines |
| 2 | Specification | `/speckit.specify` | Define what to build (requirements & user stories) |
| 3 | Clarification | `/speckit.clarify` | Clarify underspecified areas |
| 4 | Planning | `/speckit.plan` | Create technical implementation plan |
| 5 | Analysis | `/speckit.analyze` | Cross-artifact consistency & coverage |
| 6 | Task Breakdown | `/speckit.tasks` | Generate actionable task list |
| 7 | Implementation | `/speckit.implement` | Execute all tasks to build the feature |

### Optional Commands I Use
- `/speckit.checklist` - Generate quality checklists
- `/speckit.converge` - Assess codebase against spec and append remaining work
- `/speckit.taskstoissues` - Convert tasks to GitHub issues

---

## 4. DOCUMENTS I MAINTAIN

### Core Document Structure

```
my-project/
├── .specify/
│   ├── memory/
│   │   └── constitution.md        # Project principles
│   ├── templates/
│   │   ├── overrides/              # Project-local customizations
│   │   ├── spec-template.md
│   │   ├── plan-template.md
│   │   └── tasks-template.md
│   ├── presets/                    # Customized workflows
│   └── extensions/                 # Additional capabilities
├── specs/
│   └── 001-feature-name/
│       ├── spec.md                 # Requirements & user stories
│       ├── plan.md                 # Technical implementation
│       ├── tasks.md                # Actionable task list
│       └── research.md             # Research notes
└── .claude/
    └── commands/                   # Slash command definitions
```

### Key Documents I Create

**1. Constitution (`.specify/memory/constitution.md`)**
- Project governing principles
- Development guidelines
- Quality standards
- Testing requirements
- Performance expectations

**2. Specification (`specs/###-feature/spec.md`)**
- User stories
- Requirements (functional & non-functional)
- Acceptance criteria
- User personas

**3. Implementation Plan (`specs/###-feature/plan.md`)**
- Tech stack decisions
- Architecture choices
- Component breakdown
- Data models
- API design

**4. Task List (`specs/###-feature/tasks.md`)**
- Actionable implementation steps
- Task dependencies
- Priority ordering
- Estimated effort

**5. Research Notes (`specs/###-feature/research.md`)**
- Technical investigation findings
- Alternative evaluations
- Decision rationale

---

## 5. EXTENSIONS & PRESETS I LEVERAGE

### Extensions (New Capabilities)
Extensions add new commands and workflows beyond the core SDD functionality.

**My Extension Workflow:**
```bash
# Search available extensions
specify extension search

# Install an extension
specify extension add <extension-name>

# List installed extensions
specify extension list

# Remove an extension
specify extension remove <extension-name>
```

**Types of Extensions I Use:**
- Jira integration
- Post-implementation code review
- V-Model test traceability
- Project health diagnostics
- Security scanning integration

### Presets (Customize Existing Workflows)
Presets override templates and commands to adapt Spec-Kit to specific needs.

**My Preset Workflow:**
```bash
# Search available presets
specify preset search

# Install a preset
specify preset add <preset-name>

# View current preset stack
specify preset list

# Remove a preset
specify preset remove <preset-name>
```

**Customizations I Apply:**
- Compliance-oriented spec formats
- Domain-specific terminology
- Organizational standards
- Regulatory traceability requirements
- Security review gates
- Test-first task ordering
- Multilingual localization

### Resolution Priority Stack

| Priority | Component Type | Location |
|----------|----------------|----------|
| 1 | Project-Local Overrides | `.specify/templates/overrides/` |
| 2 | Presets | `.specify/presets/templates/` |
| 3 | Extensions | `.specify/extensions/templates/` |
| 4 | Spec-Kit Core | `.specify/templates/` |

---

## 6. BUNDLES: ROLE-BASED SETUPS

For complete team provisioning, I use bundles that package extensions, presets, steps, and workflows into a single setup.

**Bundle Workflow:**
```bash
# Discover bundles
specify bundle search <query>

# Inspect bundle contents
specify bundle info <bundle-id>

# Install a bundle
specify bundle install <bundle-id>

# List installed bundles
specify bundle list

# Update a bundle
specify bundle update <bundle-id>

# Remove a bundle
specify bundle remove <bundle-id>
```

**Bundle Management:**
```bash
# Manage catalog sources
specify bundle catalog list
specify bundle catalog add <source>
specify bundle catalog remove <source>

# Validate a bundle
specify bundle validate --path ./my-bundle

# Build a bundle
specify bundle build --path ./my-bundle
```

---

## 7. INTEGRATION WITH AI CODING AGENTS

I use Spec-Kit with multiple AI coding agents:

### Supported Integrations
- GitHub Copilot
- Claude
- Codex CLI
- IDE-based assistants
- 30+ other integrations

### Available Commands by Agent

**Slash Commands (most agents):**
- `/speckit.constitution`
- `/speckit.specify`
- `/speckit.plan`
- `/speckit.tasks`
- `/speckit.implement`

**Skills Mode (Codex CLI):**
- `$speckit-constitution`
- `$speckit-specify`
- `$speckit-plan`
- `$speckit-tasks`
- `$speckit-implement`

**Integration Setup:**
```bash
# List available integrations
specify integration list

# Initialize with specific integration
specify init my-project --integration <agent>

# Use skills mode
specify init my-project --integration <agent> --integration-options="--skills"
```

---

## 8. BEST PRACTICES & TIPS

### Development Phases

**Greenfield (0-to-1 Development):**
- Start with high-level requirements
- Generate specifications
- Plan implementation steps
- Build production-ready applications

**Brownfield (Iterative Enhancement):**
- Add features iteratively
- Modernize legacy systems
- Adapt processes
- Keep tooling updates separate from feature artifact evolution

### My Proven Workflow Tips

1. **Start with Constitution** - Always establish principles before writing specs
2. **Use Clarify Command** - Run `/speckit.clarify` before planning for better requirements
3. **Analyze Before Implementation** - `/speckit.analyze` catches consistency issues early
4. **Stack Customizations** - Use overrides for single project, presets for organizational standards
5. **Version Control Everything** - Keep all spec documents in version control
6. **Review Generated Artifacts** - AI-generated documents need human review
7. **Maintain Separation** - Keep spec artifacts separate from code

### Common Pitfalls to Avoid

- Skipping the constitution step
- Not clarifying requirements before planning
- Over-customizing without understanding defaults
- Ignoring the analysis phase
- Treating generated tasks as final without review

### Troubleshooting

**Outdated CLI:**
```bash
specify self upgrade
```

**Integration Issues:**
```bash
specify integration list
specify init --integration <agent>
```

**Extension Conflicts:**
```bash
specify extension list
specify extension remove <extension-name>
```

---

## DOCUMENTATION SUMMARY

| Document | Purpose | Location |
|----------|---------|----------|
| Constitution | Project principles & guidelines | `.specify/memory/constitution.md` |
| Specification | Requirements & user stories | `specs/###-feature/spec.md` |
| Implementation Plan | Technical architecture | `specs/###-feature/plan.md` |
| Task List | Actionable work items | `specs/###-feature/tasks.md` |
| Research Notes | Technical investigation | `specs/###-feature/research.md` |
| Overrides | Project-local customizations | `.specify/templates/overrides/` |
| Presets | Workflow customizations | `.specify/presets/templates/` |
| Extensions | Added capabilities | `.specify/extensions/templates/` |

---

## QUICK REFERENCE CARD

### Essential Commands
```bash
# Project Setup
specify init PROJECT --integration AGENT

# Self Management
specify self check
specify self upgrade

# Extensions
specify extension search
specify extension add NAME

# Presets
specify preset search
specify preset add NAME

# Bundles
specify bundle search
specify bundle install ID
```

### Slash Commands (in agent)
```
/speckit.constitution    - Set project principles
/speckit.specify         - Define requirements
/speckit.clarify         - Clarify ambiguity
/speckit.plan            - Create implementation plan
/speckit.analyze         - Check consistency
/speckit.tasks           - Generate task list
/speckit.implement       - Execute implementation
/speckit.checklist       - Quality checklist
/speckit.converge        - Assess remaining work
```

---

*This resource guide reflects my personal Spec-Kit workflow and document structure. The toolkit is continuously evolving, so I regularly check for updates using the self-management commands.*

*For complete documentation: https://github.com/github/spec-kit*

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
