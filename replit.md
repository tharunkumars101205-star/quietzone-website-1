# Project QuietZone: The Urban Serenity Manager

**Last Updated**: November 24, 2025

## Overview
A real-time urban noise monitoring and management system designed for city administrators. The application simulates a 20×20 city grid, predicts noise levels using traffic flow and zoning data, detects violations against regulatory thresholds, and recommends automated mitigation policies.

## Recent Changes
### November 24, 2025 - Initial MVP Implementation
- Complete full-stack implementation with React frontend and Express backend
- Noise simulation engine with weighted regression model
- Interactive heatmap with tooltips showing zone details
- Violations page with sortable table and filtering
- Policy recommendation engine
- Auto-refresh every 10 seconds
- CSV export functionality for noise data and violations
- Comprehensive data-testid coverage for automated testing

## Project Architecture

### Frontend (React + TypeScript)
- **Pages**:
  - Home/Dashboard: Summary metrics, live heatmap preview, recent violations
  - Heatmap: Interactive 20×20 grid with zone filters and detailed sidebar
  - Violations: Sortable table with severity and zoning filters
  - Policy Engine: Active and recommended policies
  - About: Project explanation and technical approach

- **Components**:
  - Navigation: Sticky header with live status indicator
  - MetricCard: Reusable stat display with trend indicators
  - Material Design system with Roboto fonts
  
- **State Management**: TanStack Query (React Query v5) for server state
- **Routing**: Wouter for client-side routing
- **UI Components**: Shadcn/ui with Tailwind CSS

### Backend (Node.js + Express)
- **Storage**: In-memory storage (MemStorage class)
- **Simulation Engine**: 
  - Calculates noise based on zoning type, traffic flow, activity level, and time of day
  - Residential zones: No nighttime reduction (to enable <55 dBA violation detection after 10PM)
  - Commercial/Industrial: 0.7× multiplier at night, 1.3× during rush hours
  - ±5 dBA random variation for realistic fluctuations

- **API Endpoints**:
  - `GET /api/noise` - Current 20×20 grid state
  - `GET /api/violations` - All violations
  - `GET /api/violations/recent` - Last 10 violations
  - `GET /api/policy` - Policy recommendations
  - `GET /api/stats` - Summary statistics
  - `POST /api/simulate` - Manual simulation trigger
  - `GET /api/export/noise` - CSV export of noise data
  - `GET /api/export/violations` - CSV export of violations

### Data Model
- **Zone**: Grid cell with location, zoning type, current noise, traffic flow, activity level, status
- **Violation**: Detected when noise exceeds limits (Residential <55 dBA after 10PM/<65 dBA daytime, Commercial <65, Industrial <75)
- **Policy**: Mitigation recommendation (traffic control, industrial compliance, signal timing)
- **SummaryStats**: Dashboard metrics (total violations, average noise, zones at risk, active policies)

## Key Features

1. **Real-Time Simulation**: Auto-generates new data every 10 seconds
2. **Violation Detection**: Compares noise against zoning-specific limits
3. **Interactive Heatmap**: Color-coded cells (green=compliant, yellow=near limit, red=violation) with hover tooltips
4. **Policy Recommendations**: Identifies high-traffic and industrial violations, suggests targeted mitigation
5. **Data Export**: CSV downloads for compliance reporting

## Technical Decisions

### Why Material Design?
Material Design provides excellent patterns for data-dense civic applications with robust grid systems perfect for mapping interfaces.

### Why In-Memory Storage?
For MVP demonstration, in-memory storage provides fast performance and simplifies deployment. Future versions could use PostgreSQL for persistence.

### Nighttime Residential Violation Logic
Critical implementation detail using ADDITIVE MODEL:
- Residential zones are excluded from ALL time-based multipliers (no rush hour boost, no nighttime reduction)
- At night (22:00-06:00), residential zones receive +8 dBA additive offset AFTER base calculation
- Formula: `noise = (base + traffic + activity) + (residential && nighttime ? +8 : 0) + variation`
- This ensures reliable violation detection against 55 dBA limit after 10PM
- Commercial/industrial zones use 1.3× multiplier during rush hours, 0.7× at night

## Design Guidelines
See `design_guidelines.md` for comprehensive Material Design system implementation including:
- Typography hierarchy (Roboto primary, Roboto Mono for metrics)
- Spacing system (4, 6, 8, 12, 16 unit grid)
- Component specifications for heatmap, metrics, violations table, policy cards
- Responsive behavior patterns

## Development Commands
- `npm run dev` - Start development server (port 5000)
- Auto-refresh enabled via HMR

## Testing

### Automated Regression Testing
**Nighttime Residential Violation Enforcement** - Critical automated regression guard:
- Runs automatically on server initialization in `server/storage.ts`
- Tests 500 random residential zone samples at hour 23:00
- **Assertion**: Throws error if violation rate < 35% (current: ~81%)
- **Purpose**: Prevents silent regression of nighttime residential violation logic
- **Implementation**: If test fails, server will not start, forcing immediate investigation
- This ensures the nighttime residential noise calculation (+8 dBA offset) remains functional

### Data-testid Coverage
Comprehensive data-testid attributes added for automated QA:
- All interactive elements (buttons, links, filters, sort controls)
- All data displays (metrics, violations, policies, zone details)
- Heatmap cells and tooltips with sub-element identifiers
- Table rows and cells
- Legend items and filter buttons
- Recent violation cards with all nested data
- Format: `{type}-{description}-{id}` or `{action}-{target}`
