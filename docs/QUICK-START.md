# Quick Start Guide - Ticket-Based Workflow

## 🚀 Getting Started in 3 Steps

### 1. Create a Ticket

```bash
# Choose template
ls docs/templates/

# Copy template
cp docs/templates/feature.md docs/tickets/feature-my-feature.md

# Edit ticket
vim docs/tickets/feature-my-feature.md
# Fill all sections, remove placeholders
```

### 2. Execute with AI

Tell your AI assistant:

```
"Thực hiện docs/tickets/feature-my-feature.md cho tôi"
```

Or with agent:

```
"Bạn là Planner Agent (đọc .agents/planner.md).
Thực hiện docs/tickets/feature-my-feature.md"
```

### 3. Review & Complete

```bash
# Review AI's work
# Run tests
# Make adjustments if needed

# Commit when satisfied
git add .
git commit -m "[feature-my-feature] Implement my feature"

# Archive completed ticket
mv docs/tickets/feature-my-feature.md docs/completed/2025-01/
```

## 📚 Key Documents

- **Policy**: `docs/PROJECT-POLICY.md` - Complete rules and guidelines
- **Templates Guide**: `docs/templates/README.md` - How to use templates
- **Agents Guide**: `.agents/README.md` - Using specialized agents

## 🎯 Daily Workflow

```bash
# Morning: Create tickets for today's work
cp docs/templates/task.md docs/tickets/task-implement-auth.md
vim docs/tickets/task-implement-auth.md

# Execute
"Thực hiện docs/tickets/task-implement-auth.md"

# Review & commit
git commit -m "[task-implement-auth] Add authentication"

# Archive
mv docs/tickets/task-implement-auth.md docs/completed/2025-01/
```

## ✅ Checklist

**Before Starting:**
- [ ] Ticket exists in `docs/tickets/`
- [ ] All sections filled completely
- [ ] Requirements are clear
- [ ] Status is `open`

**Before Submitting for Review:**
- [ ] All requirements met
- [ ] Tests passing
- [ ] Code follows standards
- [ ] Documentation updated

**Before Marking Done:**
- [ ] User approved
- [ ] Committed with ticket reference
- [ ] Ticket moved to completed/

## 💡 Tips

✅ **Do:**
- Fill tickets completely
- Be specific with requirements
- Include examples
- Reference documentation
- Ask when unclear

❌ **Don't:**
- Skip required sections
- Make assumptions
- Add scope without approval
- Skip tests
- Forget to archive completed tickets

## 🔗 Quick Links

- [Project Policy](PROJECT-POLICY.md)
- [Templates README](templates/README.md)
- [Agents README](../.agents/README.md)
- [Example Ticket](tickets/example-feature-dark-mode.md)

---

**Need Help?** Read the full policy document or ask your AI assistant to explain specific sections.
