# Guided Learning

![WIP](https://img.shields.io/badge/status-WIP-yellow)
![Skills.sh](https://img.shields.io/badge/skills.sh-guided--learning-black?logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0IiBmaWxsPSJub25lIiBzdHJva2U9IndoaXRlIiBzdHJva2Utd2lkdGg9IjIiPjxwYXRoIGQ9Ik0yIDNINU0yIDlINE0yIDE1SDNNMiAyMUg0Ii8+PC9zdmc+)
![Agent Skills](https://img.shields.io/badge/Agent_Skills-compatible-green)
![License: MIT](https://img.shields.io/badge/license-MIT-blue)

An interactive, turn-by-turn guided learning tutor skill for AI coding agents. Teaches any topic in small chunks with visual aids, builds a lesson plan, and checks understanding with edge-case questions — one turn at a time.

> **🚧 Work in Progress** — The core skill works, but more reference transcripts and refinements are coming.

## Install

```sh
npx skills add static-var/guided-learning
```

## What it does

When a user asks to learn something ("Teach me X", "Explain Y", "How does Z work?"), the agent enters a structured teaching loop:

1. **Builds a Lesson TODO** — 5–12 actionable items, updated every turn
2. **Teaches one chunk** — 2–3 short paragraphs, no walls of text
3. **Adds a visual** — table, mermaid diagram, or ASCII art (one per turn, only when helpful)
4. **Asks one question** — edge cases, predictions, comparisons to force thinking
5. **Stops and waits** — grades the user's answer next turn, then continues

The skill handles continuations (answers, "next", hints, branch picks) and resets cleanly on topic changes.

## Repo structure

```
README.md                ← you are here (not installed to agents)
LICENSE                  ← MIT (not installed to agents)
skills/
  guided-learning/
    SKILL.md             ← installed to agents
    references/
      style-examples.md
      transcripts/
        01-kotlin-coroutines.md
        02-kotlin-flow.md
```

Only the `skills/guided-learning/` directory is installed into agent skill directories. The root README and LICENSE stay on GitHub.

## Supported agents

Works with any agent that supports the [Agent Skills](https://agentskills.io/) format, including:

Claude Code, GitHub Copilot, Cursor, Windsurf, Codex, Cline, Roo Code, Gemini CLI, Trae, and [many more](https://github.com/vercel-labs/skills#supported-agents).

## License

MIT
