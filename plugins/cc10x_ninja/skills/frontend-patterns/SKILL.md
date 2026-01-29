---
name: frontend-patterns
description: "Internal skill. Use cc10x-router for all development tasks."
allowed-tools: Read, Grep, Glob
---

# Frontend Patterns — UX Planning Methodology

## Overview

User interfaces exist to help users accomplish tasks. Every UI decision should make the user's task easier or the interface more accessible.

**Core principle:** Design for user success, not aesthetic preference.

**Violating the letter of this process is violating the spirit of frontend design.**

## The Iron Law

```
NO UI CODE BEFORE UI INVENTORY IS COMPLETE
```

If you haven't catalogued every element the user will see and interact with, you cannot build UI.

---

## UI Inventory Framework (MANDATORY)

Before writing ANY frontend code, produce a UI Inventory:

### Template

```markdown
## UI Inventory: [Feature Name]

### Pages/Views
- [ ] What new pages/views does this feature need?
- [ ] Which existing pages are modified?

### Actions & Entry Points
- [ ] Primary action button (where? what label? what icon?)
- [ ] Secondary actions (edit, delete, archive, export)
- [ ] How does the user GET to this feature? (nav item? button? link?)

### Data Display
- [ ] Tables needed? (columns, sorting, filtering, pagination)
- [ ] Cards/lists? (what data on each card?)
- [ ] Charts/visualizations?
- [ ] Stats/KPI widgets?
- [ ] Empty states for each collection

### Forms & Input
- [ ] What forms are needed? (create, edit, filter, search)
- [ ] Fields per form (type, validation, required?)
- [ ] Dropdowns/selects (what options? searchable?)
- [ ] Date/time pickers?
- [ ] File uploads?
- [ ] Multi-step flows?

### Modals & Dialogs
- [ ] Confirmation dialogs (delete, discard changes)
- [ ] Detail modals (view/edit)
- [ ] Creation wizards

### Navigation Changes
- [ ] New sidebar items?
- [ ] New tabs within a page?
- [ ] Breadcrumb updates?

### Interaction State Matrix
For EACH interactive element, define:
| Element | Default | Hover | Active | Focus | Disabled | Loading | Error | Success |
|---------|---------|-------|--------|-------|----------|---------|-------|---------|
```

---

## User Flow First

**Before any UI work, map the flow:**

```
User Flow: [Feature Name]
1. User navigates to [page/section]
2. User sees [initial state]
3. User clicks [action]
4. System responds [feedback]
5. Success: [outcome]
6. Error: [recovery path]
```

**For each step, identify:**
- What user sees
- What user does
- What feedback they get
- What can go wrong

## Universal Questions (Answer First)

**ALWAYS answer before designing/reviewing:**

1. **What is the user trying to accomplish?** - Specific task, not feature
2. **What are the steps?** - Click by click
3. **What can go wrong?** - Every error state
4. **Who might struggle?** - Accessibility needs
5. **What's the existing pattern?** - Project conventions

---

## Interaction Design Patterns

### Hover Behaviors
- Reveal hidden actions (edit, delete buttons on table rows)
- Color shift on interactive elements (links, buttons, cards)
- Scale transform on cards (scale-[1.01] with shadow elevation)
- Tooltip display after 200ms delay

### Click Feedback
- Button press: translateY(1px) on active
- Loading transition: text changes to "Processing..." + spinner
- Optimistic updates: show result immediately, reconcile on response
- Haptic-like feedback: brief scale down then up

### Keyboard Navigation Flow
- Tab order follows visual reading order (top-left to bottom-right)
- Arrow keys navigate within composite widgets (menus, tabs, grids)
- Escape closes modals/dropdowns (focus returns to trigger)
- Enter/Space activates focused element
- Skip-to-content link as first focusable element

### Transition Choreography
- Stagger list items: each +50ms delay
- Parent animates before children
- Exit animations: reverse of entry, 60% duration
- Shared element transitions between views (hero pattern)

---

## Layout Composition

### Page Layout Patterns
- **Sidebar + Main**: Fixed sidebar (w-64) + scrollable main content
- **Header + Content + Footer**: Sticky header, flex-1 content, footer
- **Dashboard**: Stats row -> primary content -> secondary panels
- **Detail View**: Header with actions -> tabbed content sections

### Grid Systems
- Responsive cols: grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4
- Consistent gap: gap-4 (sm), gap-6 (lg)
- Content max-width: max-w-7xl mx-auto for readability

### Content Hierarchy
- Hero section: primary metric or action
- Stats row: 3-4 KPI cards
- Primary content: table, list, or main interaction area
- Secondary: filters, sidebar info, related content
- Footer: pagination, bulk actions, metadata

---

## Loading State Decision Tree (CRITICAL)

**Always handle states in this order:**

```typescript
// CORRECT order
if (error) return <ErrorState error={error} onRetry={refetch} />;
if (loading && !data) return <LoadingState />;
if (!data?.items.length) return <EmptyState />;
return <ItemList items={data.items} />;
```

**Loading State Decision Tree:**
```
Is there an error? -> Yes: Show error with retry
                   -> No: Continue
Is loading AND no data? -> Yes: Show loading indicator
                        -> No: Continue
Do we have data? -> Yes, with items: Show data
                 -> Yes, but empty: Show empty state
                 -> No: Show loading (fallback)
```

**Golden Rule:** Show loading indicator ONLY when there's no data to display.

### Skeleton vs Spinner

| Use Skeleton When | Use Spinner When |
|-------------------|------------------|
| Known content shape | Unknown content shape |
| List/card layouts | Modal actions |
| Initial page load | Button submissions |
| Content placeholders | Inline operations |

## Error Handling Hierarchy

| Level | Use For |
|-------|---------|
| **Inline error** | Field-level validation |
| **Toast notification** | Recoverable errors, user can retry |
| **Error banner** | Page-level errors, data still partially usable |
| **Full error screen** | Unrecoverable, needs user action |

## Success Criteria Framework

**Every UI must have explicit success criteria:**

1. **Task completion**: Can user complete their goal?
2. **Error recovery**: Can user recover from mistakes?
3. **Accessibility**: Can all users access it?
4. **Performance**: Does it feel responsive?

---

## Accessibility Checklist (WCAG 2.1 AA)

| Check | Criterion | How to Verify |
|-------|-----------|---------------|
| **Keyboard** | All interactive elements keyboard accessible | Tab through entire flow |
| **Focus visible** | Current focus clearly visible | Tab and check highlight |
| **Focus order** | Logical tab order | Tab matches visual order |
| **Labels** | All inputs have labels | Check `<label>` or `aria-label` |
| **Alt text** | Images have meaningful alt | Check `alt` attributes |
| **Color contrast** | 4.5:1 for text, 3:1 for large | Use contrast checker |
| **Color alone** | Info not conveyed by color only | Check without color |
| **Screen reader** | Content accessible via SR | Test with VoiceOver/NVDA |

**For each issue found:**
```markdown
- [WCAG 2.1 1.4.3] Color contrast at `component:line`
  - Current: 3.2:1 (fails AA)
  - Required: 4.5:1
  - Fix: Change text color to #333 (7.1:1)
```

---

## Responsive Design Checklist

| Breakpoint | Check |
|------------|-------|
| **Mobile (< 640px)** | Touch targets 44px+, no horizontal scroll |
| **Tablet (640-1024px)** | Layout adapts, navigation accessible |
| **Desktop (> 1024px)** | Content readable, not too wide |

---

## UX Review Checklist

| Check | Criteria | Example Issue |
|-------|----------|---------------|
| **Task completion** | Can user complete goal? | Button doesn't work |
| **Discoverability** | Can user find what they need? | Hidden navigation |
| **Feedback** | Does user know what's happening? | No loading state |
| **Error handling** | Can user recover from errors? | No error message |
| **Efficiency** | Can user complete task quickly? | Too many steps |

**Severity levels:**
- **BLOCKS**: User cannot complete task
- **IMPAIRS**: User can complete but with difficulty
- **MINOR**: Small friction, not blocking

---

## UI States Checklist (CRITICAL)

**Before completing ANY UI component:**

### States
- [ ] Error state handled and shown to user
- [ ] Loading state shown ONLY when no data exists
- [ ] Empty state provided for all collections/lists
- [ ] Success state with appropriate feedback

### Buttons & Mutations
- [ ] Buttons disabled during async operations
- [ ] Buttons show loading indicator
- [ ] Mutations have onError handler with user feedback
- [ ] No double-click possible on submit buttons

### Data Handling
- [ ] State order: Error -> Loading (no data) -> Empty -> Success
- [ ] All user actions have feedback (toast/visual)

---

## Red Flags - STOP and Reconsider

If you find yourself:

- Writing UI code before completing the UI Inventory
- Designing UI before mapping user flow
- Focusing on aesthetics before functionality
- Ignoring accessibility ("we'll add it later")
- Not handling error states
- Not providing loading feedback
- Using color alone to convey information
- Making decisions based on "it looks nice"

**STOP. Go back to the UI Inventory.**

## Rationalization Prevention

| Excuse | Reality |
|--------|---------|
| "Most users don't use keyboard" | Some users ONLY use keyboard. |
| "We'll add accessibility later" | Retrofitting is 10x harder. |
| "Error states are edge cases" | Errors happen. Handle them. |
| "Loading is fast, no need for state" | Network varies. Show state. |
| "It looks better without labels" | Unlabeled inputs are inaccessible. |
| "Users can figure it out" | If it's confusing, fix it. |
| "I'll do the inventory later" | No inventory = no code. |

---

## Output Format

```markdown
## Frontend Review: [Component/Feature]

### UI Inventory
[Complete inventory per template above]

### User Flow
[Step-by-step what user is trying to do]

### Success Criteria
- [ ] User can complete [task]
- [ ] User can recover from errors
- [ ] All users can access (keyboard, screen reader)
- [ ] Interface feels responsive

### UX Issues
| Severity | Issue | Location | Impact | Fix |
|----------|-------|----------|--------|-----|
| BLOCKS | [Issue] | `file:line` | [Impact] | [Fix] |

### Accessibility Issues
| WCAG | Issue | Location | Fix |
|------|-------|----------|-----|
| 1.4.3 | [Issue] | `file:line` | [Fix] |

### Recommendations
1. [Most critical fix]
2. [Second fix]
```

## Final Check

Before completing frontend work:

- [ ] UI Inventory completed for all elements
- [ ] User flow mapped and understood
- [ ] All interaction states defined (hover, active, focus, disabled, loading)
- [ ] All data states handled (loading, error, empty, success)
- [ ] Keyboard navigation works
- [ ] Screen reader tested
- [ ] Color contrast verified
- [ ] Touch targets adequate on mobile
- [ ] Error messages clear and actionable
- [ ] Dark/light mode variants planned
- [ ] Success criteria met
