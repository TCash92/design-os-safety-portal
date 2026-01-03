# Application Shell Specification

## Shell Pattern
**Sidebar Navigation** with responsive bottom nav for mobile.

## Layout Structure

```
┌─────────────────────────────────────────────────────────┐
│ HEADER (dark, sticky)                           [User] │
│ ═══════════════════════════════ (orange accent bar) ═══│
├───────────┬─────────────────────────────────────────────┤
│           │                                             │
│  SIDEBAR  │           MAIN CONTENT                      │
│   NAV     │                                             │
│           │           (max-width: 1200px)               │
│           │           (centered)                        │
│           │                                             │
├───────────┴─────────────────────────────────────────────┤
│                    BOTTOM NAV (mobile only)             │
└─────────────────────────────────────────────────────────┘
```

## Header

### Structure
- **Height:** 56px
- **Background:** slate-900
- **Position:** sticky top
- **Border:** 3px solid orange-500 (bottom)

### Left Section
- Hamburger menu button (mobile only)
- App title: "SAFETY" (Rajdhani, 18px, bold, uppercase, tracking-wide)
- Version badge (e.g., "v0.5.9")

### Right Section
- User badge showing employee ID
  - Click once: shows "Tap again to sign out"
  - Click twice: logs out
  - Confirm state: red background with pulse animation
- Online/Offline status badge
  - Online: green background, pulsing dot
  - Offline: red background
  - Shows pending sync count if > 0

## Sidebar Navigation

### Dimensions
- **Width:** 240px (desktop)
- **Behavior:** 
  - Desktop: Always visible, fixed position
  - Mobile: Slide-in drawer from left
  - Overlay behind drawer (black/50)

### Structure
Navigation grouped into sections with headers:

**Main**
- My Work (home icon) → `/`
- My Day (clipboard icon) → `/my-day`
- Quick Report (alert icon) → `/report`
- Incident Report (warning triangle) → `/incident/report`

**Safety**
- FLHA (clipboard icon) → `/flha`
- Hazards (alert icon) → `/safety/hazards`
- Inspections (clipboard icon) → `/safety/inspections`
- Incidents (shield icon) → `/safety/incidents`
- Actions (check icon) → `/safety/actions`

**Resources**
- Knowledge Base (book icon) → `/kb`

**Supervisor** (role-based, hidden for workers)
- Review Queue (check icon) → `/supervisor/queue`

**Admin** (role-based, hidden for non-admins)
- Admin Panel (settings icon) → `/admin`

### Styling
- Section title: 11px, uppercase, slate-400, tracking-wide
- Nav item: 14px, semibold, slate-600
- Active state: slate-900 text, slate-50 background, orange-500 left border
- Hover: slate-50 background

## Bottom Navigation (Mobile)

### Visibility
- Shown on screens < 768px
- Fixed to bottom of viewport
- Safe area padding for notched devices

### Items (4 items max)
1. Home (home icon) → `/`
2. My Day (clipboard icon) → `/my-day`
3. Report (alert icon) → `/report`
4. Actions (check icon) → `/safety/actions`

### Styling
- Background: white
- Border: 1px solid slate-200 (top)
- Item: centered icon + label, slate-500 default
- Active: orange-500 color, orange highlight

## Main Content Area

### Dimensions
- **Max width:** 1200px
- **Margin:** auto (centered)
- **Padding:** 20px
- **Padding bottom:** 84px on mobile (space for bottom nav)

### Background Pattern
Subtle grid pattern:
- 24x24px grid lines
- slate-100 color
- 1px line width

## Page Header Pattern

Every page should include a consistent header:

```jsx
<div class="page-header">
  <h1 class="page-title">{title}</h1>
  <p class="page-subtitle">{subtitle}</p>
</div>
```

- Title: 24px, bold, Rajdhani
- Subtitle: 14px, slate-500
- Margin bottom: 24px

## Responsive Breakpoints

- **Mobile:** < 768px
  - Bottom nav visible
  - Sidebar hidden (drawer mode)
  - Main content full width
  
- **Desktop:** ≥ 768px
  - Bottom nav hidden
  - Sidebar always visible
  - Main content offset by nav width (240px)
