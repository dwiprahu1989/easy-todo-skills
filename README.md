# ✅ Easy TODO Skills — Autonomous Task Planning & Execution for AI Agents

> **Two complementary skills that turn any AI agent into an autonomous coding machine.**

`todo-generator` creates a structured, verifiable task plan. `todo-executor` runs it end-to-end — no hand-holding, no confirmations, just results.

---

## 🎯 Why This Exists

Most AI coding assistants stop and ask for permission after every step. That's fine for exploration, but terrible for execution. These skills solve that:

1. **todo-generator** — Turns a PRD, feature request, or idea into a detailed `TODO.md` with verifiable achievements
2. **todo-executor** — Reads that `TODO.md` and executes every task autonomously, self-verifying and self-correcting along the way

```
PRD/Feature Request
        │
        ▼
┌──────────────────┐     ┌──────────────────┐
│  TODO Generator  │────▶│  TODO Executor   │
│                  │     │                  │
│ • Analyze code   │     │ • Execute tasks  │
│ • Break down     │     │ • Verify results │
│ • Plan phases    │     │ • Fix errors     │
│ • Define goals   │     │ • Run tests      │
│                  │     │ • Git commit     │
└──────────────────┘     └──────────────────┘
        │                        │
        ▼                        ▼
     TODO.md              Final Report ✅
```

---

## 📦 What's Included

### `todo-generator/`
Generates a structured `TODO.md` from any input (PRD, feature description, bug report, or plain text).

**Key features:**
- 🧠 **Context-aware** — reads existing project structure, code patterns, and tech stack before planning
- 📋 **Phase organization** — automatically groups tasks into logical phases (Setup → Core → Integration → Testing → Polish)
- 🎯 **Achievement-oriented** — every task has a verifiable expected outcome (file exists, tests pass, API returns expected JSON)
- 📁 **File-aware** — specifies target files for each task when possible
- 🔄 **Dependency-ordered** — tasks are sequenced so each builds on the previous

### `todo-executor/`
Reads `TODO.md` and executes all tasks autonomously. Zero confirmations. Zero pauses.

**Key features:**
- 🤖 **Fully autonomous** — no "should I proceed?" or "is this correct?" between tasks
- 🔁 **Self-correcting** — encounters an error? Reads it, fixes it, retries. Loops until correct.
- ✅ **Self-verifying** — checks each task's outcome against the defined achievement
- 🧪 **Bug testing** — runs project test suite after all tasks complete, fixes any bugs found
- 📊 **Progress tracking** — maintains `TODO_PROGRESS.md` with real-time status
- 📝 **Git integration** — commits logical groups of changes, pushes when done

---

## 🚀 Quick Start

### Installation

Copy both skill folders into your agent's skills directory:

```bash
# For EasyClaw
cp -r todo-generator ~/.easyclaw/workspace/skills/
cp -r todo-executor ~/.easyclaw/workspace/skills/

# For OpenClaw
cp -r todo-generator ~/.openclaw/workspace/skills/
cp -r todo-executor ~/.openclaw/workspace/skills/
```

### Usage

**Step 1: Generate TODO**
```
You: "Read this PRD and create a TODO for the order management feature"

Agent: 
  → Analyzes project structure
  → Reads existing code patterns
  → Generates TODO.md with phases, tasks, and achievements
  → Shows summary: "15 tasks in 4 phases. Execute now?"
```

**Step 2: Execute**
```
You: "Yes, execute all"

Agent:
  → Reads TODO.md
  → Executes task 1 → verifies ✅
  → Executes task 2 → verifies ✅
  → Executes task 3 → error → fixes → verifies ✅
  → ... (continues autonomously)
  → Runs tests → fixes bugs → all green
  → Git commit + push
  → Final report
```

---

## 📄 TODO.md Format

The skills use a structured markdown format:

```markdown
# TODO — Feature Name
> Generated: 2026-06-02 | Project: /path/to/project

## Phase 1: Setup
- [ ] Create migration for orders table
  → achievement: php artisan migrate runs without error
  → file: database/migrations/2026_06_02_create_orders_table.php
  → columns: id, user_id, total, status, timestamps

## Phase 2: Core
- [ ] Create OrderController with CRUD endpoints
  → achievement: GET/POST/PUT/DELETE /api/orders return correct responses
  → files: app/Http/Controllers/OrderController.php, routes/api.php

## Phase 3: Testing
- [ ] Write tests for order CRUD
  → achievement: 8 tests pass — create, read, update, delete, validation, auth
  → file: tests/Feature/OrderTest.php
```

Each task has:
- **Description** — what to do
- **Achievement** — how to verify it's done (mandatory, must be objectively verifiable)
- **Files** — target file paths (optional)
- **Details** — implementation specifics (optional)

---

## ⚙️ Configuration

### todo-generator

| Setting | Default | Description |
|---------|---------|-------------|
| Phase structure | Adaptive | Adjusts phases based on tech stack (Laravel, Frontend, API, etc.) |
| Task granularity | 1-10 min/task | Each task is a small, focused unit |
| Output location | Project root | `TODO.md` goes in the active project folder |

### todo-executor

| Rule | Behavior |
|------|----------|
| Confirmations | ❌ None |
| Pauses | ❌ None |
| Retry limit | ♾️ Unlimited |
| Error handling | Auto-fix, retry until correct |
| Progress tracking | `TODO_PROGRESS.md` updated in real-time |
| Git | Commit per logical group, push at end |

---

## 🏗️ Architecture

```
easy-todo-skills/
├── README.md                    ← You are here
├── LICENSE
├── todo-generator/
│   └── SKILL.md                 ← Generator skill definition
└── todo-executor/
    └── SKILL.md                 ← Executor skill definition
```

Each `SKILL.md` is a self-contained skill definition that tells the AI agent:
- **When** to activate (trigger words)
- **What** to do (step-by-step instructions)
- **How** to do it (rules, formats, examples)
- **What NOT** to do (anti-patterns)

---

## 🤝 Compatibility

Built for [EasyClaw](https://easyclaw.ai) / [OpenClaw](https://github.com/openclaw/openclaw) but works with any AI agent system that:
- Reads markdown skill files
- Can execute code and file operations
- Has access to the project filesystem

Tested with:
- ✅ EasyClaw (GLM-5.1, Claude, GPT)
- ✅ OpenClaw
- ✅ Any agent with tool access (file read/write, shell exec)

---

## 📊 Example Output

### Generator creates:
```
TODO.md — 15 tasks in 4 phases
Phase 1: Database & Models (5 tasks)
Phase 2: API Endpoints (4 tasks)  
Phase 3: Frontend (3 tasks)
Phase 4: Testing & Polish (3 tasks)
```

### Executor produces:
```markdown
## ✅ TODO Execution Complete — Order Management

| # | Task | Status | 
|---|------|--------|
| 1 | Create migration | ✅ Done |
| 2 | Create Order model | ✅ Done |
| 3 | Add CRUD endpoints | ✅ Done |
| 4 | Write tests | ✅ Done |

### Bug Testing
- Tests: 12/12 passed ✅
- Bugs found & fixed: 1 (missing import)

### Git
- Commits: 3
- Files changed: 7
```

---

## 📜 License

MIT License — use it, fork it, improve it.

---

## 🙏 Credits

Built by [Dwi Prasetyo Hutomo](https://github.com/dwiprasetyo) with [Jarvis (EasyClaw)](https://easyclaw.ai).

If you find this useful, ⭐ the repo and share it!
