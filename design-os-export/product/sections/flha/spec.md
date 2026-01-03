# Section: FLHA (Field Level Hazard Assessment)

**ID:** `flha`

## Overview
The FLHA module handles the daily field-level hazard assessment workflow. Supervisors create FLHAs identifying job hazards and controls, workers review and acknowledge them before starting work.

## Screens

### FLHA List (`/flha`)

#### Purpose
Display today's FLHAs and their acknowledgment status for the current user.

#### Layout
- Page header with title, subtitle, and action buttons
- List of FLHA cards or empty state

#### Components

**Page Header**
- Title: "Today's FLHAs"
- Subtitle: "Review and acknowledge field hazard assessments"
- Actions:
  - Refresh button (icon only)
  - "New FLHA" button (supervisors only)

**FLHA List**
Cards for each FLHA showing:
- FLHA name/title
- "Acknowledged" badge if current user has acknowledged
- Created by: {supervisor name}
- Job description (truncated to 2 lines)

**Empty State**
- Icon: Clipboard
- Title: "No FLHAs Today"
- Description varies by role:
  - Supervisor: "Create an FLHA to get started..."
  - Worker: "No field level hazard assessments have been created..."
- "Create FLHA" button (supervisors only)

#### Card States
- **Default**: White background, slate border
- **Acknowledged**: Green border, light green tint background

---

### FLHA Detail (`/flha/:id`)

#### Purpose
View full FLHA details and acknowledge (workers) or manage (supervisors).

#### Layout
- Back navigation
- FLHA header with title and status
- Job details section
- Hazards section
- Controls section
- PPE requirements
- Acknowledgment section

#### Components

**FLHA Header**
- Name/title
- Date
- Supervisor name
- Status badge

**Job Details Card**
- Location
- Job description (full text)
- Special considerations

**Hazards List**
For each identified hazard:
- Hazard description
- Severity badge (Low/Medium/High/Critical)
- Category label

**Control Measures**
- Free text field with control measures

**PPE Requirements**
- List of required PPE items as badges/chips

**Acknowledgment Section**

*For workers who haven't acknowledged:*
- "I confirm I have read and understand..." checkbox
- "Acknowledge" button

*For workers who have acknowledged:*
- "You acknowledged this FLHA at {time}" message
- Green check icon

*For supervisors:*
- "Acknowledgments" expandable section
- List of employees who acknowledged with timestamps

---

### FLHA Create (`/flha/create`)

**Access:** Supervisors only

#### Purpose
Create a new FLHA for the current day.

#### Layout
- Multi-step form or single scrolling form
- Header with cancel/save actions

#### Form Sections

1. **Basic Information**
   - FLHA Name (required)
   - Location (required)
   - Job Description (textarea, required)

2. **Hazards**
   - Add hazard button
   - For each hazard:
     - Description (required)
     - Severity (dropdown)
     - Category (dropdown)
   - Remove hazard button

3. **Controls**
   - Control Measures (textarea)
   - Special Considerations (textarea)

4. **PPE Requirements**
   - Multi-select or checkbox list
   - Common PPE: Hard Hat, Safety Glasses, Steel Toes, Hi-Vis, Gloves, Hearing Protection

5. **Submit**
   - Preview summary
   - "Create FLHA" button

## UI Patterns

### FLHA Card (List View)
```
┌─────────────────────────────────────────┐
│ FLHA Title                    [Acked✓]  │
│ Created by: Supervisor Name             │
│ Job description text that may           │
│ span multiple lines but truncates...    │
└─────────────────────────────────────────┘
```

### Hazard Item
```
┌─────────────────────────────────────────┐
│ ⚠ Hazard description here               │
│ [HIGH] [Chemical]                       │
└─────────────────────────────────────────┘
```

### PPE Chip
```
[👷 Hard Hat] [👓 Safety Glasses] [🥾 Steel Toes]
```

## Data Model

### FLHA
```typescript
interface FLHA {
  id: number;
  name: string;
  date: string; // ISO date
  supervisorId: number;
  supervisor: string;
  location: string;
  jobDescription: string;
  hazards: Hazard[];
  controlMeasures: string;
  ppeRequired: string[];
  specialConsiderations?: string;
  status: 'Active' | 'Closed';
  createdAt: string;
}

interface Hazard {
  id: number;
  description: string;
  severity: 'Low' | 'Medium' | 'High' | 'Critical';
  category: string;
}

interface FLHAAcknowledgment {
  id: number;
  flhaId: number;
  employeeId: number;
  employeeName: string;
  acknowledgedAt: string;
}
```

## API Requirements

### Endpoints
- `GET /flhas/today` - Get today's FLHAs
- `GET /flhas/:id` - Get FLHA details
- `POST /flhas` - Create new FLHA
- `GET /flhas/:id/acknowledgments` - Get acknowledgments
- `POST /flhas/:id/acknowledge` - Submit acknowledgment
- `GET /user/:id/acknowledged-flhas` - Get user's acknowledged FLHA IDs

## States

### Loading
- List: Show loading message in card body
- Detail: Show skeleton or loading spinner

### Empty States
- No FLHAs: Clipboard icon with message
- No acknowledgments yet (supervisor view)

### Error States
- Failed to load: Show error with retry option
- Failed to submit: Show toast error, keep form state

### Offline
- List: Show cached FLHAs (may be stale)
- Create: Queue submission, show pending badge
- Acknowledge: Queue submission, show optimistic UI
