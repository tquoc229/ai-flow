---
type: feature
status: todo
priority: medium
created: 2025-01-20
updated: 2025-01-20
estimated_hours: 4
---

# Feature: Add Dark Mode Toggle

## Context

**Why do we need this feature?**
Users want to use the app at night without eye strain.

**Current Situation:**
App only has light theme.

**Desired State:**
User can toggle between light and dark modes, preference saved.

## User Story

As a user
I want to toggle between light and dark modes
So that I can use the app comfortably at night

**Example Scenario:**

```
Given I'm using the app
When I click the theme toggle in settings
Then the app switches to dark mode
And my preference is saved for next time
```

## Requirements

### Functional Requirements

- [ ] Toggle button in settings page
- [ ] Dark theme colors defined
- [ ] Theme preference persisted (localStorage)
- [ ] Smooth transition animation
- [ ] System theme detection (optional)

### Non-Functional Requirements

- [ ] Transition animation < 300ms
- [ ] All components support both themes
- [ ] Maintains WCAG AA contrast ratio

## Technical Approach

**Implementation:**

1. Add CSS variables for theming
2. Create theme context (React) or provider
3. Implement toggle component
4. Add localStorage persistence
5. Update all components to use theme variables

**Files to Modify:**

- `src/styles/theme.css` - Add dark mode variables
- `src/contexts/ThemeContext.tsx` - Create theme context
- `src/components/ThemeToggle.tsx` - Create toggle button
- `src/pages/Settings.tsx` - Add toggle to settings

## Testing Strategy

**Manual Testing:**

- [ ] Toggle works in settings
- [ ] Theme persists after refresh
- [ ] All pages render correctly in dark mode
- [ ] No color contrast issues

## Success Criteria

- [ ] Toggle button works
- [ ] Dark mode looks good on all pages
- [ ] Preference persists
- [ ] Smooth transition
- [ ] Code reviewed

## References

- Design inspiration: [link]
- Color palette: [link]

## Notes

- Use CSS variables for easy theme switching
- Consider auto-mode based on system preference later
