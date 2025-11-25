# Design Guidelines: Project QuietZone - Urban Serenity Manager

## Design Approach: Material Design System
**Rationale**: Material Design provides excellent patterns for data-dense civic applications, robust data visualization support, and established grid systems perfect for mapping interfaces. Its emphasis on hierarchy and responsive grids suits our real-time monitoring dashboard needs.

## Typography System
- **Primary Font**: Roboto (Google Fonts)
- **Secondary Font**: Roboto Mono (for numeric data, metrics, coordinates)

**Hierarchy**:
- Page Titles: 2.5rem, weight-700, tight tracking
- Section Headers: 1.75rem, weight-600
- Card Titles: 1.25rem, weight-500
- Body Text: 1rem, weight-400, 1.6 line-height
- Data Labels: 0.875rem, weight-500, uppercase, wider tracking
- Numeric Values: 1.5rem, Roboto Mono, weight-600
- Timestamps: 0.8125rem, weight-400

## Layout & Spacing System
**Tailwind Units**: Consistent use of 4, 6, 8, 12, 16 for spacing
- Component padding: p-6 to p-8
- Section spacing: py-12 to py-16
- Card gaps: gap-6
- Grid spacing: gap-4 for dense data, gap-8 for feature cards
- Container max-width: max-w-7xl

**Grid Strategy**:
- Dashboard: 12-column responsive grid
- Metrics cards: 4-column on desktop (grid-cols-1 md:grid-cols-2 lg:grid-cols-4)
- Content areas: 8-column main + 4-column sidebar for live stats
- Mobile: Single column stack with preserved hierarchy

## Core Components

### Navigation
- Top horizontal nav bar with app logo, page links, and live status indicator
- Sticky positioning for persistent access
- Active page indication with underline accent
- Right-aligned: Real-time clock and "Last Updated: X seconds ago" indicator

### Dashboard Cards
- Elevated cards with subtle shadow (shadow-md)
- Rounded corners (rounded-lg)
- Consistent header pattern: Icon + Title + Metric badge
- Content area with 8-unit padding
- Card actions in footer when applicable

### Data Visualization Components

**Heatmap Grid (20x20)**:
- Square cells with consistent sizing
- Cell labels showing zone coordinates (e.g., "14B")
- Tooltip on hover showing: Zone ID, Current dBA, Status, Timestamp
- Legend positioned top-right showing: Green (Compliant), Yellow (Near Limit), Red (Violation)
- Grid container with subtle border separators

**Metrics Display**:
- Large numeric value with unit label
- Trend indicator (↑↓→) showing change from previous reading
- Small sparkline graph showing last 10 readings
- Status badge (Active/Warning/Critical)

**Violation Table**:
- Striped rows for readability
- Columns: Zone ID, Time, dBA Level, Limit, Severity Badge, Predicted Source
- Sort functionality on headers
- Severity badges: Critical (>10dBA over), Warning (5-10dBA over), Minor (<5dBA over)

**Policy Recommendations**:
- Card-based layout with icon representing policy type (traffic/industrial/timing)
- Priority indicator (High/Medium/Low)
- Recommendation text with implementation details
- "Deploy Policy" action button

### Forms & Inputs
- Floating labels for text inputs
- Consistent input height (h-12)
- Rounded borders (rounded-md)
- Focus states with subtle ring

### Buttons
- Primary: Bold, elevated with shadow
- Secondary: Outlined style
- Icon buttons: Circular with centered icon
- Consistent height: h-10 for standard, h-12 for prominent actions
- Background blur for buttons on images/heatmap

## Page Layouts

### Home/Dashboard Page
- Hero stats row: 4 metric cards (Total Violations Today, Average City Noise, Zones at Risk, Active Policies)
- Live heatmap preview (compressed 20x20 grid, click to expand)
- Recent violations list (last 10)
- Auto-refresh indicator pulsing every 10 seconds

### Heatmap Page
- Full-screen heatmap grid as primary focus
- Left sidebar: Zone filters (Residential/Commercial/Industrial toggles), time range selector
- Right panel: Selected zone details card showing historical trend graph
- Floating controls: Zoom, Legend toggle, Export CSV button

### Violations Page
- Filter bar: Zone type, severity level, time range
- Sortable table with pagination
- Export violations CSV button (top-right)
- Expandable rows showing detailed mitigation suggestions

### Policy Engine Page
- Two-column layout: Active Policies (left) | Recommended Actions (right)
- Each policy card includes: Icon, Policy name, Target zones, Expected impact (-X dBA), Status
- Timeline showing policy deployment history

### About Page
- Single column, max-w-4xl centered
- Hero section: Problem statement with city skyline illustration
- Features grid: 3 columns explaining system capabilities
- Technical approach section with diagram
- Contact/team information

## Icons
**Library**: Material Icons (via CDN)
- Consistent 24px size for inline icons
- 32px for card header icons
- 48px for feature section icons

**Icon Usage**:
- Noise level: volume_up
- Violations: warning
- Policies: gavel
- Traffic: traffic
- Industrial: factory
- Time: schedule
- Map: map
- Export: download
- Refresh: refresh

## Images
**No large hero images** - This is a data-focused application. Use:
- City skyline illustration (SVG) in About page header
- Icon-based graphics for feature explanations
- Diagram showing system architecture in About page
- All visual emphasis on real-time data visualization

## Responsive Behavior
- Desktop (1024px+): Full multi-column layouts, expanded heatmap
- Tablet (768px-1023px): 2-column grids, compressed sidebar
- Mobile (<768px): Single column, collapsible navigation drawer, simplified heatmap with zoom capability

## Animation Principles
**Minimal, Purposeful Only**:
- Auto-refresh pulse: Subtle 1s fade on data update
- Violation alerts: Gentle slide-in for new violations
- Loading states: Simple spinner, no skeleton screens
- No page transitions, no decorative animations

**Critical**: Focus animation budget on heatmap cell updates (smooth color transitions) and real-time data refresh indicators only.