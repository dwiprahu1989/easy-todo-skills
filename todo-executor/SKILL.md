---
name: todo-executor
description: Autonomous TODO.md task runner for AI agents. Executes all tasks with zero confirmations, self-verifies, self-corrects, and runs bug tests.
version: 1.0.0
metadata:
  openclaw:
    emoji: "🤖"
    homepage: https://github.com/dwiprahu1989/easy-todo-skills
---

# TODO Executor — Autonomous Coding Task Runner

Reads `TODO.md` from the active project and executes all tasks autonomously.
No confirmation. No pauses. Only report when everything is done + tested.

## Trigger
- User says "kerjakan TODO", "execute todos", "jalankan semua", "do all tasks"
- User points to a project and says "finish the TODOs"
- Any instruction to work through a TODO.md file

## How It Works

### Step 1: Locate TODO.md
```
1. Identify the active project directory
2. Read TODO.md from the project root (or subfolder if specified)
3. Parse all task items
4. Create a working copy: TODO_PROGRESS.md (track status)
```

### Step 2: Execute Each Task
```
completed = 0
failed = 0

Read TODO.md → parse all items

count_remaining = count of unchecked items ([ ])

WHILE count_remaining > 0:
  For each unchecked task ([ ] not [x]):
    1. Read the task description + expected achievement
    2. Execute (write code, edit file, create file, etc.)
    3. Verify the result against the achievement:
       - File exists? Code runs? Output matches?
    4. If CORRECT:
       → Mark [x] in TODO.md
       → Mark ✅ in TODO_PROGRESS.md
       → completed++
       → Move to next unchecked item
    5. If WRONG:
       → Analyze error
       → Fix the code
       → Re-verify
       → Repeat until correct
    6. Re-count unchecked items
    7. If count_remaining == 0 → EXIT LOOP
    8. If no progress made in this pass → mark ❌ FAILED, continue

When count_remaining == 0 → proceed to Bug Testing
```

### Step 3: Bug Testing
After ALL tasks are done:
```
1. Run project tests (pytest, pest, phpunit, npm test, etc.)
2. Check for syntax errors, import errors
3. Verify no breaking changes
4. Fix any bugs found — still autonomous, no confirmation
5. Re-test until clean
```

### Step 4: Final Report
```
## ✅ TODO Execution Complete — [Project Name]

| # | Task | Status | Time |
|---|------|--------|------|
| 1 | Create user model | ✅ Done | 2m |
| 2 | Add API endpoint | ✅ Done | 3m |
| 3 | Write tests | ✅ Done | 4m |

### Bug Testing
- Tests: 15/15 passed ✅
- Bugs found & fixed: 2

### Git
- Commits: X
- Files changed: Y

Ready for review.
```

## Rules (MANDATORY — NO EXCEPTIONS)

### Execution Rules
| Rule | Description |
|------|-------------|
| **NO confirmation** | Never ask "should I proceed?" between tasks |
| **NO pauses** | Never stop to wait for user reply |
| **NO questions** | Use best judgment, fix errors yourself |
| **NO retry limit** | Loop until correct — no max retries, never give up |
| **Self-verify** | Check your own work after each task |
| **Self-correct** | Fix errors immediately, don't report and wait |
| **Keep going** | Even if one task fails, continue to next |
| **Report once** | Only report when ALL tasks + bug testing complete |
| **NEVER skip** | Only skip if truly impossible (missing service/dependency) |

### Git Rules
- Commit after each logical group of changes (not every file)
- Push to remote after all tasks complete
- Commit message format: `feat/fix/refactor: description (TODO #X)`

### Verification Rules
- **Code tasks**: Check syntax, imports, logic
- **File tasks**: Verify file exists with correct content
- **Config tasks**: Verify config is valid
- **DB tasks**: Verify migration runs cleanly
- **Test tasks**: Run and verify all pass

### Error Handling
```
Error encountered:
  1. Read the error message
  2. Identify the cause
  3. Fix the code
  4. Re-run/re-verify
  5. If fixed → ✅ continue
  6. If not fixed → try different approach, loop again
  7. NEVER skip a task — keep trying until correct
  8. Only mark ❌ if truly impossible (e.g. missing dependency, service down)
```

## TODO.md Format

The skill expects TODO.md in this format:

```markdown
# TODO — Project Name

## Phase 1: Setup
- [ ] Create database migration for users table
- [ ] Create User model with relationships
- [ ] Add CRUD API endpoints

## Phase 2: Features
- [ ] Implement search functionality
- [ ] Add pagination
- [ ] Write unit tests for all endpoints

## Phase 3: Polish
- [ ] Add input validation
- [ ] Error handling
- [ ] API documentation
```

Tasks can have expected outcomes:
```markdown
- [ ] Create User model → file: app/Models/User.php with fillable fields
- [ ] Add login API → POST /api/login returns token
```

## Progress Tracking
During execution, update `TODO_PROGRESS.md` in the project:

```markdown
# Progress — [timestamp]

## Phase 1: Setup
- [x] ✅ Create database migration — 2 files, tested
- [x] ✅ Create User model — done
- ⏳ Add CRUD API endpoints — in progress...
```

## What NOT to Do
- ❌ Ask "should I proceed to the next task?"
- ❌ Ask "is this correct?" after each task
- ❌ Wait for approval between tasks
- ❌ Stop after partial completion
- ❌ Skip verification
- ❌ Report bugs and wait for instruction — FIX them

## What TO Do
- ✅ Execute everything end-to-end
- ✅ Verify each task yourself
- ✅ Fix all errors yourself
- ✅ Run tests at the end
- ✅ Fix any bugs found
- ✅ Git commit + push
- ✅ Report ONCE when fully done

## Integration with Other Skills
- Works with `plan-review` skill if 3+ files change (but auto-approve)
- Works with `diagnose` skill for bug fixing during testing phase
- Works with `tdd` skill if TODO includes test-driven tasks
