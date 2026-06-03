# ✅ Easy TODO Skills — Autonomous Task Planning, Execution & Dashboard for AI Agents

> **Three complementary skills that turn any AI agent into an autonomous coding machine.**

`todo-generator` creates a structured, verifiable task plan. `todo-executor` runs it end-to-end with zero confirmations. `todo-dashboard` provides a live visual Kanban board to track progress in real-time.

---

## 🎯 Why This Exists

Most AI coding assistants stop and ask for permission after every step. That's fine for exploration, but terrible for execution. These skills solve that:

1. **todo-generator** — Turns a PRD, feature request, or idea into a detailed `TODO.md` with verifiable achievements
2. **todo-executor** — Reads that `TODO.md` and executes every task autonomously, self-verifying and self-correcting along the way
3. **todo-dashboard** — Renders a live-updating HTML Kanban board from `TODO.md`, showing progress as tasks complete

```
PRD / Feature Request / Idea
        │
        ▼
┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐
│  TODO Generator  │────▶│  TODO Executor   │────▶│  TODO Dashboard  │
│                  │     │                  │     │                  │
│ • Analyze code   │     │ • Execute tasks  │     │ • Visual board   │
│ • Break down     │     │ • Verify results │     │ • Live refresh   │
│ • Plan phases    │     │ • Fix errors     │     │ • Phase columns  │
│ • Define goals   │     │ • Run tests      │     │ • Progress bar   │
│                  │     │ • Git commit     │     │ • Dark theme     │
└──────────────────┘     └──────────────────┘     └──────────────────┘
        │                        │                        │
        ▼                        ▼                        ▼
     TODO.md              Final Report ✅          Kanban Board 📊
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
- 🛠️ **Adaptive phases** — adjusts phase structure based on project type (Laravel, Frontend, API, Bug Fix)

### `todo-executor/`
Reads `TODO.md` and executes all tasks autonomously. Zero confirmations. Zero pauses.

**Key features:**
- 🤖 **Fully autonomous** — no "should I proceed?" or "is this correct?" between tasks
- 🔁 **Self-correcting** — encounters an error? Reads it, fixes it, retries. Loops until correct.
- ✅ **Self-verifying** — checks each task's outcome against the defined achievement
- 🧪 **Bug testing** — runs project test suite after all tasks complete, fixes any bugs found
- 📊 **Progress tracking** — maintains `TODO_PROGRESS.md` with real-time status
- 📝 **Git integration** — commits logical groups of changes, pushes when done

### `todo-dashboard/`
Renders an interactive HTML Kanban board from project `TODO.md` and `TODO_PROGRESS.md` files.

**Key features:**
- 📊 **Visual Kanban board** — each phase becomes a column with task cards
- 🔴 **Live auto-refresh** — re-reads `TODO.md` every 5 seconds during execution for real-time updates
- 🌙 **Dark theme** — consistent with modern dev tooling aesthetics
- 📱 **Responsive** — works on desktop and mobile
- 🎨 **Status badges** — ✅ Done (green), ⏳ In Progress (yellow), ❌ Failed (red), ☐ Pending (gray)
- 📈 **Progress bar** — overall completion percentage with visual indicator
- 🔍 **Expandable cards** — click to show achievement details, target files, implementation notes
- 📦 **Single file** — self-contained HTML with embedded CSS + JS, no external dependencies

---

## 🚀 Quick Start

### Installation

Copy skill folders into your agent's skills directory:

```bash
# For EasyClaw
cp -r todo-generator ~/.easyclaw/workspace/skills/
cp -r todo-executor ~/.easyclaw/workspace/skills/
cp -r todo-dashboard ~/.easyclaw/workspace/skills/

# For OpenClaw
cp -r todo-generator ~/.openclaw/workspace/skills/
cp -r todo-executor ~/.openclaw/workspace/skills/
cp -r todo-dashboard ~/.openclaw/workspace/skills/
```

Or install the combined skill from ClawHub for a single-folder install.

### Usage

**Step 1: Generate TODO**
```
You: "Read this PRD and create a TODO for the order management feature"

Agent (todo-generator): 
  → Analyzes project structure
  → Reads existing code patterns
  → Generates TODO.md with phases, tasks, and achievements
  → Shows summary: "15 tasks in 4 phases. Execute now?"
```

**Step 2: Execute**
```
You: "Yes, execute all"

Agent (todo-executor):
  → Reads TODO.md
  → Executes task 1 → verifies ✅
  → Executes task 2 → verifies ✅
  → Executes task 3 → error → fixes → verifies ✅
  → ... (continues autonomously)
  → Runs tests → fixes bugs → all green
  → Git commit + push
  → Final report
```

**Step 3: Monitor (Optional)**
```
You: "Show me the TODO dashboard"

Agent (todo-dashboard):
  → Reads TODO.md + TODO_PROGRESS.md
  → Generates interactive Kanban board
  → Auto-refreshes every 5s during execution
  → Shows phase progress + overall completion
```

---

## 📄 TODO.md Format

The skills use a structured markdown format with **mandatory achievements**:

```markdown
# TODO — Feature Name
> Generated: 2026-06-03 | Project: /path/to/project
> Source: PRD / feature request / instruction

## Phase 1: Setup
- [ ] Create migration for orders table
  → achievement: php artisan migrate runs without error
  → file: database/migrations/2026_06_03_create_orders_table.php
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

### Achievement Rules

Achievements must be **objectively verifiable without human judgment**:

| ✅ Good Achievement | ❌ Bad Achievement |
|---|---|
| `file app/Models/User.php exists with fillable fields` | `looks good` |
| `POST /api/login returns {token, user} with 200` | `works correctly` |
| `8 tests pass — create, read, update, delete, validation` | `code is clean` |
| `php artisan migrate runs without error` | `database is ready` |

---

## ⚙️ Configuration

### todo-generator

| Setting | Default | Description |
|---------|---------|-------------|
| Phase structure | Adaptive | Adjusts phases based on tech stack |
| Task granularity | 1-10 min/task | Each task is a small, focused unit |
| Output location | Project root | `TODO.md` goes in the active project folder |
| Achievement format | Mandatory | Every task must have a verifiable outcome |

### todo-executor

| Rule | Behavior |
|------|----------|
| Confirmations | ❌ None — never ask between tasks |
| Pauses | ❌ None — never wait for user reply |
| Retry limit | ♾️ Unlimited — loop until correct |
| Error handling | Auto-fix, retry until correct |
| Progress tracking | `TODO_PROGRESS.md` updated in real-time |
| Git | Commit per logical group, push at end |

### todo-dashboard

| Setting | Default | Description |
|---------|---------|-------------|
| Theme | Dark | Dark background (#0f172a) |
| Refresh mode | Auto | Re-reads TODO.md every 5s during execution |
| Layout | Kanban grid | One column per phase, responsive |
| Output | Single HTML | Self-contained, no external deps |

---

## 🏗️ Architecture

```
easy-todo-skills/
├── README.md                     ← You are here
├── LICENSE                       ← MIT
├── easy-todo-skills/              ← Combined skill (ClawHub single-install)
│   └── SKILL.md
├── todo-generator/                ← Standalone generator skill
│   └── SKILL.md
├── todo-executor/                 ← Standalone executor skill
│   └── SKILL.md
└── todo-dashboard/                ← Visual Kanban dashboard
    ├── SKILL.md
    └── templates/
        └── dashboard.html         ← Reusable HTML template
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

## 📊 Example Flow

### 1. Generator creates:
```
TODO.md — 15 tasks in 4 phases
Phase 1: Database & Models (5 tasks)
Phase 2: API Endpoints (4 tasks)  
Phase 3: Frontend (3 tasks)
Phase 4: Testing & Polish (3 tasks)
```

### 2. Dashboard renders:
```
┌──────────┬──────────┬──────────┬──────────┐
│ Phase 1  │ Phase 2  │ Phase 3  │ Phase 4  │
│ Setup    │ Core     │ Integrate│ Testing  │
│          │          │          │          │
│ ✅ Task1 │ ✅ Task5 │ ☐ Task10│ ☐ Task13 │
│ ✅ Task2 │ ✅ Task6 │ ☐ Task11│ ☐ Task14 │
│ ✅ Task3 │ ⏳ Task7 │ ☐ Task12│ ☐ Task15 │
│ ✅ Task4 │ ⏳ Task8 │         │          │
│          │ ☐ Task9  │         │          │
│ 4/4 ✅   │ 2/5 🔄   │ 0/3 ☐   │ 0/3 ☐   │
└──────────┴──────────┴──────────┴──────────┘
  ████████████████░░░░░░░░░░░░ 53% Complete
```

### 3. Executor produces:
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

## 🔄 Integration with Other Skills

These skills pair well with:

| Skill | Integration |
|-------|-------------|
| **plan-review** | If 3+ files will change, show plan first before executing |
| **diagnose** | Use for systematic bug fixing during test phase |
| **tdd** | Combine with test-driven development workflow |
| **codegraph** | Re-initialize code graph after batch edits (3+ files) |

---

## 📜 License

MIT License — use it, fork it, improve it.

---

## 🙏 Credits

Built by [Dwi Prasetyo Hutomo](https://github.com/dwiprasetyo) with [Jarvis (EasyClaw)](https://easyclaw.ai).

If you find this useful, ⭐ the repo and share it!
