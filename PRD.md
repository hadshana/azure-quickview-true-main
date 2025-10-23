# Azure QuickView - Monitoring Dashboard

A comprehensive Azure resource monitoring dashboard that provides quick insights into subscription health, cost analysis, and actionable remediation workflows.

**Experience Qualities**:
1. **Efficient** - Users can identify and remediate issues within 1-2 clicks, minimizing time spent navigating complex Azure portals
2. **Transparent** - Clear visualization of resource health, costs, and recommendations with no hidden information or confusing states
3. **Professional** - Enterprise-grade interface that instills confidence in monitoring critical cloud infrastructure

**Complexity Level**: Complex Application (advanced functionality, accounts)
This application requires multi-dimensional data filtering, authentication integration, report generation, external system integration (Azure Portal, Outlook), and sophisticated state management across multiple interconnected dashboard views.

## Essential Features

### 1. Subscription Selection
- **Functionality**: Dropdown allowing users to select from available Azure subscriptions
- **Purpose**: Scope all dashboard data to a specific subscription context
- **Trigger**: Page load (default selection) or user dropdown interaction
- **Progression**: Load dashboard → Select subscription dropdown → Choose subscription → All tiles/lists refresh with subscription-specific data
- **Success criteria**: Dashboard data updates within 500ms, subscription context persists across page refreshes

### 2. Dashboard Filters (Multi-Select)
- **Functionality**: Multi-select dropdown with 7 filter options to show/hide specific dashboard sections
- **Purpose**: Allow users to focus on specific problem areas or reports without visual clutter
- **Trigger**: User clicks "Dashboard Filters" dropdown
- **Progression**: Click filters dropdown → Select one or more filters → Dashboard sections immediately show/hide → Unselect filters → Sections reappear
- **Success criteria**: Filtering is instantaneous (<100ms), no layout shift, deselecting all filters shows complete dashboard

### 3. Summary Tiles
- **Functionality**: Four key metric tiles showing Total VMs, Untagged VMs, Running VMs, Stopped VMs
- **Purpose**: Provide at-a-glance infrastructure health metrics
- **Trigger**: Subscription selection or page load
- **Progression**: Select subscription → Tiles animate in → Display current counts → Click tile → Navigate to relevant detail section
- **Success criteria**: Tiles are clearly readable, clickable, and link to detailed views

### 4. Untagged & Unattached Resources Lists
- **Functionality**: Tabular lists showing VMs without tags and orphaned resources (disks, NICs, IPs) with name, resource group, last modified
- **Purpose**: Identify resources that need cleanup or proper tagging for cost allocation
- **Trigger**: Visible when no filters selected or relevant filter selected
- **Progression**: View list → Identify resource → Click action button → Open in Portal or trigger remediation
- **Success criteria**: Lists are sortable, searchable, and action buttons work correctly

### 5. Azure Advisor Recommendations
- **Functionality**: Per-VM recommendations with severity highlighting (Critical, High, Medium, Low)
- **Purpose**: Surface Azure's built-in optimization suggestions for security, cost, reliability
- **Trigger**: Visible when no filters selected or "Azure Advisor Recommendations" filter selected
- **Progression**: View recommendations → Identify critical items → Click to view details → Take remediation action
- **Success criteria**: Critical recommendations are visually distinct, recommendations grouped by VM

### 6. Runbook Schedule Overview
- **Functionality**: Display scheduled runbooks with last job status (Running/Completed/Failed)
- **Purpose**: Monitor automation health and identify failing maintenance tasks
- **Trigger**: Visible when no filters selected or "Runbook Schedule Overview" filter selected
- **Progression**: View runbook list → Identify failed jobs → Click action → Open in Portal for troubleshooting
- **Success criteria**: Status indicators use clear color coding (green/yellow/red), timestamps are human-readable

### 7. Patch Management Summary
- **Functionality**: Show compliance percentage and list of non-compliant hosts
- **Purpose**: Track security posture and identify machines requiring patches
- **Trigger**: Visible when no filters selected or "Patch Management Summary" filter selected
- **Progression**: View compliance → Identify non-compliant hosts → Click action → Remediate or schedule patching
- **Success criteria**: Compliance percentage prominently displayed, non-compliant list is actionable

### 8. Cost Analysis
- **Functionality**: 3-month trend graph and top 10 cost drivers for selected subscription
- **Purpose**: Track spending trends and identify optimization opportunities
- **Trigger**: Visible when no filters selected or "Cost Analysis" or "Top 10 Cost Drivers" filters selected
- **Progression**: View cost graph → Identify spending spikes → Review top cost drivers → Click action → Investigate in Azure Portal
- **Success criteria**: Graph is interactive (tooltips on hover), cost data is accurate, top drivers show resource name and cost

### 9. Action Column
- **Functionality**: Each resource row has action buttons: "Open in Portal" and "Remediate"
- **Purpose**: Provide immediate paths to resolution
- **Trigger**: User clicks action button
- **Progression**: Identify issue → Click "Open in Portal" → New tab opens to Azure Portal resource page OR Click "Remediate" → Trigger automation runbook
- **Success criteria**: Portal links construct correct Azure resource URLs, remediation actions show confirmation dialogs

### 10. Authentication
- **Functionality**: GitHub OAuth sign-in with user profile display
- **Purpose**: Secure access and personalize experience
- **Trigger**: Page load (not authenticated) or sign-out
- **Progression**: Load app → Redirect to GitHub OAuth → Authorize → Return to dashboard → Display user avatar
- **Success criteria**: Authentication persists across sessions, user profile visible in header

### 11. Monthly Summary Report
- **Functionality**: Generate PDF report with dashboard summary and provide download or email options
- **Purpose**: Enable sharing of infrastructure insights with stakeholders
- **Trigger**: User clicks "Send Monthly Summary" button
- **Progression**: Click button → Choose "Download PDF" or "Email" → If Download: PDF downloads → If Email: Outlook opens with pre-populated email and PDF attachment
- **Success criteria**: PDF contains all dashboard data with professional formatting, Outlook integration works on desktop

### 12. Mock Data System
- **Functionality**: Simulated API responses for all Azure data endpoints
- **Purpose**: Enable demo and development without Azure credentials
- **Trigger**: All API calls use mock data provider
- **Progression**: App loads → Mock API returns sample data → Dashboard populates → User can interact with realistic data
- **Success criteria**: Mock data is realistic and comprehensive, README explains how to swap for real Azure APIs

## Edge Case Handling
- **Empty Subscription**: If subscription has no resources, show friendly empty state with setup guidance
- **API Failures**: Display error toast with retry option, maintain last known good state
- **Filtering Edge Cases**: When all filters deselected, show full dashboard; prevent UI layout shift when filtering
- **Long Resource Names**: Truncate with ellipsis and show full name on hover tooltip
- **Large Data Sets**: Implement pagination or virtualization for lists exceeding 50 items
- **Mobile Portrait**: Stack tiles vertically, collapse filters to drawer, tables become horizontally scrollable cards
- **Offline Mode**: Show cached data with "Last updated" timestamp and warning banner

## Design Direction
The design should feel professional, trustworthy, and efficient—reminiscent of enterprise SaaS dashboards like Datadog or Azure Portal itself. Clean, data-dense but not cluttered. Prioritize information hierarchy and actionability over decorative elements. A balanced interface where metrics are immediately scannable and actions are one click away.

## Color Selection
Analogous (adjacent colors on color wheel) - Cool blues and teals to evoke trust, stability, and cloud technology. Secondary warm accents for alerts and critical items.

- **Primary Color**: Deep Azure Blue `oklch(0.45 0.15 250)` - Represents Microsoft Azure brand alignment and professional cloud services
- **Secondary Colors**: 
  - Light Sky Blue `oklch(0.75 0.10 240)` for cards and secondary surfaces
  - Teal `oklch(0.60 0.12 200)` for accents and interactive elements
- **Accent Color**: Vibrant Cyan `oklch(0.70 0.15 210)` for CTAs, active states, and drawing attention to actionable items
- **Foreground/Background Pairings**:
  - Background (White `oklch(0.98 0 0)`): Dark Text `oklch(0.20 0 0)` - Ratio 15.8:1 ✓
  - Card (Light Blue `oklch(0.96 0.02 240)`): Dark Text `oklch(0.20 0 0)` - Ratio 14.1:1 ✓
  - Primary (Azure Blue `oklch(0.45 0.15 250)`): White Text `oklch(0.98 0 0)` - Ratio 7.2:1 ✓
  - Secondary (Sky Blue `oklch(0.75 0.10 240)`): Dark Text `oklch(0.20 0 0)` - Ratio 9.5:1 ✓
  - Accent (Cyan `oklch(0.70 0.15 210)`): White Text `oklch(0.98 0 0)` - Ratio 5.8:1 ✓
  - Muted (Cool Gray `oklch(0.88 0.01 240)`): Medium Text `oklch(0.45 0 0)` - Ratio 5.1:1 ✓
  - Destructive (Alert Red `oklch(0.55 0.22 25)`): White Text `oklch(0.98 0 0)` - Ratio 4.9:1 ✓

## Font Selection
Clean, highly legible sans-serif typefaces that convey professionalism and are optimized for data display. Inter for its excellent readability at small sizes (perfect for tables and metrics) paired with system fonts for performance.

- **Typographic Hierarchy**:
  - H1 (Dashboard Title): Inter SemiBold/32px/tight letter-spacing (-0.02em) - High visual weight for page identity
  - H2 (Section Headers): Inter SemiBold/24px/normal letter-spacing - Clear section delineation
  - H3 (Card Titles): Inter Medium/18px/normal letter-spacing - Subtle hierarchy within sections
  - Body (Tables, Lists): Inter Regular/14px/relaxed line-height (1.6) - Optimized for scanning dense data
  - Metrics (Tile Numbers): Inter Bold/36px/tight letter-spacing - Emphasize key figures
  - Labels (Form Fields): Inter Medium/13px/uppercase/wide letter-spacing (0.05em) - Distinct from content
  - Caption (Timestamps): Inter Regular/12px/muted color - De-emphasized metadata

## Animations
Animations should be subtle and functional, reinforcing the professional nature of the tool. Motion communicates state changes (loading, filtering, success) without distracting from data analysis. Quick, purposeful transitions that guide the eye.

- **Purposeful Meaning**: Use gentle fade-ins for dashboard tiles (staggered 50ms delay) to create sense of data loading; slide-out transitions for filter panels; pulse animations for status indicators (running jobs)
- **Hierarchy of Movement**: Priority 1 - Filter application (immediate, no delay); Priority 2 - Tile updates (200ms fade); Priority 3 - Toast notifications (slide-in from top-right); Priority 4 - Hover states (100ms color transitions)

## Component Selection
- **Components**: 
  - `Select` for Subscription and Filter dropdowns (multi-select mode for filters)
  - `Card` for summary tiles and dashboard sections with subtle shadows
  - `Table` for resource lists with sortable headers
  - `Badge` for status indicators (Running/Failed/Completed, severity levels)
  - `Button` for action columns (variant="ghost" for "Open Portal", variant="default" for "Remediate")
  - `Dialog` for remediation confirmations
  - `Dropdown Menu` for "Send Monthly Summary" options (Download/Email)
  - `Tabs` for switching between Untagged VMs and Unattached Resources
  - `Progress` for compliance percentage visualization
  - `Chart` (Recharts) for 3-month cost analysis area chart
  - `Avatar` for user profile in header
  - `Toaster` (Sonner) for success/error notifications
- **Customizations**: 
  - Custom dashboard grid layout using CSS Grid with responsive breakpoints
  - Custom status badge color scheme (green/yellow/red for job status; blue/orange/red for severity)
  - Custom metric tile component with large number display and icon
- **States**: 
  - Buttons: Default (primary blue), Hover (darker blue), Active (pressed effect), Disabled (gray with reduced opacity)
  - Dropdowns: Default (border), Focus (ring + border), Open (elevated shadow), Populated (show count badge)
  - Cards: Default (subtle shadow), Hover (lifted shadow for clickable tiles), Selected (blue border)
- **Icon Selection**: 
  - `ChartLine` for cost analysis
  - `Warning` for critical recommendations
  - `CheckCircle`/`XCircle` for job status
  - `Tag` for untagged VMs
  - `HardDrive`/`NetworkCard` for resource types
  - `ArrowSquareOut` for "Open Portal" actions
  - `Wrench` for remediation actions
  - `FileArrowDown` for PDF download
  - `Envelope` for email
  - `Funnel` for filters icon
- **Spacing**: 
  - Consistent 16px (space-4) gap between cards in grid
  - 24px (space-6) section padding
  - 8px (space-2) between label and input
  - 12px (space-3) between table rows
  - 32px (space-8) between major sections
- **Mobile**: 
  - Tiles: 4-column grid on desktop → 2-column on tablet → 1-column on mobile
  - Tables: Horizontal scrollable wrapper with sticky first column on mobile
  - Filters: Collapse to floating action button with drawer on mobile
  - Header: Stack subscription selector and filters vertically on mobile
  - Charts: Responsive width, simplified tooltips with larger touch targets
  - Navigation: Fixed top header with z-index layering
