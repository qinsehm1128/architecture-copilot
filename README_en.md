# Architecture Copilot 🧭

> A "co-design your architecture" skill for **Claude Code / Cursor / OpenAI Codex**.
> Before you write the first line of code, it walks you through your system's architecture with **relentless, structured questions** — not deciding for you, but making the trade-off behind every choice visible.

**English** · [简体中文](./README.md)

[![Skill for](https://img.shields.io/badge/skill%20for-Claude%20Code%20·%20Cursor%20·%20Codex-3c8772.svg)](#-three-tools-ready-to-use)
[![Method from](https://img.shields.io/badge/method%20from-awesome--architecture-blue.svg)](https://github.com/study8677/awesome-architecture)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](#-contributing)

---

## Why it exists

AI can already write most of your code. But there's one thing it won't do for you on its own — and it's exactly what makes or breaks a project:

> **Figure out what the system should look like, before you start.**

Most AI coding assistants default to "you say what you want, I start writing immediately." So you get rushed into implementation details before thinking through boundaries, scale, consistency, trade-offs — and by the time the architectural landmine goes off, it's months later and you're rewriting from scratch.

**Architecture Copilot does the opposite**: it turns the AI from "an intern itching to write code" into "a senior architect who thinks it through with you first." You describe what you want to build; it draws nothing and writes nothing — instead it asks **one question after another** until the architecture surfaces in the conversation itself.

---

## How it works

It follows a fixed guided flow (adapted from the "six soul questions" and "8-step design method" in [awesome-architecture](https://github.com/study8677/awesome-architecture)):

```
You: I want to build a multiplayer to-do list app.

Copilot: OK, forget tech for now. Which existing product is it most like?
         Personal use, or teams?
You: Teams, like a simplified Notion.

Copilot: Got it. Let's cut scope first — what does this version NOT do?
         (Comments? Permissions? Offline?)
You: Offline can wait, but permissions are needed.

Copilot: Then a few key ones: (1) Roughly how many people edit the same
         list at once? (2) If two people edit the same item simultaneously,
         who wins — or do you keep both?
You: Small teams, a few at once; simultaneous edits... can't lose data.

Copilot: That's the crux. "Can't lose data" means you don't want simple
         "last-write-wins" — you need CONFLICT MERGING, the hard part of
         collaborative docs. Two paths:
           A. OT: needs a central server to order ops; mature but complex.
           B. CRDT: built for offline/multi-device merge, but extra overhead.
         You said "no offline for now," so A's cost is more contained.
         Lean toward A?
You: Makes sense, A for now.

Copilot: ✅ Decision logged: use OT, give up CRDT's offline edge, because
         the MVP doesn't do offline. Next fork in the road…
```

By the end, it helps you converge on: **an ASCII architecture diagram, a data model, a list of ADRs (decision records), scaling-bottleneck warnings, an MVP→mature evolution path, and an honest list of risks.**

---

## The seven stages

| Stage | What it's doing |
|---|---|
| 0 · Open | One question: what do you want to build? Like which product? |
| 1 · Scope | Whom does it serve? **More importantly, what does this version NOT do** |
| 2 · Six soul questions | Scale / read-write ratio / consistency / growth / cost of failure / constraints |
| 3 · Back-of-envelope | Compute QPS & storage on the spot — what will crush this system? |
| 4 · Quality attributes | Performance/availability/consistency/cost… forcing you to **rank, because you can't have all** |
| 5 · Key decisions ⭐ | By system type, raise each fork: option A vs B, and the cost of each |
| 6 · Converge | Diagram + data model + ADRs + bottlenecks + evolution + risks |
| 7 · Challenge | Proactively: where will it die? what did you give up? |

> Iron rules throughout: **ask before answering, one dimension at a time, interrogate "why / at what cost" for every choice, never sink into syntax, cut scope relentlessly.**

---

## 🛠 Three tools, ready to use

| Tool | Form | Where it goes |
|---|---|---|
| **Claude Code** | [`skills/architecture-copilot/SKILL.md`](skills/architecture-copilot/SKILL.md) | `~/.claude/skills/` or project `.claude/skills/` |
| **Cursor** | [`.cursor/rules/architecture-copilot.mdc`](.cursor/rules/architecture-copilot.mdc) | your project's `.cursor/rules/` |
| **OpenAI Codex** | [`AGENTS.md`](AGENTS.md) | your project root `AGENTS.md` |
| **Any other AI** | paste `SKILL.md` body as the system prompt | —— |

👉 Full install steps in **[INSTALL.md](INSTALL.md)**.

---

## 🔗 Relationship with awesome-architecture

The two repos are a pair:

| | Repo | Role |
|---|---|---|
| 📚 | **[awesome-architecture](https://github.com/study8677/awesome-architecture)** | **Knowledge**: 26-chapter tutorial on architectural thinking + 25 templates / architecture maps (architecture only, no syntax). |
| 🧭 | **architecture-copilot** (this repo) | **Capability**: turns that knowledge into a skill that **actively guides you** inside Claude Code / Cursor / Codex. |

> One is the textbook; the other is the tutor that asks the questions. While guiding you, the Copilot cites the matching template and methodology from the former (building e-commerce? it pulls out the [e-commerce template](https://github.com/study8677/awesome-architecture/blob/main/templates/ecommerce-platform/README.md)'s key decisions to interrogate you).

---

## 🤝 Contributing

Add key-decision prompts for more system types, improve the guiding script, or translate. Edit [`skills/architecture-copilot/SKILL.md`](skills/architecture-copilot/SKILL.md) first (the source of truth), then sync `.cursor/rules/architecture-copilot.mdc` and `AGENTS.md`; if tutorial/template counts change, also sync `README.md`, `README_en.md`, and `INSTALL.md`.

---

> 📌 In one line: **AI made "writing code" cheap, and "figuring out what to write" priceless. This skill flips the AI into the "think it through with you first" mode.**
