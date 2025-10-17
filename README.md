# Ticket-Based Workflow System

Simple, flexible workflow for human-AI collaboration.

## 🚀 Quick Start

### 1. Create Ticket
```bash
cp docs/templates/feature.md docs/tickets/feature-my-feature.md
vim docs/tickets/feature-my-feature.md
```

### 2. Execute
```
"Thực hiện docs/tickets/feature-my-feature.md cho tôi"
```

### 3. Complete
```bash
git add .
git commit -m "[feature-my-feature] description"
mv docs/tickets/feature-my-feature.md docs/completed/2025-01/
```

## 📁 Structure

```
.
├── docs/
│   ├── PROJECT-POLICY.md      # Complete policy
│   ├── QUICK-START.md         # Quick guide
│   ├── templates/             # Ticket templates
│   ├── tickets/               # Active tickets
│   └── completed/             # Archives
├── .agents/                   # Agent definitions
├── plans/                     # Planning outputs
└── test/                      # Test files
```

## 📚 Documentation

- **[Project Policy](docs/PROJECT-POLICY.md)** - Complete rules
- **[Quick Start](docs/QUICK-START.md)** - Get started fast
- **[Templates Guide](docs/templates/README.md)** - Template usage
- **[Agents Guide](.agents/README.md)** - Using agents

## 🎯 Key Features

✅ Simple workflow  
✅ GitHub Issues style  
✅ Works with ANY AI  
✅ Template-based  
✅ Agent system  
✅ Complete policy  

## 💡 Example

```bash
# Create
cp docs/templates/feature.md docs/tickets/feature-auth.md

# Fill
vim docs/tickets/feature-auth.md

# Execute
"Thực hiện docs/tickets/feature-auth.md"

# Done
git commit -m "[feature-auth] Add authentication"
mv docs/tickets/feature-auth.md docs/completed/2025-01/
```

## 🤖 With Agents

```
"Bạn là UI/UX Designer (đọc .agents/ui-ux-designer.md).
Thực hiện docs/tickets/design-dashboard.md"
```

## ✅ Works With

- Claude (CLI, Desktop, Web)
- Cline CLI
- Cursor
- Gemini CLI
- Any AI tool

---

**Version:** 2.0  
**Created:** 2025-01-20
