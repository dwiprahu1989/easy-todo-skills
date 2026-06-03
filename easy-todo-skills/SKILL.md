---
name: easy-todo-skills
description: Autonomous TODO planning, execution & visual dashboard for AI agents. Generate structured task plans from PRDs, execute them end-to-end with zero confirmations, and monitor progress via live Kanban board.
version: 1.1.0
metadata:
  openclaw:
    emoji: "✅"
    homepage: https://github.com/dwiprahu1989/easy-todo-skills
---

# Easy TODO Skills — Autonomous Task Planning, Execution & Dashboard

Three-in-one skill: **generate** a structured `TODO.md` from any input, **execute** it autonomously, and **monitor** progress via live Kanban board.

No hand-holding. No confirmations. Just results.

---

## Part 1: TODO Generator

### Trigger
- "buatkan TODO", "create todo list", "breakdown fitur X"
- "baca PRD ini dan buat todo", "plan this feature"
- "kembangkan fitur X", "develop Y"
- Any instruction to plan or break down work into tasks

### How It Works

#### Step 1: Analyze Input
```
1. Read the PRD, feature description, or instruction from user
2. Understand the project context:
   - Read project structure (ls, key files)
   - Read existing code patterns
   - Read CONTEXT.md if exists
   - Identify tech stack, framework, conventions
3. Break down into logical phases and tasks
```

#### Step 2: Generate TODO.md
Write `TODO.md` to the **project root folder** (not workspace root):

```markdown
# TODO — [Feature/Project Name]
> Generated: [date] | Project: [project path]
> Source: [PRD file / user instruction / feature description]

## Phase 1: [Phase Name]
- [ ] Task description → achievement: expected outcome
- [ ] Task description → achievement: expected outcome
  - file: path/to/file.ext
  - detail: specific implementation notes

## Phase 2: [Phase Name]
- [ ] Task description → achievement: expected outcome

## Phase 3: [Phase Name] (Testing & Polish)
- [ ] Write unit tests → achievement: X tests passing
- [ ] Bug testing → achievement: 0 errors
- [ ] Git commit + push → achievement: pushed to remote

## Summary
- Total tasks: X
- Estimated files: Y
- Phases: Z
```

#### Step 3: Ask to Execute
After generating TODO.md, ask user:
```
TODO.md sudah dibuat di [project path] dengan X tasks dalam Y phases.

Mau saya kerjakan sekarang? (kalau ya, execute semua — tanpa konfirmasi sampai selesai)
```

### Achievement Format (MANDATORY)

Every task MUST have an achievement describing the verifiable outcome:

**Simple task:**
```
- [ ] Create User model → achievement: file app/Models/User.php exists with fillable fields
```

**Task with file target:**
```
- [ ] Add login API endpoint
  → achievement: POST /api/login returns {token, user} with 200
  → files: routes/api.php, app/Http/Controllers/AuthController.php
```

**Task with DB change:**
```
- [ ] Create migration for orders table
  → achievement: php artisan migrate runs without error
  → file: database/migrations/2026_06_02_create_orders_table.php
  → columns: id, user_id, total, status, timestamps
```

**Task with test:**
```
- [ ] Write tests for order creation
  → achievement: 5 tests pass — create, validation, auth check, edge cases
  → file: tests/Feature/OrderTest.php
```

### Phase Organization

| Phase | Content | Typical Tasks |
|-------|---------|---------------|
| **Phase 1: Setup** | Foundation, DB, models | Migrations, models, configs |
| **Phase 2: Core** | Main functionality | Controllers, services, APIs |
| **Phase 3: Integration** | Connect pieces | Routes, views, frontend |
| **Phase 4: Testing** | Quality assurance | Unit tests, feature tests |
| **Phase 5: Polish** | Refinement | Validation, error handling, docs |

Adaptive phases by project type:
- **Laravel**: Migration → Model → Controller → Route → View → Test
- **Frontend**: Component → State → API → UI → Test
- **API**: Schema → Endpoint → Service → Middleware → Test
- **Bug Fix**: Reproduce → Identify → Fix → Verify → Test

### Smart Breakdown Rules

- **File Awareness**: Check existing files before planning — don't duplicate
- **Dependency Order**: Tasks ordered by dependency — first things first
- **Granularity**: Each task should take 1-10 minutes to execute
- **Achievement Verification**: Every achievement must be objectively verifiable
  - ✅ "file exists with correct content"
  - ✅ "command runs without error"
  - ✅ "X tests pass"
  - ❌ "looks good" (subjective)
  - ❌ "works correctly" (vague)

### Project Detection
```
1. Check current working directory
2. Check last mentioned project path
3. Look for project markers: composer.json, package.json, .git, etc.
4. Read project CONTEXT.md if exists
```

---

## Part 2: TODO Executor

### Trigger
- "kerjakan TODO", "execute todos", "jalankan semua", "do all tasks"
- "finish the TODOs"
- Any instruction to work through a TODO.md file

### How It Works

#### Step 1: Locate TODO.md
```
1. Identify the active project directory
2. Read TODO.md from the project root
3. Parse all task items
4. Create working copy: TODO_PROGRESS.md (track status)
```

#### Step 2: Execute Each Task
```
count_remaining = count of unchecked items ([ ])

WHILE count_remaining > 0:
  For each unchecked task ([ ] not [x]):
    1. Read task description + expected achievement
    2. Execute (write code, edit file, create file, etc.)
    3. Verify result against achievement
    4. If CORRECT:
       → Mark [x] in TODO.md
       → Mark ✅ in TODO_PROGRESS.md
       → Move to next unchecked item
    5. If WRONG:
       → Analyze error → Fix → Re-verify → Repeat until correct
    6. Re-count unchecked items
    7. If count_remaining == 0 → EXIT LOOP
    8. If no progress made in this pass → mark ❌ FAILED, continue

When count_remaining == 0 → proceed to Bug Testing
```

#### Step 3: Bug Testing
```
1. Run project tests (pytest, pest, phpunit, npm test, etc.)
2. Check for syntax errors, import errors
3. Verify no breaking changes
4. Fix any bugs found — autonomous, no confirmation
5. Re-test until clean
```

#### Step 4: Final Report
```
## ✅ TODO Execution Complete — [Project Name]

| # | Task | Status |
|---|------|--------|
| 1 | Create user model | ✅ Done |
| 2 | Add API endpoint | ✅ Done |
| 3 | Write tests | ✅ Done |

### Bug Testing
- Tests: 15/15 passed ✅
- Bugs found & fixed: 2

### Git
- Commits: X
- Files changed: Y
```

### Execution Rules (MANDATORY)

| Rule | Description |
|------|-------------|
| **NO confirmation** | Never ask "should I proceed?" between tasks |
| **NO pauses** | Never stop to wait for user reply |
| **NO questions** | Use best judgment, fix errors yourself |
| **NO retry limit** | Loop until correct — never give up |
| **Self-verify** | Check your own work after each task |
| **Self-correct** | Fix errors immediately, don't report and wait |
| **Keep going** | Even if one task fails, continue to next |
| **Report once** | Only report when ALL tasks + bug testing complete |
| **NEVER skip** | Only skip if truly impossible (missing dependency) |

### Git Rules
- Commit after each logical group of changes
- Push to remote after all tasks complete
- Format: `feat/fix/refactor: description (TODO #X)`

### Error Handling
```
Error encountered:
  1. Read error message
  2. Identify cause
  3. Fix code
  4. Re-run/re-verify
  5. If fixed → ✅ continue
  6. If not fixed → try different approach, loop again
  7. NEVER skip — keep trying until correct
  8. Only mark ❌ if truly impossible
```

### Progress Tracking
Update `TODO_PROGRESS.md` in the project during execution:

```markdown
# Progress — [timestamp]

## Phase 1: Setup
- [x] ✅ Create database migration — done
- [x] ✅ Create User model — done
- ⏳ Add CRUD API endpoints — in progress...
```

---

## What NOT to Do
- ❌ Generate vague tasks ("improve code")
- ❌ Skip achievements
- ❌ Ask "should I proceed?" between tasks
- ❌ Wait for approval between tasks
- ❌ Stop after partial completion
- ❌ Skip verification
- ❌ Report bugs and wait — FIX them

## What TO Do
- ✅ Every task has a clear achievement
- ✅ Files are specified where possible
- ✅ Phases ordered by dependency
- ✅ Adapt to project tech stack
- ✅ Execute everything end-to-end
- ✅ Self-verify and self-correct
- ✅ Run tests at the end
- ✅ Git commit + push
- ✅ Report ONCE when fully done

---

## Part 3: TODO Dashboard

### Trigger
- "buka dashboard TODO", "tampilkan TODO board", "show todo dashboard"
- "lihat progress project", "task board", "kanban board"
- After running todo-generator or todo-executor
- User wants visual overview of project tasks

### How It Works

#### Step 1: Locate TODO Files
```
1. Identify active project directory
2. Read TODO.md (required — the main task list)
3. Read TODO_PROGRESS.md (optional — execution status)
4. Read any supporting context (git status, recent changes)
```

#### Step 2: Parse TODO.md
Extract structure:
- Project name (from H1 heading)
- Phases (from H2 headings)
- Tasks (from `- [ ]` / `- [x]` checkboxes)
- Achievements (from `→ achievement:` lines)
- File targets (from `→ file:` / `→ files:` lines)
- Details (from `→ detail:` / indented lines)

#### Step 3: Generate Dashboard HTML
Create a self-contained HTML file at `<project>/dashboard.html` with:
- **Phase columns** — each phase is a Kanban column
- **Task cards** — show name, status, achievement, target files
- **Progress bar** — overall completion percentage
- **Status badges:** ✅ Done (green), ⏳ In Progress (yellow), ❌ Failed (red), ☐ Pending (gray)
- **Click to expand** — shows achievement, files, details
- **Auto-refresh** — re-reads TODO.md every 5 seconds during execution
- **Dark theme** — consistent with modern dev tooling
- **Responsive** — works on desktop and mobile

#### Step 4: Present Dashboard
Serve via Canvas or open in browser.

### Dashboard Features
| Feature | Description |
|---------|-------------|
| Kanban columns | One column per phase, auto-fit layout |
| Task cards | Name + status + expandable details |
| Progress bar | Overall completion % with visual bar |
| Live refresh | Auto re-read TODO.md every 5s during execution |
| Dark theme | #0f172a background, status-colored cards |
| Single file | Self-contained HTML, no external deps |

### Refresh Behavior
| Mode | Trigger | Behavior |
|------|---------|----------|
| **Static** | User opens file | Read TODO.md once, render |
| **Live** | During todo-executor | Auto-refresh every 5 seconds |
| **Manual** | User clicks refresh | Re-read and re-render |

### Style Colors
| Status | Background | Border | Text |
|--------|-----------|--------|------|
| ✅ Done | #064e3b | #34d399 | #6ee7b7 |
| ⏳ In Progress | #713f12 | #fbbf24 | #fde68a |
| ❌ Failed | #7f1d1d | #f87171 | #fca5a5 |
| ☐ Pending | #1f2937 | #374151 | #9ca3af |
