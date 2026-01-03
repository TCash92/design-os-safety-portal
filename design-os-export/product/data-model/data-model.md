# Data Model

## Core Entities

### Employee
The users of the system - field workers, supervisors, and administrators.

| Field | Type | Description |
|-------|------|-------------|
| id | number | Unique identifier (from Baserow) |
| name | string | Full display name |
| employeeId | string | Company employee ID (e.g., "EMP001") |
| pin | string | 4-digit PIN for login |
| role | enum | "Worker" \| "Supervisor" \| "Admin" |
| email | string? | Optional email address |
| department | string? | Department/team name |
| active | boolean | Whether employee can log in |

**Relationships:**
- Creates many FLHAs (as supervisor)
- Has many FLHA Acknowledgments
- Has many assigned Actions
- Creates many Hazard Reports
- Creates many Incident Reports

---

### Asset
Equipment and vehicles that can be associated with work and hazards.

| Field | Type | Description |
|-------|------|-------------|
| id | number | Unique identifier |
| name | string | Asset name/description |
| assetNumber | string | Company asset ID |
| type | string | "Vehicle" \| "Equipment" \| "Tool" |
| location | string? | Current location |
| status | string | "Active" \| "Maintenance" \| "Retired" |

---

### FLHA (Field Level Hazard Assessment)
Daily hazard assessments created by supervisors and acknowledged by workers.

| Field | Type | Description |
|-------|------|-------------|
| id | number | Unique identifier |
| name | string | FLHA title/name |
| date | date | Date of assessment |
| supervisorId | number | FK to Employee who created |
| supervisor | string | Supervisor name (denormalized) |
| jobDescription | text | Description of work being done |
| location | string | Work location |
| hazards | Hazard[] | List of identified hazards |
| controlMeasures | text | Control measures in place |
| ppeRequired | string[] | Required PPE items |
| status | enum | "Active" \| "Closed" |
| createdAt | datetime | Creation timestamp |

**Relationships:**
- Belongs to Employee (supervisor)
- Has many FLHA Acknowledgments
- Has many Hazards

---

### FLHAAcknowledgment
Record of a worker acknowledging/reviewing an FLHA.

| Field | Type | Description |
|-------|------|-------------|
| id | number | Unique identifier |
| flhaId | number | FK to FLHA |
| employeeId | number | FK to Employee |
| acknowledgedAt | datetime | When acknowledged |
| signature | string? | Digital signature data |

**Relationships:**
- Belongs to FLHA
- Belongs to Employee

---

### Hazard
A reported hazard or near-miss.

| Field | Type | Description |
|-------|------|-------------|
| id | number | Unique identifier |
| type | enum | "Hazard" \| "Near Miss" |
| description | text | What was observed |
| location | string | Where it was found |
| severity | enum | "Low" \| "Medium" \| "High" \| "Critical" |
| category | string | Hazard category |
| reportedById | number | FK to Employee |
| reportedBy | string | Reporter name (denormalized) |
| reportedAt | datetime | When reported |
| status | enum | "Open" \| "Under Review" \| "Resolved" |
| photos | string[] | Attached photo URLs |
| flhaId | number? | FK to FLHA if from assessment |

**Relationships:**
- Belongs to Employee (reporter)
- Belongs to FLHA (optional)
- Has many Actions

---

### Incident
An incident, injury, or accident report.

| Field | Type | Description |
|-------|------|-------------|
| id | number | Unique identifier |
| type | enum | "Injury" \| "Property Damage" \| "Environmental" \| "Near Miss" |
| description | text | What happened |
| location | string | Where it occurred |
| occurredAt | datetime | When incident occurred |
| severity | enum | "Minor" \| "Moderate" \| "Serious" \| "Critical" |
| reportedById | number | FK to Employee |
| injuredPersons | Person[] | List of injured individuals |
| witnesses | Person[] | List of witnesses |
| immediateActions | text | Actions taken immediately |
| rootCause | text? | Root cause analysis |
| status | enum | "Open" \| "Under Investigation" \| "Closed" |
| photos | string[] | Attached photo URLs |
| createdAt | datetime | Report creation time |

**Relationships:**
- Belongs to Employee (reporter)
- Has many Actions

---

### Action
A corrective action assigned to address a hazard or incident.

| Field | Type | Description |
|-------|------|-------------|
| id | number | Unique identifier |
| name | string | Action title |
| description | text | What needs to be done |
| priority | enum | "Low" \| "Medium" \| "High" \| "Critical" |
| status | enum | "Open" \| "In Progress" \| "Pending Verification" \| "Closed" |
| assignedToId | number | FK to Employee |
| assignedTo | string | Assignee name (denormalized) |
| targetCompletionDate | date | Due date |
| completedAt | datetime? | When marked complete |
| verifiedById | number? | FK to Employee who verified |
| source | string | "FLHA" \| "Hazard" \| "Incident" \| "Inspection" |
| sourceId | number | FK to source record |
| notes | text? | Additional notes |
| createdAt | datetime | When created |

**Relationships:**
- Belongs to Employee (assignee)
- Belongs to Employee (verifier)
- Belongs to source entity (Hazard, Incident, etc.)

---

## Offline Storage Schema (IndexedDB/Dexie)

```javascript
// Local database tables
db.version(1).stores({
  // Reference data (synced periodically)
  refEmployees: 'id, employeeId, name, role',
  refAssets: 'id, assetNumber, name, type',
  referenceData: 'key',  // Sync metadata
  
  // Sync queue for offline submissions
  syncQueue: '++id, type, status, createdAt',
  
  // Local state
  currentUser: 'key'
});
```

## Sync Queue Entry

| Field | Type | Description |
|-------|------|-------------|
| id | number | Auto-increment ID |
| type | string | "flha" \| "hazard" \| "incident" \| "action" |
| action | string | "create" \| "update" |
| data | object | Full record data |
| status | enum | "pending" \| "syncing" \| "failed" |
| attempts | number | Retry count |
| createdAt | datetime | When queued |
| lastAttempt | datetime? | Last sync attempt |
| error | string? | Last error message |
