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
[![Kimi](https://img.shields.io/badge/Kimi-compatible-000000?style=flat-square)]()
[![Qwen](https://img.shields.io/badge/Qwen-compatible-6554AF?style=flat-square)]()

Three components working together:
1. **Android CLI** — terminal-first tooling for setup, build, run, and inspect
2. **Android Skills** — modular `SKILL.md` instruction sets that teach agents Android best practices
3. **Android Knowledge Base** — up-to-date Android docs fetched at runtime

---

## Quickstart (Agentic Flow)

[![Download Android CLI](https://img.shields.io/badge/Download-Android_CLI-3DDC84?style=flat-square&logo=android&logoColor=white)](https://d.android.com/tools/agents)

**→ [d.android.com/tools/agents](https://d.android.com/tools/agents)**

```bash
android init                              # installs the android-cli skill into your agents globally

mkdir MyApp && cd MyApp                  # create & navigate to your project directory
android create --list                    # (optional) view all available project templates
android create --name="MyApp"            # scaffold a new project (or add a template like empty-activity-xml)

android skills list                      # (optional) see all official skills
android skills add --all                 # install all official Android skills (or add specific: --skill navigation-3)
```

Then launch your agent of choice inside the project folder:
```bash
claude     # Claude Code
gemini     # Gemini CLI
kimi       # Kimi
cursor .   # Cursor / any AI-native IDE
```

Then,
- **Prompt:** *"/android-cli Create a todu phodu app that blow everyone's mind"*

If you have to test the screen,
- **Test:** *"Test my app on the emulator"* — the agent spins up the emulator, installs the APK, and opens the app.

---


## Agentic Workflow

```
Your Prompt (with /skill-name)
    ↓
Agent reads Android Skills (SKILL.md)
    ↓
Android CLI (describe, build, emulator, run, layout)
    ↓
Android Knowledge Base (latest docs on demand)
    ↓
Android Studio (for polish, publish)
```

---

## Key Commands

| Command | What it does |
|---|---|
| `android init` | Set up agent environment globally (run once) |
| `android create --name=<app>` | Scaffold a new project |
| `android describe` | Analyze project structure for agent context |
| `android sdk install <pkg>` | Install SDK packages |
| `android sdk list` | List installed & available packages |
| `android emulator create` | Create a virtual device |
| `android emulator start <name>` | Launch a virtual device |
| `android run --apks=<path>` | Deploy APK to device/emulator |
| `android layout --pretty` | Get active UI as JSON (agent reads this) |
| `android screen capture --annotate` | Screenshot with labeled bounding boxes |
| `android skills list` | List available skills |
| `android skills add --skill=<name>` | Install a specific skill |
| `android skills find <keyword>` | Search skills by keyword |
| `android docs search <query>` | Search Android Knowledge Base |
| `android update` | Update CLI |

> `android emulator` is disabled on Windows.

---

## Invoking Skills

Most CLI agents **don't auto-pick up skills** — you need to explicitly tell the agent to use one:

```
/android-cli   → core Android CLI skill (setup, build, run)
/edge-to-edge  → edge-to-edge UI skill
/navigation-3  → Navigation 3 migration skill
```

In AI-native IDEs (Cursor, Android Studio with Gemini), you can also reference skills with:
```
@SKILL.md
```

---

## Why It Matters

[![Speed](https://img.shields.io/badge/Setup_Speed-3x_Faster-success?style=flat-square)]()
[![Tokens](https://img.shields.io/badge/LLM_Tokens-70%25_Reduced-informational?style=flat-square)]()

- **3x faster** project setup vs. agents using standard toolsets
- **70%+ fewer tokens** spent on boilerplate setup tasks
- Works with any agent — Claude, Gemini, Codex, Kimi, Qwen, Cursor, and more

---

## Community Skills

[![Skills Hub](https://img.shields.io/badge/Android_KMP_Skills_Hub-community_skills-3DDC84?style=flat-square&logo=android&logoColor=white)](https://rohit-554.github.io/Android-KMP-Skills-Hub/)

Not limited to official skills. Browse the **[Android KMP Skills Hub](https://rohit-554.github.io/Android-KMP-Skills-Hub/)** for community-built skills:

| Category | Examples |
|---|---|
| Architecture | Clean Architecture, MVVM, Hilt, modularization |
| Compose / UI | Material 3, animations, theming |
| KMP / CMP | Kotlin Multiplatform, Compose Multiplatform |

Clone a repo → drop its skill folder into your project's `.skills/` directory → done.

Skills follow the open **[agentskills.io](https://agentskills.io/specification)** standard — compatible with any AI agent that supports it.

---

> [![Status](https://img.shields.io/badge/Android_CLI-Preview_%7C_April_2026-orange?style=flat-square&logo=android&logoColor=white)]()

*Written with ❤️ by Jadu*
