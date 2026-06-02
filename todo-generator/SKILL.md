# TODO Generator — Auto-create TODO.md from PRD, Feature Requests, or Instructions

Automatically generates a structured `TODO.md` with achievements/expected outcomes
in the active project folder when asked to plan features, develop from PRD, or break down work.

## Trigger
- User says "buatkan TODO", "create todo list", "breakdown fitur X"
- User says "baca PRD ini dan buat todo", "plan this feature"
- User says "kembangkan fitur X", "develop Y"
- Any instruction to plan or break down work into tasks

## How It Works

### Step 1: Analyze Input
```
1. Read the PRD, feature description, or instruction from user
2. Understand the project context:
   - Read project structure (ls, key files)
   - Read existing code patterns
   - Read CONTEXT.md if exists
   - Identify tech stack, framework, conventions
3. Break down into logical phases and tasks
```

### Step 2: Generate TODO.md
Write `TODO.md` to the **project root folder** (not workspace root):

```markdown
# TODO — [Feature/Project Name]
> Generated: [date] | Project: [project path]
> Source: [PRD file / user instruction / feature description]

## 📋 Overview
[Brief description of what will be built]

## Phase 1: [Phase Name]
- [ ] Task description → achievement: expected outcome
- [ ] Task description → achievement: expected outcome
  - file: path/to/file.ext
  - detail: specific implementation notes

## Phase 2: [Phase Name]
- [ ] Task description → achievement: expected outcome
- [ ] Task description → achievement: expected outcome
  - file: path/to/file.ext

## Phase 3: [Phase Name] (Testing & Polish)
- [ ] Write unit tests → achievement: X tests passing
- [ ] Bug testing → achievement: 0 errors
- [ ] Git commit + push → achievement: pushed to remote

## 📊 Summary
- Total tasks: X
- Estimated files: Y
- Phases: Z
```

### Step 3: Ask to Execute
After generating TODO.md, ask user:
```
TODO.md sudah dibuat di [project path] dengan X tasks dalam Y phases.

Mau saya kerjakan sekarang? (kalau ya, saya jalankan todo-executor — tanpa konfirmasi sampai selesai)
```

## Achievement Format (MANDATORY)

Every task MUST have an achievement describing the verifiable outcome:

### Format Options

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

**Task with config:**
```
- [ ] Setup queue worker
  → achievement: php artisan queue:work processes jobs without error
  → files: config/queue.php, .env
```

## Phase Organization

### Standard Phases
| Phase | Content | Typical Tasks |
|-------|---------|---------------|
| **Phase 1: Setup** | Foundation, DB, models | Migrations, models, configs |
| **Phase 2: Core** | Main functionality | Controllers, services, APIs |
| **Phase 3: Integration** | Connect pieces | Routes, views, frontend |
| **Phase 4: Testing** | Quality assurance | Unit tests, feature tests |
| **Phase 5: Polish** | Refinement | Validation, error handling, docs |

### Adaptive Phases
Adjust phases based on project type:
- **Laravel**: Migration → Model → Controller → Route → View → Test
- **Frontend**: Component → State → API → UI → Test
- **API**: Schema → Endpoint → Service → Middleware → Test
- **Bug Fix**: Reproduce → Identify → Fix → Verify → Test

## Smart Breakdown Rules

### File Awareness
- Check existing files before planning — don't duplicate
- Follow existing patterns and conventions
- Match existing code style

### Dependency Order
- Tasks are ordered by dependency — first things first
- Each task builds on previous achievements
- No circular dependencies

### Granularity
- Each task should take 1-10 minutes to execute
- Break large tasks into smaller ones
- One task = one logical unit of work

### Achievement Verification
Every achievement must be **verifiable without asking user**:
- ✅ "file exists with correct content"
- ✅ "command runs without error"
- ✅ "X tests pass"
- ✅ "route returns expected JSON"
- ❌ "looks good" (subjective)
- ❌ "works correctly" (vague)

## Project Detection

When generating TODO, detect the project:
```
1. Check current working directory
2. Check last mentioned project path
3. Look for project markers: composer.json, package.json, .git, etc.
4. Read project CONTEXT.md if exists for domain knowledge
```

## Integration

### With todo-executor skill
After TODO.md is created, user can say "kerjakan" → triggers todo-executor:
- Reads the same TODO.md
- Executes each task
- Verifies each achievement
- No confirmation until done

### With plan-review skill
If 3+ files will be changed, show plan first (unless user says "langsung")

### Example Flow
```
User: "baca PRD ini dan kembangkan fitur order management"

Jarvis:
1. Reads PRD
2. Analyzes project structure
3. Generates TODO.md with 15 tasks in 4 phases
4. Shows summary
5. Asks "mau dikerjakan sekarang?"

User: "ya, kerjakan semua"

Jarvis (todo-executor):
1. Reads TODO.md
2. Executes all 15 tasks autonomously
3. Verifies each achievement
4. Bug tests
5. Reports results
```

## What NOT to Do
- ❌ Generate vague tasks ("improve code")
- ❌ Skip achievements ("implement feature" without expected outcome)
- ❌ Put TODO in wrong folder (always project root)
- ❌ Generate tasks without understanding project context
- ❌ Create tasks that depend on user decisions without noting them

## What TO Do
- ✅ Every task has a clear achievement
- ✅ Files are specified where possible
- ✅ Phases are ordered by dependency
- ✅ Adapt to project tech stack
- ✅ Read existing code for patterns
- ✅ Place TODO.md in the correct project folder
