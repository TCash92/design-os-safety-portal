# Design OS Export: Safety & Operations Portal

This Design OS package captures the complete design system, architecture, and specifications for the **Safety & Operations Portal** - an industrial safety management application.

## Quick Start

Use these files to replicate the design in new projects:

### 1. Apply Design Tokens

Copy `product/design-system/tokens.css` into your project and import it:

```css
@import './tokens.css';
```

Or extract the CSS variables for Tailwind CSS configuration.

### 2. Set Up Typography

The design uses two Google Fonts:

```html
<link href="https://fonts.googleapis.com/css2?family=Rajdhani:wght@400;500;600;700&family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
```

- **Rajdhani** - Industrial display font for headings, buttons, badges, navigation
- **Inter** - Clean body text (used sparingly)

### 3. Implement the Shell

Reference `product/shell/spec.md` for the navigation and layout structure:

- Dark header with orange accent border
- Sidebar navigation (desktop)
- Bottom navigation (mobile)
- Responsive breakpoint at 768px

### 4. Follow Section Specs

Each section in `product/sections/` contains detailed specifications for screens, components, and data requirements.

## Package Structure

```
design-os-export/
├── README.md                          # This file
├── product/
│   ├── product-overview.md            # Vision, problems, features
│   ├── product-roadmap.md             # Sections breakdown
│   ├── design-system/
│   │   ├── colors.json                # Color palette definitions
│   │   ├── typography.json            # Font specifications
│   │   └── tokens.css                 # Ready-to-use CSS variables
│   ├── data-model/
│   │   └── data-model.md              # Entities and relationships
│   ├── shell/
│   │   └── spec.md                    # Navigation and layout
│   └── sections/
│       ├── home-navigation/
│       │   └── spec.md                # Home dashboard spec
│       └── flha/
│           └── spec.md                # FLHA module spec
```

## Design Principles

### Industrial Aesthetic
- High-contrast dark header with vibrant accent
- Bold, technical typography (Rajdhani)
- Subtle grid pattern background
- Clear status indicators with safety colors

### Mobile-First & Touch-Optimized
- 48px minimum touch targets
- Bottom navigation for primary actions
- Large form inputs for field use
- Glove-friendly interfaces

### Offline-First
- All data persists locally
- Background sync when online
- Clear sync status indicators
- Graceful degradation

### Safety-Focused Colors
- **Green** (#22c55e) - Success, online, low priority
- **Amber** (#f59e0b) - Warning, medium priority
- **Red** (#ef4444) - Danger, critical, overdue
- **Blue** (#3b82f6) - Info, links, in-progress

## Tailwind CSS Configuration

If using Tailwind, extend with these colors:

```javascript
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      colors: {
        accent: {
          DEFAULT: '#f97316',
          bright: '#fb923c',
          dim: '#c2410c',
        },
        safety: {
          green: '#22c55e',
          amber: '#f59e0b',
          red: '#ef4444',
          blue: '#3b82f6',
        }
      },
      fontFamily: {
        industrial: ['Rajdhani', 'Inter', 'system-ui', 'sans-serif'],
      }
    }
  }
}
```

## Component Patterns

### Page Header
```jsx
<div className="mb-6">
  <h1 className="font-industrial text-2xl font-bold text-slate-900">
    Page Title
  </h1>
  <p className="text-sm text-slate-500">
    Page description
  </p>
</div>
```

### Card
```jsx
<div className="bg-white border border-slate-200 rounded-lg p-4">
  {/* Card content */}
</div>
```

### Badge
```jsx
<span className="inline-flex items-center gap-1 px-2 py-0.5 font-industrial text-xs font-bold uppercase tracking-wide rounded-sm bg-green-500/10 text-green-500">
  Success
</span>
```

### Primary Button
```jsx
<button className="relative inline-flex items-center justify-center gap-2 min-h-12 px-4 font-industrial text-sm font-bold uppercase tracking-wide text-white bg-slate-900 rounded overflow-hidden">
  <span className="absolute top-0 left-0 right-0 h-0.75 bg-orange-500" />
  Button Text
</button>
```

## License

This design system documentation is provided as-is for replication in other projects.
