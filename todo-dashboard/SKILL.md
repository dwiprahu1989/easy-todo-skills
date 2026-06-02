---
name: todo-dashboard
description: Visual HTML Kanban-style dashboard for TODO.md tracking. Reads project TODO files and renders interactive board view with status, progress, and phase grouping. Pairs with easy-todo-skills.
version: 1.0.0
metadata:
  openclaw:
    emoji: "📊"
    homepage: https://github.com/dwiprahu1989/easy-todo-skills
---

# TODO Dashboard — Visual Kanban Board

Renders an interactive HTML dashboard from project `TODO.md` and `TODO_PROGRESS.md` files.

## Trigger
- "buka dashboard TODO", "tampilkan TODO board", "show todo dashboard"
- "lihat progress project", "task board", "kanban board"
- After running todo-generator or todo-executor
- User wants visual overview of project tasks

## How It Works

### Step 1: Locate TODO Files
```
1. Identify active project directory
2. Read TODO.md (required — the main task list)
3. Read TODO_PROGRESS.md (optional — execution status)
4. Read any supporting context (git status, recent changes)
```

### Step 2: Parse TODO.md
Extract structure:
- Project name (from H1 heading)
- Phases (from H2 headings)
- Tasks (from `- [ ]` / `- [x]` checkboxes)
- Achievements (from `→ achievement:` lines)
- File targets (from `→ file:` / `→ files:` lines)
- Details (from `→ detail:` / indented lines)

### Step 3: Generate Dashboard HTML

Create a self-contained HTML file at `<project>/dashboard.html` with:

**Layout:**
```
┌─────────────────────────────────────────────────┐
│  📊 TODO Dashboard — [Project Name]              │
│  Generated: [date] | Auto-refresh: ON            │
├─────────────────────────────────────────────────┤
│  Summary Bar                                     │
│  [Total: 15] [Done: 8] [In Progress: 3] [☐: 4] │
│  ██████████████░░░░░░░░ 53% Complete             │
├──────────┬──────────┬──────────┬────────────────┤
│ Phase 1  │ Phase 2  │ Phase 3  │ Phase 4        │
│ Setup    │ Core     │ Integrate│ Testing         │
│          │          │          │                 │
│ ✅ Task1 │ ✅ Task5 │ ☐ Task10│ ☐ Task13        │
│ ✅ Task2 │ ✅ Task6 │ ☐ Task11│ ☐ Task14        │
│ ✅ Task3 │ ⏳ Task7 │ ☐ Task12│ ☐ Task15        │
│ ✅ Task4 │ ⏳ Task8 │         │                  │
│          │ ☐ Task9  │         │                  │
│ 4/4 ✅   │ 2/5 🔄   │ 0/3 ☐   │ 0/3 ☐          │
└──────────┴──────────┴──────────┴────────────────┘
│  Recent Activity                                 │
│  ✅ Task 4 — Create User model (14:32)           │
│  ⏳ Task 7 — Add API endpoints (14:35)           │
└─────────────────────────────────────────────────┘
```

**Features:**
- **Phase columns** — each phase is a Kanban column
- **Task cards** — show name, status, achievement, target files
- **Progress bar** — overall completion percentage
- **Status badges:**
  - ✅ Done (green) — `[x]` in TODO.md
  - ⏳ In Progress (yellow) — marked in TODO_PROGRESS.md
  - ❌ Failed (red) — marked in TODO_PROGRESS.md
  - ☐ Pending (gray) — `[ ]` in TODO.md
- **Click to expand** — shows achievement, files, details
- **Auto-refresh** — re-reads TODO.md every 5 seconds during execution
- **Dark theme** — consistent with EasyClaw/EA Lab style
- **Responsive** — works on desktop and mobile

### Step 4: Present Dashboard
Serve via Canvas:
```javascript
canvas.present("dashboard.html", { target: projectDir + "/dashboard.html" })
```

Or open in browser:
```bash
start dashboard.html
```

## HTML Template Structure

The generated HTML must be a **single self-contained file** with embedded CSS + JS:

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>TODO Dashboard — {project}</title>
  <style>
    /* Dark theme, Kanban layout, responsive grid */
    /* Card styles with status colors */
    /* Progress bar animation */
  </style>
</head>
<body>
  <!-- Header with project name + stats -->
  <!-- Summary bar with progress -->
  <!-- Kanban columns (one per phase) -->
  <!-- Task cards inside columns -->
  <script>
    // Auto-refresh: re-read TODO.md via fetch every 5s
    // Click handlers for expand/collapse
    // Progress calculation
  </script>
</body>
</html>
```

## Integration with Other Skills

### With todo-generator
After TODO.md is generated → auto-trigger dashboard:
```
1. todo-generator creates TODO.md
2. Ask "mau lihat dashboard?"
3. Generate dashboard.html
4. Present via Canvas or open in browser
```

### With todo-executor
During execution → dashboard auto-updates:
```
1. todo-executor marks tasks [x] in TODO.md
2. Dashboard re-reads TODO.md every 5s
3. Progress bar updates in real-time
4. Cards change from ☐ → ⏳ → ✅
```

### With plan-review
After plan review approved:
```
1. Plan approved → generate TODO.md
2. Auto-generate dashboard
3. Show dashboard for final visual check
4. Then execute
```

## Refresh Behavior

| Mode | Trigger | Behavior |
|------|---------|----------|
| **Static** | User opens file | Read TODO.md once, render |
| **Live** | During todo-executor | Auto-refresh every 5 seconds |
| **Manual** | User clicks refresh | Re-read and re-render |

Live mode is activated when:
- `TODO_PROGRESS.md` exists (executor is running)
- User explicitly says "live mode"

## Style Guide

### Colors
| Status | Background | Border | Text |
|--------|-----------|--------|------|
| ✅ Done | #064e3b | #34d399 | #6ee7b7 |
| ⏳ In Progress | #713f12 | #fbbf24 | #fde68a |
| ❌ Failed | #7f1d1d | #f87171 | #fca5a5 |
| ☐ Pending | #1f2937 | #374151 | #9ca3af |

### Layout
- Dark background (#0f172a)
- Kanban grid (auto-fit, min 280px per column)
- Cards with 8px border-radius, subtle shadow
- Monospace font for file paths
- System font for everything else

## What NOT to Do
- ❌ Generate dashboard without a TODO.md
- ❌ Hard-code task data — always read from TODO.md
- ❌ Require external dependencies (no CDN, no build step)
- ❌ Over-complicate — keep it readable and fast
- ❌ Store state in the HTML — TODO.md is the source of truth

## What TO Do
- ✅ Single self-contained HTML file
- ✅ Read from TODO.md dynamically
- ✅ Dark theme by default
- ✅ Responsive (mobile + desktop)
- ✅ Auto-refresh during execution
- ✅ Show phase progress + overall progress
- ✅ Click to expand task details
- ✅ Integrate with todo-generator and todo-executor
