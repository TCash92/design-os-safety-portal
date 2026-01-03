# Section: Home & Navigation

**ID:** `home-navigation`

## Overview
The home screen serves as the primary dashboard and entry point for all users. It displays a personalized greeting, quick action cards, pending items, and weekly statistics.

## Screens

### Home Dashboard (`/`)

#### Layout
- Page header with greeting and date
- Quick actions grid (2x2 on mobile, flexible on desktop)
- Pending sync banner (conditional)
- Open actions section
- Weekly stats cards

#### Components

**Page Header**
- Greeting based on time of day: "Good morning/afternoon/evening, {Name}"
- Current date in format: "Monday, January 6, 2025"

**Quick Actions Grid**
Cards linking to primary actions:
1. **FLHA** (primary style) → `/flha`
   - Icon: Clipboard
   - Description: "View and acknowledge FLHAs"
2. **Quick Report** → `/report`
   - Icon: Alert circle
   - Description: "Report hazard or near-miss"
3. **Incident Report** (warning style) → `/incident/report`
   - Icon: Warning triangle
   - Description: "Report injury, incident, or accident"
4. **Inspections** → `/safety/inspections`
   - Icon: Shield
   - Description: "Safety inspections and audits"
5. **My Actions** → `/safety/actions`
   - Icon: Check square
   - Description: "Corrective actions assigned to you"

**Pending Sync Banner** (conditional)
- Shows when pendingCount > 0
- Amber/warning styling
- Text: "{n} submission(s) pending sync"

**Open Actions Section**
- Header: "My Open Actions" with "View All" link
- Empty state if no actions
- List of up to 3 actions showing:
  - Priority badge (Critical/High/Medium/Low)
  - Status badge (Open/In Progress/Pending Verification)
  - Description text
  - Due date with overdue styling
  - Source indicator
- "+{n} more actions" link if > 3

**Weekly Stats**
Two stat cards side by side:
1. FLHAs Completed (green when > 0)
2. Hazards Reported (amber when > 0)

## UI Patterns

### Action Card
```
┌─────────────────────────────────┐
│ [Icon]  TITLE                   │
│         Description text        │
└─────────────────────────────────┘
```
- White background, 1px border
- Hover: slight shadow, accent border
- Primary variant: orange left border
- Warning variant: red left border

### Stat Card
```
┌─────────────────────┐
│       VALUE         │
│       Label         │
└─────────────────────┘
```
- Large number value (32px, bold)
- Small label (13px, muted)
- Success state: green left border
- Warning state: amber left border

### Action Item (list)
```
┌─────────────────────────────────────┐
│ [PRIORITY] [STATUS]                 │
│ Description text that may wrap to   │
│ multiple lines if needed            │
│ Due in 3 days • From FLHA           │
└─────────────────────────────────────┘
```
- Overdue items: red left border, red due text

## Data Requirements

### Inputs
- `currentUser` - Logged in user object
- `pendingCount` - Number of items in sync queue
- `openActions[]` - Actions assigned to current user
- `weeklyStats` - { flhas: number, hazards: number }

### API Calls
- `fetchUserWeeklyFLHACount(userId)`
- `fetchUserWeeklyHazardCount(userId)`
- `fetchOpenActionsForEmployee(userId)`

## States

### Loading
- Stat values show "-" while loading
- Actions section shows "Loading actions..."

### Empty States
- No open actions: Show success empty state with message "You're all caught up!"

### Error States
- API failures log to console, don't block UI
- Stats show 0 if fetch fails
