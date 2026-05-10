# PRD: Procurion -- B2B Procurement Platform

## 1. Overview

Procurion is a B2B procurement platform designed for mid-to-large enterprises that need to manage bulk ordering, supplier discovery, RFQ (Request for Quotation) workflows, and purchase order tracking at scale. The platform serves procurement managers, suppliers, finance approvers, and system administrators. It replaces fragmented email-and-spreadsheet procurement processes with a centralized, auditable, real-time system.

Target users: procurement teams at manufacturing companies, retail chains, construction firms, and government agencies who purchase raw materials, components, or services in volume from a distributed supplier base.

---

## 2. User Roles

- Guest -- Unauthenticated visitor. Can browse the public supplier directory and view marketing pages. Cannot access catalog pricing, RFQ forms, or order data.
- Buyer -- Authenticated procurement professional. Can search the catalog, create RFQs, place purchase orders, manage saved suppliers, track shipments, and invite team members.
- Buyer Manager -- Senior buyer with approval authority. All Buyer permissions plus approve/reject POs above threshold, manage team budgets, and view org-wide spend analytics.
- Supplier -- Seller account. Can list products, set pricing tiers and MOQs, respond to RFQs, manage inventory, view incoming orders, and update fulfillment status.
- Supplier Manager -- Supplier admin. All Supplier permissions plus manage sub-accounts, view sales analytics, and configure company profile and certifications.
- Finance Approver -- Read-only access to PO and invoice data. Can approve or reject purchase orders within their approval tier. Cannot create RFQs or modify catalog.
- System Admin -- Full platform access. Manages org settings, user provisioning, integration configs (ERP, accounting), and platform-wide policies.

---

## 3. Authentication and Authorization

### 3.1 Login Methods

- Email + password with mandatory MFA (TOTP or SMS) for all paid accounts.
- SSO via SAML 2.0 for enterprise orgs (Okta, Azure AD, Google Workspace).
- Magic link login as fallback for users locked out of MFA.

### 3.2 Signup Flow

1. User clicks "Start Free Trial" on landing page.
2. Enters work email, company name, full name, password (min 12 chars, complexity enforced).
3. Receives verification email. Clicks link to confirm.
4. Selects role: Buyer or Supplier.
5. Completes onboarding wizard: company details, industry, estimated annual spend (Buyer) or product categories (Supplier).
6. Account created in "Trial" status with 30-day access to all features.

### 3.3 Session Management

- JWT access tokens (15 min TTL) stored in httpOnly secure cookies.
- Refresh tokens (7 day TTL) rotated on each use.
- Idle timeout: 30 minutes of inactivity triggers re-auth prompt.
- Concurrent session limit: 3 active sessions per user. New login evicts oldest session.
- All auth events (login, logout, password change, MFA enrollment) logged to audit trail.

### 3.4 Role-Based Access Control

Permissions are checked at the API gateway layer. Each endpoint declares required permission scopes. Example:

- POST /rfqs requires scope `rfq:create`
- PUT /purchase-orders/:id/approve requires scope `po:approve`
- GET /analytics/spend requires scope `analytics:read_org`

Org-level data isolation enforced via tenant_id claim in JWT. All queries filter by tenant_id.

---

## 4. Core Features as User Flows

### 4.1 Supplier Discovery and Catalog Browse

1. Buyer lands on the dashboard and clicks "Explore Suppliers" or uses the global search bar.
2. Search results display in a 3-column card grid with 16px gap. Each card shows: supplier name, logo (64x64), rating (1-5 stars), primary category, location, and "Verified" badge if applicable.
3. Buyer applies filters: category (multi-select dropdown), location (country/region), certification (ISO 9001, ISO 14001, etc.), minimum order quantity range, lead time range, and supplier rating.
4. Results update in real-time (debounced 300ms) with a result count shown above the grid.
5. Buyer clicks a supplier card to view the Supplier Profile page.
6. Supplier Profile shows: company overview, certifications (with verification links), product catalog (paginated, 24 items per page), contact info, and customer reviews.
7. Buyer clicks a product to view the Product Detail page with: title, description, SKU, images (gallery with thumbnails on left, main image on right), pricing tiers table, MOQ, lead time, specifications table, and related products.
8. Buyer can add the product to a "Quick RFQ" list or save the supplier to their "My Suppliers" list.

### 4.2 RFQ (Request for Quotation) Workflow

1. Buyer clicks "Create RFQ" from the dashboard or from a product detail page.
2. Step 1 -- RFQ Details: Buyer enters RFQ title, selects delivery location (auto-fills from company address), sets required delivery date (date picker, min 7 days from today), and adds internal notes (visible only to buyer team).
3. Step 2 -- Line Items: Buyer adds products by searching the catalog or entering custom items. For each line item: product/SKU, quantity (validated against MOQ if applicable), unit of measure, target unit price (optional), and special instructions.
4. Step 3 -- Supplier Selection: Buyer selects suppliers to send the RFQ to. Options: select from saved suppliers, search the directory, or broadcast to all suppliers in a category (requires Buyer Manager approval for broadcast).
5. Step 4 -- Review and Submit: Summary view showing all RFQ details. Buyer can save as draft or submit. On submit, selected suppliers receive email notification and in-platform notification.
6. Supplier receives RFQ notification. Opens RFQ detail page showing all line items and buyer requirements.
7. Supplier responds by entering: unit price per line item, quantity they can fulfill, estimated lead time, validity period for the quote (default 30 days), and counter-terms or notes.
8. Supplier submits response. Buyer receives notification.
9. Buyer views all responses side-by-side in a comparison table. Columns: supplier name, total quoted price, per-unit price for each line item, lead time, and validity.
10. Buyer selects a winning response. System notifies the winning supplier and auto-creates a draft Purchase Order. Losing suppliers receive a polite decline notification.
11. If no responses are received within the RFQ deadline (configurable, default 7 days), RFQ auto-closes and buyer is notified.

### 4.3 Purchase Order Management

1. From a winning RFQ or directly from the catalog, Buyer creates a Purchase Order.
2. PO form pre-fills from RFQ data: supplier, line items, pricing, delivery date.
3. Buyer adds: payment terms (Net 30, Net 60, COD), shipping method, and any attachments (specs, contracts as PDF).
4. If PO total exceeds the buyer's approval threshold (configurable per org, default $10,000), PO is routed to a Buyer Manager or Finance Approver.
5. Approver receives notification, reviews PO, and clicks Approve or Reject with comments.
6. On approval, PO status changes to "Approved" and supplier receives the PO via email and in-platform notification.
7. Supplier acknowledges PO, updates fulfillment status at each stage: Accepted, In Production, Shipped (with tracking number), Delivered.
8. Buyer tracks PO status on the Order Tracking dashboard: table view with columns PO number, supplier, order date, expected delivery, status (color-coded badges), and actions (view details, cancel, raise dispute).
9. On delivery, Buyer confirms receipt. PO status changes to "Completed." If issues, Buyer raises a Dispute (see 4.5).

### 4.4 Bulk Ordering

1. Buyer navigates to "Bulk Order" from the dashboard.
2. Uploads a CSV file with columns: SKU, quantity, delivery_date, notes.
3. System validates each row: checks SKU exists, quantity meets MOQ, delivery date is feasible.
4. Validation errors displayed inline with row highlighting (red border, error message below row).
5. Valid rows are grouped by supplier automatically.
6. Buyer reviews grouped order summary, adjusts quantities if needed.
7. Submits bulk order, which generates one PO per supplier.

### 4.5 Dispute Resolution

1. Buyer opens a PO and clicks "Raise Dispute."
2. Selects dispute type: Quality Issue, Late Delivery, Wrong Items, Short Shipment, Other.
3. Adds description (min 50 chars), uploads evidence (photos, documents, max 10MB per file, max 5 files).
4. Supplier is notified and has 5 business days to respond.
5. Supplier responds with resolution offer: full refund, partial refund, replacement shipment, or credit note.
6. Buyer accepts or counters. If unresolved after 10 business days, it escalates to Procurion support.
7. All dispute communications are logged in an immutable audit trail per PO.

### 4.6 Spend Analytics

1. Buyer Manager or Finance Approver navigates to "Analytics" from the sidebar.
2. Dashboard shows: total spend (current month, YTD), spend by category (donut chart), top 10 suppliers by spend (horizontal bar chart), PO approval cycle time (line chart, last 12 months), and spend vs. budget (gauge chart).
3. User can filter by date range, department, category, and supplier.
4. Data exports available as CSV or PDF report.

---

## 5. Data Model

### 5.1 Entities

| Entity | Fields | Relationships |
|---|---|---|
| Organization | id, name, industry, address, logo_url, subscription_tier, created_at, updated_at | has many Users, has many PurchaseOrders |
| User | id, org_id, email, password_hash, full_name, role, mfa_enabled, status, last_login_at, created_at | belongs to Organization, has many Notifications |
| SupplierProfile | id, org_id, company_name, description, logo_url, website, location_country, location_city, verified, rating_avg, rating_count, created_at | belongs to Organization, has many Certifications, has many Products |
| Certification | id, supplier_id, name, issuing_body, certificate_url, verified, issued_date, expiry_date | belongs to SupplierProfile |
| Product | id, supplier_id, sku, title, description, category, unit_of_measure, moq, lead_time_days, images (JSON array of URLs), specifications (JSON), status, created_at, updated_at | belongs to SupplierProfile, has many PricingTiers, has many RFQLineItems |
| PricingTier | id, product_id, min_quantity, max_quantity, unit_price, currency | belongs to Product |
| RFQ | id, buyer_org_id, created_by_user_id, title, delivery_location, delivery_date, status (draft, open, closed, awarded, expired), notes, deadline_at, created_at | belongs to Organization, has many RFQLineItems, has many RFQResponses |
| RFQLineItem | id, rfq_id, product_id (nullable for custom items), custom_description, quantity, unit_of_measure, target_unit_price, special_instructions | belongs to RFQ, optionally belongs to Product |
| RFQSupplier | id, rfq_id, supplier_org_id, invited_at, viewed_at, responded_at | belongs to RFQ, belongs to Organization (supplier) |
| RFQResponse | id, rfq_id, supplier_org_id, total_price, validity_days, notes, status (submitted, shortlisted, accepted, declined), submitted_at | belongs to RFQ, has many RFQResponseLineItems |
| RFQResponseLineItem | id, response_id, rfq_line_item_id, unit_price, quantity_available, lead_time_days | belongs to RFQResponse, belongs to RFQLineItem |
| PurchaseOrder | id, buyer_org_id, supplier_org_id, rfq_response_id (nullable), po_number (auto-generated, sequential per org), status (draft, pending_approval, approved, rejected, acknowledged, in_production, shipped, delivered, completed, disputed, cancelled), payment_terms, shipping_method, total_amount, currency, expected_delivery_date, approved_by_user_id, approved_at, created_at, updated_at | belongs to Organization (buyer), belongs to Organization (supplier), has many POLineItems, has many POStatusHistory entries |
| POLineItem | id, po_id, product_id, description, quantity, unit_price, unit_of_measure, line_total | belongs to PurchaseOrder |
| POStatusHistory | id, po_id, from_status, to_status, changed_by_user_id, notes, created_at | belongs to PurchaseOrder |
| Dispute | id, po_id, raised_by_user_id, type, description, status (open, supplier_responded, buyer_reviewing, escalated, resolved, closed), resolution_type, resolution_amount, created_at, resolved_at | belongs to PurchaseOrder |
| DisputeMessage | id, dispute_id, sender_user_id, message, attachments (JSON array), created_at | belongs to Dispute |
| Notification | id, user_id, type, title, body, entity_type, entity_id, read_at, created_at | belongs to User |
| AuditLog | id, org_id, user_id, action, entity_type, entity_id, changes (JSON), ip_address, created_at | belongs to Organization |

### 5.2 Indexes

- Users: unique index on (email), index on (org_id, role)
- Products: index on (supplier_id, status), index on (sku), full-text index on (title, description, category)
- RFQs: index on (buyer_org_id, status), index on (deadline_at)
- PurchaseOrders: index on (buyer_org_id, status), index on (supplier_org_id, status), index on (po_number, buyer_org_id) unique
- Notifications: index on (user_id, read_at, created_at)

---

## 6. API Design

All endpoints prefixed with /api/v1. Responses use JSON envelope: { "data": ..., "meta": ..., "errors": ... }.

### 6.1 Auth

- POST /api/v1/auth/register -- Create account
- POST /api/v1/auth/login -- Authenticate, return JWT pair
- POST /api/v1/auth/refresh -- Refresh access token
- POST /api/v1/auth/logout -- Invalidate refresh token
- POST /api/v1/auth/mfa/enable -- Generate TOTP secret
- POST /api/v1/auth/mfa/verify -- Verify TOTP code
- POST /api/v1/auth/password/reset-request -- Send reset email
- POST /api/v1/auth/password/reset -- Confirm new password

### 6.2 Suppliers

- GET /api/v1/suppliers -- List suppliers (paginated, filterable by category, location, rating, certification)
- GET /api/v1/suppliers/:id -- Supplier profile with certifications
- GET /api/v1/suppliers/:id/products -- Supplier's product catalog (paginated, filterable)
- GET /api/v1/suppliers/:id/reviews -- Supplier reviews (paginated)

### 6.3 Products

- GET /api/v1/products -- Full-text search with filters (category, MOQ range, price range, lead time)
- GET /api/v1/products/:id -- Product detail with pricing tiers
- POST /api/v1/products -- Supplier creates product (requires supplier role)
- PUT /api/v1/products/:id -- Supplier updates product
- DELETE /api/v1/products/:id -- Soft-delete product

### 6.4 RFQs

- POST /api/v1/rfqs -- Create RFQ (draft or submitted)
- GET /api/v1/rfqs -- List RFQs for buyer org (filterable by status, date range)
- GET /api/v1/rfqs/:id -- RFQ detail with line items and responses
- PUT /api/v1/rfqs/:id -- Update draft RFQ
- POST /api/v1/rfqs/:id/submit -- Submit draft RFQ, notify suppliers
- POST /api/v1/rfqs/:id/close -- Manually close RFQ
- POST /api/v1/rfqs/:id/award/:response_id -- Award RFQ to a response

- GET /api/v1/supplier/rfqs -- List RFQs received by supplier (filterable by status)
- GET /api/v1/supplier/rfqs/:id -- RFQ detail from supplier perspective
- POST /api/v1/supplier/rfqs/:id/respond -- Submit RFQ response with line item pricing

### 6.5 Purchase Orders

- POST /api/v1/purchase-orders -- Create PO
- GET /api/v1/purchase-orders -- List POs (filterable by status, date range, supplier)
- GET /api/v1/purchase-orders/:id -- PO detail with line items and status history
- POST /api/v1/purchase-orders/:id/approve -- Approve PO (requires approver role)
- POST /api/v1/purchase-orders/:id/reject -- Reject PO with comments
- POST /api/v1/purchase-orders/:id/acknowledge -- Supplier acknowledges PO
- PUT /api/v1/purchase-orders/:id/status -- Supplier updates fulfillment status
- POST /api/v1/purchase-orders/:id/confirm-receipt -- Buyer confirms delivery
- POST /api/v1/purchase-orders/:id/cancel -- Cancel PO (only if not yet shipped)

### 6.6 Bulk Orders

- POST /api/v1/bulk-orders/validate -- Validate CSV, return errors per row
- POST /api/v1/bulk-orders/submit -- Submit validated bulk order

### 6.7 Disputes

- POST /api/v1/purchase-orders/:id/disputes -- Raise dispute
- GET /api/v1/disputes/:id -- Dispute detail with message thread
- POST /api/v1/disputes/:id/messages -- Add message to dispute
- POST /api/v1/disputes/:id/resolve -- Accept resolution

### 6.8 Analytics

- GET /api/v1/analytics/spend -- Spend data (filterable by date, category, supplier)
- GET /api/v1/analytics/suppliers -- Supplier performance metrics
- GET /api/v1/analytics/cycle-time -- PO approval cycle time data

### 6.9 Notifications

- GET /api/v1/notifications -- List notifications (paginated, filterable by read status)
- PUT /api/v1/notifications/:id/read -- Mark as read
- PUT /api/v1/notifications/read-all -- Mark all as read

---

## 7. Search, Filter, and Sort

### 7.1 Product Search

- Full-text search on title, description, SKU, category using Elasticsearch.
- Autocomplete suggestions after 2 characters (debounced 200ms).
- Filters: category (hierarchical, multi-select), price range (slider), MOQ range (slider), lead time (dropdown: under 7 days, 7-14, 14-30, 30+), supplier location (country multi-select), supplier rating (minimum stars), certifications (multi-select).
- Sort: relevance (default), price low-high, price high-low, MOQ low-high, lead time short-long, supplier rating.

### 7.2 Supplier Directory Search

- Full-text search on company name, description, product titles.
- Filters: category, location, certifications, verified status, rating.
- Sort: relevance, rating, product count, newest.

### 7.3 Purchase Order Table

- Columns sortable: PO number, supplier name, order date, expected delivery, total amount, status.
- Filters: status (multi-select), date range picker, supplier (autocomplete search), amount range.
- Search: keyword search on PO number, supplier name, product names.

---

## 8. Error Handling

### 8.1 API Error Responses

All errors returned in consistent format:

{ "errors": [{ "code": "VALIDATION_ERROR", "message": "Quantity must meet minimum order quantity of 100", "field": "quantity" }] }

Error codes and HTTP status mapping:

- 400 VALIDATION_ERROR -- Input validation failed. Returns field-level errors.
- 401 UNAUTHORIZED -- Missing or expired token.
- 403 FORBIDDEN -- User lacks required permission scope.
- 404 NOT_FOUND -- Resource does not exist or belongs to another tenant.
- 409 CONFLICT -- State conflict (e.g., approving an already-approved PO).
- 422 UNPROCESSABLE -- Business rule violation (e.g., PO total exceeds org limit without approval routing).
- 429 RATE_LIMITED -- Too many requests. Returns Retry-After header.
- 500 INTERNAL -- Unexpected server error. Logged with trace ID. User sees generic message with trace ID.

### 8.2 Client-Side Error Handling

- Network failures: toast notification "Connection lost. Retrying..." with automatic retry (3 attempts, exponential backoff: 1s, 2s, 4s).
- Form validation: inline error messages below each field, red border on invalid fields, submit button disabled until all errors resolved.
- Empty states: each list view has a dedicated empty state with illustration and a CTA. Example: "No RFQs yet. Create your first RFQ to start receiving supplier quotes."
- Session expiry: modal overlay "Your session has expired. Please log in again." with redirect to login.
- File upload errors: specific messages for file too large, unsupported format, upload interrupted.

### 8.3 Notification System

- In-app notifications: real-time via WebSocket connection. Bell icon in header with unread count badge. Dropdown shows last 20 notifications with timestamp, icon per type, and "Mark all read" button.
- Email notifications: configurable per user in settings. Default on for: RFQ responses, PO approvals, shipment updates, dispute messages. Default off for: new supplier in directory, analytics reports.
- Notification types and triggers:
  - RFQ_RECEIVED -- Supplier gets new RFQ
  - RFQ_RESPONDED -- Buyer gets supplier quote
  - RFQ_AWARDED -- Winning supplier notified
  - RFQ_EXPIRED -- Buyer notified when deadline passes with no responses
  - PO_APPROVED -- Supplier gets approved PO
  - PO_REJECTED -- Buyer notified of rejection with approver comments
  - PO_SHIPPED -- Buyer gets tracking info
  - PO_DELIVERED -- Buyer prompted to confirm receipt
  - DISPUTE_OPENED -- Supplier notified of new dispute
  - DISPUTE_MESSAGE -- New message in dispute thread
  - DISPUTE_RESOLVED -- Both parties notified of resolution

---

## 9. Real-Time Features

- WebSocket connection per authenticated session for push notifications and live status updates.
- PO status changes propagate in real-time to the PO detail view without page refresh.
- RFQ response count updates live on the buyer's RFQ detail page.
- Inventory availability indicators on product pages update in real-time when supplier adjusts stock.

---

## 10. Responsive Behavior

### 10.1 Breakpoints

- Desktop: 1280px and above. Full layout: sidebar navigation (240px fixed), main content area, right-side detail panels where applicable.
- Tablet: 768px to 1279px. Sidebar collapses to icon-only (64px). Detail panels collapse into modal overlays. Tables switch to card view on the narrow end.
- Mobile: below 768px. Bottom tab navigation replaces sidebar. Tables become scrollable horizontally or switch to stacked card layout. Filters move into a slide-up drawer. RFQ creation wizard becomes a full-screen step-by-step flow.

### 10.2 Component Adaptations

- Data tables on mobile: horizontal scroll with sticky first column (PO number or product name). Filter bar becomes a "Filters" button that opens a bottom sheet.
- Charts on mobile: simplified versions, single column layout, touch-friendly tooltips.
- Image galleries: desktop shows thumbnails on left; mobile shows swipeable carousel with dots indicator.

---

## 11. Performance Targets

- Page load (LCP): under 2.5 seconds on 4G connection.
- API response time: p95 under 200ms for reads, under 500ms for writes.
- Search autocomplete: results appear within 300ms of keystroke.
- WebSocket message delivery: under 500ms from server event to client display.
- CSV bulk upload validation: under 5 seconds for 1000 rows.

### 11.1 Techniques

- Server-side pagination on all list endpoints (default 25 items per page, max 100).
- Image optimization: WebP format with responsive srcset, lazy loading below the fold.
- API response caching: Redis cache for supplier directory and product search results (TTL 5 minutes, invalidated on write).
- Database query optimization: indexed columns for all filterable/sortable fields. N+1 query prevention via eager loading.
- CDN: static assets served from edge locations. API responses cached at CDN for read-heavy public endpoints (supplier directory).

---

## 12. Accessibility

- All interactive elements keyboard-navigable with visible focus indicators (2px solid outline, color: #2563EB).
- Color contrast minimum 4.5:1 for body text, 3:1 for large text and UI components.
- All images have descriptive alt text. Decorative images use empty alt attributes.
- Form fields have associated labels (not placeholder-only). Error messages are linked to fields via aria-describedby.
- Status badges use both color and text (not color alone) to convey meaning.
- Screen reader announcements for real-time notification arrivals via aria-live="polite" region.
- Skip navigation link at top of every page.
- prefers-reduced-motion: disables all CSS transitions and animations. Chart animations replaced with instant render. Notification slide-in becomes instant appearance. Loading spinners replaced with static "Loading..." text.

---

## 13. Visual Identity

### 13.1 Typography

- Primary font: Inter (Google Fonts). Weights: 400 (body), 500 (labels, nav), 600 (subheadings), 700 (headings).
- Monospace font: JetBrains Mono for SKU codes, PO numbers, and data table identifiers.
- Body text: 16px / 1.5 line height.
- H1: 32px / 1.2 line height / weight 700.
- H2: 24px / 1.3 line height / weight 600.
- H3: 20px / 1.3 line height / weight 600.
- Table cells: 14px / 1.4 line height / weight 400.
- Captions: 12px / 1.4 line height / weight 400 / color #6B7280.

### 13.2 Color Palette

- Background: #FFFFFF (primary), #F9FAFB (secondary/surface), #F3F4F6 (tertiary/hover).
- Text: #111827 (primary), #374151 (secondary), #6B7280 (tertiary/muted), #9CA3AF (disabled).
- Primary brand: #2563EB (buttons, links, active states). Hover: #1D4ED8. Light: #EFF6FF (badges, highlights).
- Success: #059669 (approved, completed). Light: #ECFDF5.
- Warning: #D97706 (pending, attention). Light: #FFFBEB.
- Error: #DC2626 (errors, rejected, disputes). Light: #FEF2F2.
- Border: #E5E7EB (default), #D1D5DB (focus/hover).

### 13.3 Spacing and Layout

- Base unit: 4px. All spacing multiples of 4px.
- Page padding: 32px horizontal on desktop, 24px on tablet, 16px on mobile.
- Card padding: 24px.
- Section gap: 32px.
- Component gap: 16px.
- Border radius: 8px for cards and containers, 6px for buttons and inputs, 4px for badges and tags.
- Max content width: 1280px, centered.

### 13.4 Component Styles

- Buttons: 40px height, 6px radius, 16px horizontal padding. Primary: filled #2563EB, white text. Secondary: outlined #E5E7EB, #374151 text. Danger: filled #DC2626, white text.
- Input fields: 40px height, 6px radius, 12px internal padding, 1px #E5E7EB border. Focus: 2px #2563EB border with 4px box-shadow in #2563EB at 10% opacity.
- Cards: white background, 1px #E5E7EB border, 8px radius, 24px padding. Hover: subtle shadow (0 4px 6px -1px rgba(0,0,0,0.1)).
- Status badges: 24px height, 12px font, 999px radius (pill shape). Color-coded backgrounds per status.
- Tables: 1px #E5E7EB horizontal dividers. Header row: #F9FAFB background, 500 weight. Hover row: #F3F4F6.

---

## 14. Page-by-Page Breakdown

### 14.1 Login Page

- Centered card (max-width 400px) on #F9FAFB background.
- Logo centered above form. Email input, password input, "Forgot password?" link, "Log in" primary button (full width), "Sign up" link below.
- MFA step: 6-digit input field with auto-submit on completion.

### 14.2 Buyer Dashboard

- Top bar: search input (full-width, prominent), notification bell, user avatar with dropdown.
- Sidebar: Dashboard (active), Suppliers, Catalog, RFQs, Purchase Orders, Bulk Orders, Analytics, Settings.
- Main area: 4 stat cards in a row (Active RFQs, Pending POs, Open Disputes, Spend This Month). Below: "Recent Activity" feed (last 10 actions), "Quick Actions" grid (Create RFQ, Bulk Order, View Suppliers).

### 14.3 Supplier Directory

- Search bar at top with filter toggle button.
- Active filters shown as removable chips below search.
- 3-column card grid on desktop, 2 on tablet, 1 on mobile.
- Each card: supplier logo, name, rating stars, category tag, location, verified badge, product count.
- Pagination at bottom.

### 14.4 RFQ Detail Page

- Header: RFQ title, status badge, created date, deadline countdown.
- Tabs: Line Items, Responses, Activity Log.
- Line Items tab: table with product name, SKU, quantity, target price, special instructions.
- Responses tab: comparison table (one column per supplier response). Highlight best price per line item in green.
- Award button on each response column. Disabled if RFQ is not in "open" status.

### 14.5 Purchase Order Detail Page

- Header: PO number (large, monospace), status badge, created date.
- Two-column layout: left column has line items table and totals; right column has supplier info, payment terms, shipping details.
- Status timeline: vertical stepper showing each status transition with timestamp and user who made the change.
- Action buttons contextual to status: Approve (if pending_approval), Acknowledge (if approved, supplier view), Ship (if acknowledged), Confirm Receipt (if shipped).

---

## 15. Third-Party Integrations

- Payment processing: Stripe for invoice payments (pay-online option on POs with "COD" or "Prepaid" terms).
- ERP sync: REST webhooks for PO creation, status updates, and receipt confirmation. Configurable per org.
- Email: SendGrid for transactional emails (notifications, verification, password reset).
- File storage: S3-compatible storage for product images, certifications, dispute evidence, and PO attachments.
- Analytics: Mixpanel for product analytics (user behavior, feature adoption).

---

## 16. Security

- All data encrypted in transit (TLS 1.3) and at rest (AES-256).
- SQL injection prevention via parameterized queries and ORM.
- XSS prevention via Content Security Policy headers and output encoding.
- CSRF protection via SameSite cookies and CSRF tokens on state-changing requests.
- Rate limiting: 100 requests/minute per user on standard endpoints, 20/minute on auth endpoints.
- File upload scanning: virus scan on upload, file type validation (whitelist), max 10MB per file.
- Audit logging: all data mutations logged with user, timestamp, before/after values. Logs retained for 7 years.
- Data residency: tenant data stored in region closest to org headquarters (US, EU, APAC).
