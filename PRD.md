# PRD - Enterprise Resource Planning System

## 1. Visual Identity

### Typography
- Headings: Inter, 600 weight, 24px (H1), 20px (H2), 16px (H3)
- Body: Inter, 400 weight, 14px
- Mono: JetBrains Mono, 400 weight, 13px (code/IDs)
- Line height: 1.5 for body, 1.2 for headings

### Color Palette
- Primary: #0052CC (actions, links)
- Secondary: #6B778C (secondary text)
- Background: #FFFFFF (main), #F4F5F7 (cards/panels)
- Text: #172B4D (primary), #6B778C (secondary)
- Success: #00875A
- Warning: #FF8B00
- Error: #DE350B
- Border: #DFE1E6

### Spacing
- Base unit: 8px
- Card padding: 16px
- Section gap: 24px
- Border radius: 3px (buttons), 8px (cards)

---

## 2. User Roles and Authentication

### Roles
- Super Admin: Full system access, user management, billing
- Admin: Manage workspaces, integrations, user permissions
- Member: Create/edit projects, wiki, dashboards within assigned workspace
- Guest: View-only access to shared projects

### Login Flow
- Email/password authentication
- SSO via Google Workspace, Microsoft Entra
- Magic link option for passwordless login
- Session duration: 24 hours, extendable to 30 days with "Remember me"
- MFA enforcement for Admin/Super Admin roles

---

## 3. Pages and Navigation

### Global Navigation (Left Sidebar - 240px width)
- Logo at top (40px height)
- Workspace selector dropdown
- Main sections: Projects, Wiki, Dashboards, Integrations
- Bottom: User avatar, settings gear, collapse toggle
- Collapsed state: 56px icon-only mode

### 3.1 Projects Page (/projects)
- Grid/List toggle view
- Filter bar: Status (Active/Archived), Owner, Tags
- Sort: Name A-Z, Created Date, Last Updated
- Project card (grid): 280px width, shows title, description (2 lines truncated), member avatars (max 5 + overflow count), progress bar, last activity timestamp
- Quick actions on hover: Star, Archive, More menu

### 3.2 Project Board View (/projects/{id}/board)
- Horizontal scrolling board with columns
- Default columns: Backlog, To Do, In Progress, Review, Done
- Column header: Title, card count badge, Add card button (+ icon)
- Drag-and-drop cards between columns
- Card component: 240px min-width, white background, 8px radius
  - Title (14px, bold), Description preview (2 lines)
  - Priority indicator (colored left border: red/blue/green/gray)
  - Assignee avatars (24px circles)
  - Due date chip (red if overdue)
  - Label tags (pill shape, 6px radius)
- Swimlanes option: Group by Assignee or Priority
- Quick add: Press "N" to open new card modal

### 3.3 Sprint Planning View (/projects/{id}/sprints)
- Two-panel layout: Sprint backlog (left 50%), Active sprint (right 50%)
- Sprint selector dropdown at top
- Sprint header: Name, dates (start-end), velocity, progress percentage
- Story points estimation display per card
- Sprint capacity indicator (hours/days remaining vs total)
- Drag cards from backlog to active sprint
- Sprint burndown chart (line chart, 300px height)

### 3.4 Wiki/Documentation (/wiki/{id})
- Sidebar navigation tree (200px width)
  - Nested pages up to 3 levels deep
  - Expand/collapse folders
  - Drag to reorder pages
  - "+" button to add child page
- Main content area: Max-width 720px, centered
- Page title (H1, 28px), breadcrumb trail
- Rich text editor toolbar: Bold, Italic, Headings (H2-H4), Bullet list, Numbered list, Code block, Link, Image upload, Table
- Autosave indicator ("Saved 2 seconds ago")
- Version history sidebar (right side, toggleable)
- Comments thread below content (collapsed by default)
- Share button: Generate read-only public link

### 3.5 KPI Dashboard (/dashboards/{id})
- Edit mode toggle (pencil icon)
- Drag-and-drop tile grid (12-column grid, 8px gap)
- Tile sizes: Small (2x2), Medium (4x2), Large (6x4), Wide (6x2)
- Tile types:
  - Number card: Large value, label, trend indicator (arrow + percentage)
  - Line chart: Time series data, 7d/30d/90d toggles
  - Bar chart: Categorical comparison
  - Pie/Donut chart: Distribution
  - Table: Configurable columns, max 10 rows with pagination
  - Progress ring: Goal completion percentage
- Date range picker (top right): Preset ranges + custom picker
- Refresh button with last-updated timestamp
- Export button: CSV, PDF

### 3.6 Integration Settings (/settings/integrations)
- Integration cards in grid: Icon, name, status badge, Connect/Disconnect button
- Available integrations: Slack, Jira, GitHub, Zapier, Custom Webhooks
- Webhook panel:
  - URL field (auto-generated)
  - HTTP method selector (POST, GET)
  - Headers editor (key-value pairs)
  - Event triggers: checkboxes for Project Created, Card Moved, Wiki Updated, etc.
  - Payload template preview
  - Test webhook button -> shows response status, latency, body
  - Recent deliveries log table: Timestamp, Event, Status code, Retry count

---

## 4. Interactive Elements and Animations

### Buttons
- Primary: #0052CC background, white text, 3px radius
- Hover: darken 10%, cursor pointer
- Active: scale(0.98), 100ms
- Disabled: opacity 0.5, cursor not-allowed
- Loading: spinner icon replacing text

### Modals
- Centered, max-width 480px (small), 640px (medium), 960px (large)
- Backdrop: rgba(9, 30, 66, 0.54)
- Entry: fade in 150ms, scale from 0.95 to 1
- Exit: fade out 100ms

### Dropdowns
- Trigger: chevron-down icon, rotates 180deg on open
- Menu: white background, 1px border, 4px radius, 4px shadow
- Entry: opacity 0 to 1, translateY(-8px) to 0, 150ms ease-out
- Items: 32px height, hover background #F4F5F7

### Drag and Drop
- Drag start: card lifts with shadow, opacity 0.8
- Dragging: cursor grabbing, placeholder shown in original position
- Drop zone highlight: dashed border, light blue background
- Drop: card settles with bounce easing, 200ms

### Notifications
- Toast position: bottom-right, 16px from edges
- Auto-dismiss: 5 seconds (success), persistent (error)
- Stack up to 3, older ones push up
- Entry: slide in from right, 200ms
- Close button (X) on hover

---

## 5. Data Model

| Entity | Fields |
|--------|--------|
| User | id (UUID), email, password_hash, name, avatar_url, role (enum), created_at, last_login |
| Workspace | id, name, slug, owner_id (FK User), plan_type, created_at |
| Project | id, title, description, status (Active/Archived), workspace_id (FK), created_by (FK User), created_at, updated_at |
| Sprint | id, name, project_id (FK), start_date, end_date, goal, status (Planning/Active/Completed) |
| Card | id, title, description, priority (Low/Medium/High/Critical), story_points, sprint_id (FK, nullable), column_name, position, assignee_id (FK User), due_date, created_by (FK User), created_at, updated_at |
| WikiPage | id, title, content (JSON), parent_id (FK WikiPage, nullable), project_id (FK), author_id (FK User), position, created_at, updated_at |
| Dashboard | id, title, project_id (FK), layout_config (JSON), created_at |
| DashboardTile | id, dashboard_id (FK), type, title, data_config (JSON), size (enum), position_x, position_y |
| Integration | id, name, type, config (JSON), workspace_id (FK), status, last_sync_at |
| Webhook | id, integration_id (FK), url, method, headers (JSON), events (JSON array), is_active, created_at |
| WebhookDelivery | id, webhook_id (FK), event_type, payload (JSON), status_code, response_body, latency_ms, delivered_at, retry_count |
| Comment | id, body, user_id (FK), card_id (FK, nullable), wiki_page_id (FK, nullable), created_at |
| Attachment | id, filename, url, file_type, size_bytes, uploaded_by (FK User), card_id (FK, nullable), created_at |

### Relationships
- Workspace has many Projects
- Project has many Sprints, Cards, WikiPages, Dashboards
- Sprint has many Cards
- Card belongs to Sprint (optional), Assignee, Creator
- WikiPage has self-referential parent for nesting
- Dashboard has many DashboardTiles
- Workspace has many Integrations
- Integration has many Webhooks
- Webhook has many WebhookDeliveries
- Comments and Attachments polymorphic (Card or WikiPage)

---

## 6. Search, Filter, and Sort

### Global Search (Cmd+K)
- Modal overlay with search input
- Results grouped by type: Projects, Cards, Wiki Pages, Users
- Recent searches shown when empty
- Keyboard navigation: Arrow keys, Enter to select
- Fuzzy matching on title/description

### Card Filters
- Multi-select dropdowns: Assignee, Label, Priority
- Date range picker for Due Date
- Text search within card titles
- Filter chips appear below filter bar, removable individually
- Clear all button

### Sort Options
- Drag-and-drop manual ordering
- Alphabetical A-Z, Z-A
- Due date (soonest first, overdue first)
- Priority (High to Low)
- Recently updated
- Story points

---

## 7. Error Handling, Empty States, and Notifications

### Error States
- Network error: Toast "Connection lost. Retrying..." with retry button
- 404 page: Illustration, "Page not found" heading, back to home button
- Permission denied: "You don't have access" message with request access button
- Form validation: Inline red text below field, red border on input
- API error: Toast with error message, support link if persistent

### Empty States
- No projects: Illustration of empty folder, "Create your first project" heading, "New Project" button, brief description
- No cards in column: Dashed border placeholder, "Drop cards here" text
- No search results: Magnifying glass illustration, "No results for [query]" with suggestions
- Empty wiki: "Start with a title" placeholder, getting started guide link

### Notifications
- Success: Green left border, checkmark icon
- Warning: Orange left border, alert icon
- Error: Red left border, X icon
- Info: Blue left border, info icon
- Bell icon in header shows unread count badge

---

## 8. Real-time Features

### Live Updates
- Card movements broadcast to workspace members
- Presence indicators: Show active users viewing same project (avatar stack)
- Typing indicators in comments
- Real-time sync via WebSocket connection
- Optimistic UI updates with rollback on conflict

### Notifications Center
- Dropdown from bell icon
- Grouped by date (Today, Yesterday, Earlier)
- Mark as read on click
- Mark all read button
- Links to relevant card/wiki/dashboard

---

## 9. Admin Tools

### User Management (/admin/users)
- User table: Name, Email, Role, Status, Last Active, Actions
- Bulk actions: Change role, Deactivate, Delete
- Invite new user form: Email input, role dropdown, send invitation button
- Activity log: Filterable by user, action type, date range

### Workspace Settings
- General: Name, logo upload, timezone
- Billing: Plan display, upgrade CTA, invoice history
- Security: Password policy, session timeout, IP allowlist
- Audit log: All admin actions with timestamp, user, details

### System Health Dashboard
- Service status indicators (green/yellow/red)
- API response time graph (24h)
- Error rate percentage
- Active connections count
- Recent incidents table

---

## 10. Responsive Behavior

### Breakpoints
- Desktop: 1200px+ (full layout)
- Tablet: 768px-1199px (collapsed sidebar, reduced columns)
- Mobile: <768px (bottom navigation, single column board)

### Mobile Adaptations
- Sidebar becomes bottom tab bar with 5 icons
- Board view: Single column with horizontal scroll
- Cards: Full-width, stacked vertically
- Wiki: No sidebar, hamburger menu for navigation
- Dashboards: Tiles stack vertically, 1 column

### Heavy Visuals
- Charts: Simplify to essential data points on mobile
- Animations: Reduce motion for prefers-reduced-motion
- Images: Lazy load with blur placeholder

---

## 11. Performance and Accessibility

### Performance Targets
- First Contentful Paint: < 1.5s
- Time to Interactive: < 3s
- Lighthouse Performance: > 90
- Bundle size: < 500KB initial load

### Techniques
- Code splitting per route
- Image optimization (WebP, lazy loading)
- Virtual scrolling for long lists (100+ items)
- Debounced search input (300ms)
- Service worker for offline support

### Accessibility
- WCAG 2.1 AA compliance
- All interactive elements keyboard accessible
- Focus indicators: 2px solid #0052CC outline
- Screen reader announcements for dynamic content
- prefers-reduced-motion: Disable animations except essential ones
- Color contrast ratio: > 4.5:1 for text
- Form labels associated with inputs