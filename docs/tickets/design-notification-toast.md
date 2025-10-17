---
type: design
status: todo
priority: medium
created: 2025-01-20
updated: 2025-01-20
---

# Design: Notification Toast Component

## Context

We need a toast notification component to show feedback messages to users.

## Design Requirements

### User Experience

- [ ] Auto-dismiss after 3-5 seconds
- [ ] Manually closeable with X button
- [ ] Stack multiple toasts
- [ ] Don't block user interaction
- [ ] Smooth enter/exit animations

### Visual Design

- Style: Modern, clean, minimal
- Position: Top-right corner
- Width: 320px (mobile: full width minus padding)
- Types: Success, Error, Warning, Info
- Animations: Slide in from right, fade out

### Technical Constraints

- Framework: React + TypeScript
- Styling: Tailwind CSS
- Icons: Lucide React
- Responsive: Mobile-first

## Components Needed

1. **Toast Container**

   - Purpose: Manages toast stack
   - Position: Fixed top-right
   - Max toasts: 5 concurrent

2. **Toast Item**
   - Purpose: Individual notification
   - States: Entering, Visible, Exiting
   - Parts: Icon, Title, Message, Close button

## Design Specifications

**Colors:**

```
Success:
  - bg: #10B981 (green-500)
  - text: #FFFFFF

Error:
  - bg: #EF4444 (red-500)
  - text: #FFFFFF

Warning:
  - bg: #F59E0B (amber-500)
  - text: #000000

Info:
  - bg: #3B82F6 (blue-500)
  - text: #FFFFFF
```

**Typography:**

- Title: 14px, font-semibold
- Message: 13px, font-normal
- Line height: 1.5

**Spacing:**

- Padding: 16px
- Icon-Text gap: 12px
- Between toasts: 8px
- From viewport edge: 16px

**Animations:**

- Enter: Slide from right 200ms ease-out
- Exit: Fade out 150ms ease-in
- Auto-dismiss: 4000ms

## User Flow

```
1. Action triggers toast
   ↓
2. Toast slides in from right
   ↓
3. User reads message
   ↓
4. Auto-dismisses after 4s OR user clicks X
   ↓
5. Toast fades out
```

## Design References

- Inspiration: https://dribbble.com/shots/toast-notifications
- Similar: Sonner library (React)

## Implementation Notes

**Component API:**

```typescript
toast.success("Operation successful!");
toast.error("Something went wrong");
toast.warning("Please review your input");
toast.info("New version available");

// With options
toast.success("Saved!", {
  duration: 5000,
  dismissible: true,
});
```

**React Component Structure:**

```tsx
<ToastProvider>
  <ToastContainer />
  <App />
</ToastProvider>
```

## Deliverables

- [ ] Toast component code (TypeScript + Tailwind)
- [ ] Usage examples
- [ ] Storybook stories (if using)
- [ ] Animation implemented
- [ ] Responsive tested

## Success Criteria

- [ ] All 4 types render correctly
- [ ] Animations smooth
- [ ] Works on mobile
- [ ] Accessible (keyboard + screen reader)
- [ ] Easy to use API

## Notes

- Keep it simple - don't over-engineer
- Use Tailwind classes for styling
- Consider using existing toast library if complex
