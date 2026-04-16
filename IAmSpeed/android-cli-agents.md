# Android CLI: Build Android Apps 3x Faster with Any Agent

[![Platform](https://img.shields.io/badge/Platform-Android-3DDC84?style=flat-square&logo=android&logoColor=white)](https://d.android.com/tools/agents)
[![Status](https://img.shields.io/badge/Status-Preview-orange?style=flat-square)]()
[![Source](https://img.shields.io/badge/Source-Android_Developers_Blog-blue?style=flat-square&logo=googlechrome&logoColor=white)](https://android-developers.googleblog.com/2026/04/build-android-apps-3x-faster-using-any-agent.html)

---

## What Is This?

Google introduced a new suite of **Android tools for agentic workflows** — designed to help AI agents build Android apps faster and smarter, outside of Android Studio.

Compatible with:

[![Gemini CLI](https://img.shields.io/badge/Gemini_CLI-compatible-4285F4?style=flat-square&logo=google&logoColor=white)]()
[![Claude Code](https://img.shields.io/badge/Claude_Code-compatible-D97757?style=flat-square&logo=anthropic&logoColor=white)]()
[![Codex](https://img.shields.io/badge/Codex-compatible-412991?style=flat-square&logo=openai&logoColor=white)]()
[![Antigravity](https://img.shields.io/badge/Antigravity-compatible-1a1a2e?style=flat-square&logo=google&logoColor=white)]()

The three components:
1. **Android CLI** — a terminal-first interface for Android development
2. **Android Skills** — modular SKILL.md instruction sets for LLMs
3. **Android Knowledge Base** — up-to-date Android docs accessible by agents

---

## Installation

[![Download](https://img.shields.io/badge/Download-Android_CLI-3DDC84?style=flat-square&logo=android&logoColor=white)](https://d.android.com/tools/agents)

1. Head to **[d.android.com/tools/agents](https://d.android.com/tools/agents)** and download the Android CLI
2. Install the required SDK components:
   ```bash
   android sdk install
   ```
3. (Optional) Set up Android Skills for your agent workflow:
   ```bash
   android skills
   ```
4. Keep the CLI up to date:
   ```bash
   android update
   ```

---

## Key Commands

| Command | What it does |
|---|---|
| `android sdk install` | Downloads only the SDK components you need |
| `android create` | Creates a new project from official templates |
| `android emulator` | Creates and manages virtual devices |
| `android run` | Builds and deploys the app to a device |
| `android skills` | Browse & set up Android skill packs |
| `android docs` | Fetches latest Android Knowledge Base docs |
| `android update` | Updates the CLI to the latest version |

---

## How to Use It

### 1. Set Up & Create a Project
```bash
android sdk install
android create
```
Creates a new app with the recommended architecture (Jetpack, Compose, etc.) in seconds.

### 2. Run on an Emulator
```bash
android emulator
android run
```
Spins up a virtual device and deploys your app automatically.

### 3. Ground Your Agent with Skills
```bash
android skills
```
Installs modular `SKILL.md` files that auto-trigger when your prompt matches a known workflow — no need to manually attach docs.

Initial skills include:
- Navigation 3 setup & migration
- Edge-to-edge implementation
- AGP 9 & XML-to-Compose migration
- R8 config analysis

### 4. Get Latest Docs for Your Agent
```bash
android docs
```
Pulls latest guidance from Android docs, Firebase, Google Developers, and Kotlin docs — even if the LLM's training is outdated.

---

## Why It Matters

[![Speed](https://img.shields.io/badge/Setup_Speed-3x_Faster-success?style=flat-square)]()
[![Tokens](https://img.shields.io/badge/LLM_Tokens-70%25_Reduced-informational?style=flat-square)]()

- **3x faster** project setup vs. agents using standard toolsets
- **70%+ reduction** in LLM token usage for setup tasks
- Works with any agent — Gemini CLI, Claude Code, Codex, Antigravity, and more
- Bridges the gap between quick AI prototyping and production Android Studio workflows

---

## Workflow Summary

```
Agent Prompt
    ↓
Android CLI (setup, create, run)
    ↓
Android Skills (best-practice instructions)
    ↓
Android Knowledge Base (latest docs)
    ↓
Android Studio (for polish, debugging, publishing)
```

---

## Using Custom / Third-Party / Community Skills

[![Standard](https://img.shields.io/badge/Open_Standard-agentskills.io-blueviolet?style=flat-square)](https://agentskills.io/specification)

Skills follow the **open [Agent Skills standard](https://agentskills.io/specification)**, so you're not limited to official Android skills. You can:

- **Create your own** skills for team-specific workflows
- **Use community skills** (e.g., KMP, architecture, third-party libraries)
- **Mix and match** official + custom skills in the same project

---

### Where Skills Live in Your Project

Place any custom skill in one of these directories at your **project root**:

```
.skills/
└── my-custom-skill/
    └── SKILL.md

# OR

.agent/skills/
└── my-custom-skill/
    └── SKILL.md
```

You can also use subdirectories to organize:
```
.skills/
├── ui-flows/edge-to-edge/SKILL.md
└── testing/screenshot-tests/SKILL.md
```

---

### SKILL.md Format

Every skill needs a `SKILL.md` file. Minimal template:

```yaml
---
name: my-skill-name          # unique, lowercase, max 64 chars
description: >               # max 1024 chars — agent reads this to decide when to use it
  What this skill does and when the agent should activate it.
metadata:
  author: your-name
  version: "1.0"
---

## Instructions

Write your step-by-step instructions in regular Markdown here.
The agent follows these when the skill is triggered.
```

**Format rules:**

| Field | Constraint |
|---|---|
| `name` | [![name](https://img.shields.io/badge/max_64_chars-lowercase_%2B_hyphens-lightgrey?style=flat-square)]() |
| `description` | [![desc](https://img.shields.io/badge/max_1024_chars-agent_trigger_text-lightgrey?style=flat-square)]() |
| Body content | [![body](https://img.shields.io/badge/10k–20k_chars-~2500–5000_tokens-lightgrey?style=flat-square)]() |

---

### Optional Subdirectories Inside a Skill

```
my-custom-skill/
├── SKILL.md           ← required
├── scripts/           ← Python/Bash scripts the agent can run
├── references/        ← API docs, technical specs, guides
└── assets/            ← JSON schemas, UI diagrams, templates
```

Reference them inside `SKILL.md` using relative paths:
```markdown
Run the setup script at `scripts/setup.sh` before proceeding.
```

---

### Install a Specific Official Skill by Name

```bash
android skills list                       # see all available official skills
android skills add --skill skill-name     # install a specific one
```

---

### Activating a Skill

Skills **auto-trigger** when your prompt matches the skill's `description`. Example:

> _"Make my app UI edge-to-edge"_ → agent auto-finds and uses `edge-to-edge` skill

In **Android Studio**, you can also invoke a skill manually:
```
@skill-name
```

---

### Finding Community Skills

[![Skills Hub](https://img.shields.io/badge/Android_KMP_Skills_Hub-community_skills-3DDC84?style=flat-square&logo=android&logoColor=white)](https://rohit-554.github.io/Android-KMP-Skills-Hub/)

The **[Android KMP Skills Hub](https://rohit-554.github.io/Android-KMP-Skills-Hub/)** (this project!) is a curated directory of community-built `SKILL.md` repos covering:

| Category | Examples |
|---|---|
| Architecture | Clean Architecture, MVVM, Hilt, modularization |
| Compose / UI | Material 3, animations, theming |
| KMP / CMP | Kotlin Multiplatform, Compose Multiplatform |
| Discovery | Awesome-lists and skill indexes |

Browse → clone a repo → drop its skill folder into your `.skills/` directory → done.

---

> [![Status](https://img.shields.io/badge/Android_CLI-Preview_%7C_April_2026-orange?style=flat-square&logo=android&logoColor=white)]()
> Custom skills follow the open [agentskills.io](https://agentskills.io) standard — compatible with any AI agent that supports it.
