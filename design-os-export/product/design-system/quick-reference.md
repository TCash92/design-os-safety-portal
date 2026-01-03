# Design System Quick Reference

## Fonts

| Purpose | Font | Weight | Size | Transform |
|---------|------|--------|------|-----------|
| Page Title | Rajdhani | 700 | 24px | none |
| Section Title | Rajdhani | 700 | 18px | none |
| Header Title | Rajdhani | 700 | 18px | UPPERCASE |
| Nav Section | Rajdhani | 700 | 11px | UPPERCASE |
| Nav Item | Rajdhani | 600 | 14px | none |
| Button | Rajdhani | 700 | 14-16px | UPPERCASE |
| Badge | Rajdhani | 700 | 11px | UPPERCASE |
| Label | Rajdhani | 600 | 13px | UPPERCASE |
| Body | Rajdhani | 500 | 16px | none |
| Meta | Rajdhani | 500 | 13px | none |

## Colors

### Neutrals (Slate)
| Token | Hex | Usage |
|-------|-----|-------|
| slate-950 | #0a0c10 | Darkest |
| slate-900 | #111318 | Header bg, primary buttons |
| slate-800 | #1a1d24 | Button hover |
| slate-700 | #282d38 | Dark borders |
| slate-600 | #3d4451 | Secondary text |
| slate-500 | #5a6377 | Medium gray |
| slate-400 | #8b95a8 | Muted text |
| slate-300 | #b4bcc9 | Disabled |
| slate-200 | #d8dce4 | Borders |
| slate-100 | #eef0f4 | Grid lines |
| slate-50 | #f7f8fa | Background |

### Accent (Orange)
| Token | Hex | Usage |
|-------|-----|-------|
| accent | #f97316 | Primary accent |
| accent-bright | #fb923c | Hover states |
| accent-dim | #c2410c | Pressed states |

### Safety Colors
| Token | Hex | Tailwind | Usage |
|-------|-----|----------|-------|
| safety-green | #22c55e | green-500 | Success, online, low |
| safety-amber | #f59e0b | amber-500 | Warning, medium |
| safety-red | #ef4444 | red-500 | Error, critical, overdue |
| safety-blue | #3b82f6 | blue-500 | Info, links |

## Spacing

| Token | Value | Usage |
|-------|-------|-------|
| space-1 | 4px | Minimal gaps |
| space-2 | 8px | Icon gaps |
| space-3 | 12px | Card padding, nav gaps |
| space-4 | 16px | Section gaps |
| space-5 | 20px | Page padding |
| space-6 | 24px | Section spacing |
| space-8 | 32px | Large gaps |

## Sizing

| Token | Value | Usage |
|-------|-------|-------|
| touch-min | 48px | Min button/input height |
| touch-preferred | 52px | Preferred touch target |
| radius | 4px | Default border radius |
| radius-sm | 2px | Badge radius |
| radius-lg | 8px | Card radius |

## Layout

| Token | Value |
|-------|-------|
| header-height | 56px |
| nav-width | 240px |
| max-content-width | 1200px |
| bottom-nav-height | 64px |

## Badge Priority Classes

```
.badge-critical → bg-red-500, text-white
.badge-high → bg-orange-600, text-white  
.badge-medium → bg-amber-500, text-slate-900
.badge-low → bg-green-500, text-white
```

## Badge Status Classes

```
.badge-success → bg-green-500/10, text-green-500
.badge-warning → bg-amber-100, text-amber-800
.badge-danger → bg-red-500/10, text-red-500
.badge-info → bg-blue-500/10, text-blue-500
```

## Key Component Patterns

### Header
- Background: slate-900
- Bottom border: 3px solid orange-500
- Height: 56px
- Title: Rajdhani, 18px, bold, uppercase, white

### Primary Button
- Background: slate-900
- Top accent bar: 3px orange-500
- Text: Rajdhani, 14px, bold, uppercase, white
- Hover: slate-800

### Card
- Background: white
- Border: 1px slate-200
- Radius: 8px

### Form Input
- Background: white
- Border: 2px slate-200
- Border focus: slate-800
- Height: 48px min
- Font: Rajdhani, 16px, medium
