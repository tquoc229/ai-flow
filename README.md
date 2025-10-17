# Workflow Step-by-Step

## Workflow

┌─────────────────────────────────────────────────────────────────┐
│ SIMPLE TICKET WORKFLOW │
└─────────────────────────────────────────────────────────────────┘

Step 1: Copy Template Step 2: Fill Ticket
│ │
↓ ↓
docs/templates/ docs/tickets/
├─ feature.md ──copy──→ ├─ feature-graphql.md
├─ bug.md ──copy──→ ├─ bug-login-timeout.md
└─ task.md ──copy──→ └─ task-add-tests.md
│
↓
Step 3: Execute
│
"Thực hiện docs/tickets/feature-graphql.md cho tôi"
│
↓
AI implements
│
↓
Step 4: Review & Done

```

### Key Features
```

✅ Một câu lệnh: "Thực hiện docs/tickets/XXX.md cho tôi"
✅ Tickets trong docs/ - dễ tìm, dễ quản lý
✅ Templates sẵn - copy và fill thôi
✅ AI tự hiểu ticket structure
✅ Works with any AI tool
✅ Simple, fast, effective

```

---

## Cấu Trúc Thư Mục
```

project/
├── docs/
│ ├── templates/ # ⭐ Copy từ đây
│ │ ├── README.md # Hướng dẫn sử dụng templates
│ │ ├── feature.md # Template cho feature mới
│ │ ├── bug.md # Template cho bug fix
│ │ ├── task.md # Template cho task đơn giản
│ │ ├── refactor.md # Template cho refactoring
│ │ ├── design.md # Template cho UI/UX design
│ │ └── research.md # Template cho research
│ │
│ ├── tickets/ # ⭐ Tickets của bạn ở đây
│ │ ├── feature-graphql.md
│ │ ├── bug-login-timeout.md
│ │ ├── task-add-authentication.md
│ │ ├── design-notification-ui.md
│ │ └── ...
│ │
│ ├── completed/ # ⭐ Move tickets xong vào đây
│ │ └── 2025-01/
│ │ ├── feature-graphql.md
│ │ └── bug-login-timeout.md
│ │
│ ├── project-overview.md # Project context
│ ├── architecture.md # Architecture docs
│ └── code-standards.md # Code standards
│
├── .agents/ # Agent definitions (optional)
│ ├── README.md
│ └── [agent files]
│
└── src/ # Source code

## Daily Workflow

```bash
# ═══════════════════════════════════════════════════════════════
# Morning: Create Ticket for Today's Work
# ═══════════════════════════════════════════════════════════════

# Choose template
$ ls docs/templates/
feature.md  bug.md  task.md  design.md  research.md  refactor.md

# Copy template
$ cp docs/templates/feature.md docs/tickets/feature-notifications.md

# Fill in details
$ vim docs/tickets/feature-notifications.md
# ... edit with your requirements

# ═══════════════════════════════════════════════════════════════
# Execute: Simple Command
# ═══════════════════════════════════════════════════════════════

# Tell AI (any tool):
"Thực hiện docs/tickets/feature-notifications.md cho tôi"

# AI reads ticket and implements
# You review and adjust

# ═══════════════════════════════════════════════════════════════
# Afternoon: Another Task
# ═══════════════════════════════════════════════════════════════

$ cp docs/templates/task.md docs/tickets/task-add-authentication.md
$ vim docs/tickets/task-add-authentication.md

# Execute
"Thực hiện docs/tickets/task-add-authentication.md"

# ═══════════════════════════════════════════════════════════════
# End of Day: Move Completed
# ═══════════════════════════════════════════════════════════════

$ mkdir -p docs/completed/2025-01
$ mv docs/tickets/feature-notifications.md docs/completed/2025-01/
$ mv docs/tickets/task-add-authentication.md docs/completed/2025-01/

# ═══════════════════════════════════════════════════════════════
# Commit
# ═══════════════════════════════════════════════════════════════

$ git add .
$ git commit -m "feat: implement real-time notifications"
$ git push
```

## Weekly Workflow

```bash
# ═══════════════════════════════════════════════════════════════
# Monday: Plan Week
# ═══════════════════════════════════════════════════════════════

# Create tickets for the week
$ cp docs/templates/feature.md docs/tickets/feature-1.md
$ cp docs/templates/feature.md docs/tickets/feature-2.md
$ cp docs/templates/task.md docs/tickets/task-1.md
# ... edit each ticket

# ═══════════════════════════════════════════════════════════════
# Tuesday-Friday: Execute
# ═══════════════════════════════════════════════════════════════

# Each day, pick a ticket and execute
"Thực hiện docs/tickets/feature-1.md"
"Thực hiện docs/tickets/task-1.md"
# etc.

# ═══════════════════════════════════════════════════════════════
# Friday: Review Week
# ═══════════════════════════════════════════════════════════════

# Check what's done
$ ls docs/tickets/          # Still in progress
$ ls docs/completed/2025-01/ # Completed this month

# Weekly summary
$ cat > docs/weekly-summary.md << EOF
# Week of Jan 15-19, 2025

## Completed
- Feature: Real-time notifications
- Task: Add authentication
- Bug: Login timeout fix

## In Progress
- Feature: GraphQL API

## Next Week
- Complete GraphQL API
- Start payment integration
EOF
```
