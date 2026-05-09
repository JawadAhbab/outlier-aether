# Public Utility Service Platform - Product Requirements Document

## 1. Overview

### 1.1 Purpose

The Public Utility Service Platform is a comprehensive web application that consolidates essential civic services into a single, accessible portal. This platform serves as a unified gateway for citizens to manage utility billing, school enrollment, library resources, transit schedules, and public health information.

### 1.2 Target Users

- **Citizens**: Primary users who access services for personal or family needs
- **Guests**: Unauthenticated visitors browsing public information
- **Service Administrators**: Municipal staff managing service operations
- **System Administrators**: IT personnel maintaining platform infrastructure

### 1.3 Core Value Proposition

This platform eliminates the need for citizens to navigate multiple separate agency websites by providing a centralized hub for all public utility services with consistent UX, unified account management, and streamlined service delivery.

---

## 2. Visual Identity

### 2.1 Typography

| Role | Font Family | Weight | Size |
|------|-------------|--------|------|
| Headings H1 | Source Sans Pro | 700 | 32px |
| Headings H2 | Source Sans Pro | 600 | 24px |
| Headings H3 | Source Sans Pro | 600 | 20px |
| Body Text | Source Sans Pro | 400 | 16px |
| Captions | Source Sans Pro | 400 | 14px |
| Buttons | Source Sans Pro | 600 | 16px |

### 2.2 Color Palette

| Purpose | Hex Value |
|---------|-----------|
| Primary Blue | #1E3A5F |
| Secondary Teal | #2A9D8F |
| Accent Orange | #E76F51 |
| Success Green | #4CAF50 |
| Warning Amber | #FF9800 |
| Error Red | #E53935 |
| Background Light | #F5F7FA |
| Background White | #FFFFFF |
| Text Primary | #212121 |
| Text Secondary | #757575 |
| Border Gray | #E0E0E0 |

### 2.3 Spacing System

- Base unit: 8px
- Small padding: 8px
- Medium padding: 16px
- Large padding: 24px
- Section spacing: 48px
- Border radius small: 4px
- Border radius medium: 8px
- Border radius large: 12px

### 2.4 Layout Grid

- Maximum content width: 1200px
- Column grid: 12-column with 24px gaps
- Card padding: 24px
- Page margin: 24px (desktop), 16px (mobile)

---

## 3. User Authentication

### 3.1 Authentication Methods

| Method | Description |
|--------|-------------|
| Email/Password | Standard login with email verification |
| Government ID | Citizen ID number for identity verification |
| Social Login | Google and Apple integration |

### 3.2 Auth Flow

1. User clicks "Sign In" button
2. System presents login options (Email, Government ID, Social)
3. User selects method and enters credentials
4. System validates credentials against user database
5. If valid, system creates session and redirects to dashboard
6. If invalid, system displays error message with retry option
7. Session persists for 30 days with activity refresh

### 3.3 Password Requirements

- Minimum 8 characters
- At least one uppercase letter
- At least one lowercase letter
- At least one number
- At least one special character

### 3.4 Session Management

- JWT tokens with 1-hour expiry
- Refresh tokens with 30-day expiry
- Automatic session extension on user activity
- Secure logout with token invalidation

---

## 4. User Roles and Permissions

### 4.1 Role Definitions

| Role | Access Level |
|------|--------------|
| Guest | Public information only |
| Citizen | Personal account and services |
| Service Admin | Department-specific management |
| System Admin | Full platform access |

### 4.2 Permission Matrix

| Feature | Guest | Citizen | Service Admin | System Admin |
|---------|-------|---------|---------------|--------------|
| View Service Status | Yes | Yes | Yes | Yes |
| Search Library Catalog | Yes | Yes | Yes | Yes |
| View Transit Schedules | Yes | Yes | Yes | Yes |
| Manage Billing | No | Yes | Yes | Yes |
| Submit Enrollment | No | Yes | Yes | Yes |
| Process Payments | No | Yes | Yes | Yes |
| Manage Outages | No | No | Yes | Yes |
| User Management | No | No | No | Yes |
| System Configuration | No | No | No | Yes |

---

## 5. Data Model

### 5.1 Core Entities

#### Users Table

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | PRIMARY KEY |
| email | VARCHAR(255) | UNIQUE, NOT NULL |
| password_hash | VARCHAR(255) | NOT NULL |
| government_id | VARCHAR(50) | UNIQUE |
| first_name | VARCHAR(100) | NOT NULL |
| last_name | VARCHAR(100) | NOT NULL |
| phone | VARCHAR(20) | |
| role | ENUM | DEFAULT 'citizen' |
| created_at | TIMESTAMP | DEFAULT NOW() |
| updated_at | TIMESTAMP | DEFAULT NOW() |
| last_login | TIMESTAMP | |

#### Addresses Table

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | PRIMARY KEY |
| user_id | UUID | FOREIGN KEY (users) |
| street_address | VARCHAR(255) | NOT NULL |
| city | VARCHAR(100) | NOT NULL |
| state | VARCHAR(50) | NOT NULL |
| zip_code | VARCHAR(20) | NOT NULL |
| is_primary | BOOLEAN | DEFAULT FALSE |

#### Utility Accounts Table

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | PRIMARY KEY |
| user_id | UUID | FOREIGN KEY (users) |
| account_number | VARCHAR(50) | UNIQUE, NOT NULL |
| service_type | ENUM('water', 'electric', 'gas', 'waste') | NOT NULL |
| status | ENUM('active', 'inactive', 'pending') | DEFAULT 'pending' |
| service_address | UUID | FOREIGN KEY (addresses) |
| balance | DECIMAL(10,2) | DEFAULT 0.00 |
| created_at | TIMESTAMP | DEFAULT NOW() |

#### Billing History Table

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | PRIMARY KEY |
| account_id | UUID | FOREIGN KEY (utility_accounts) |
| billing_period | VARCHAR(20) | NOT NULL |
| amount | DECIMAL(10,2) | NOT NULL |
| due_date | DATE | NOT NULL |
| status | ENUM('pending', 'paid', 'overdue') | DEFAULT 'pending' |
| paid_at | TIMESTAMP | |
| payment_method | VARCHAR(50) | |
| created_at | TIMESTAMP | DEFAULT NOW() |

#### Payments Table

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | PRIMARY KEY |
| user_id | UUID | FOREIGN KEY (users) |
| account_id | UUID | FOREIGN KEY (utility_accounts) |
| billing_id | UUID | FOREIGN KEY (billing_history) |
| amount | DECIMAL(10,2) | NOT NULL |
| payment_type | ENUM('full', 'partial') | NOT NULL |
| transaction_id | VARCHAR(100) | UNIQUE |
| payment_method | VARCHAR(50) | NOT NULL |
| status | ENUM('processing', 'completed', 'failed') | DEFAULT 'processing' |
| processed_at | TIMESTAMP | |
| created_at | TIMESTAMP | DEFAULT NOW() |

#### Service Outages Table

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | PRIMARY KEY |
| service_type | ENUM('water', 'electric', 'gas', 'waste', 'transit') | NOT NULL |
| title | VARCHAR(255) | NOT NULL |
| description | TEXT | |
| affected_area | GEOMETRY | |
| latitude | DECIMAL(10,8) | |
| longitude | DECIMAL(11,8) | |
| status | ENUM('investigating', 'confirmed', 'resolved') | DEFAULT 'investigating' |
| reported_at | TIMESTAMP | |
| resolved_at | TIMESTAMP | |
| created_at | TIMESTAMP | DEFAULT NOW() |

#### School Enrollments Table

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | PRIMARY KEY |
| user_id | UUID | FOREIGN KEY (users) |
| student_first_name | VARCHAR(100) | NOT NULL |
| student_last_name | VARCHAR(100) | NOT NULL |
| date_of_birth | DATE | NOT NULL |
| grade_level | INTEGER | NOT NULL |
| school_id | UUID | FOREIGN KEY (schools) |
| status | ENUM('draft', 'submitted', 'under_review', 'approved', 'rejected') | DEFAULT 'draft' |
| submitted_at | TIMESTAMP | |
| created_at | TIMESTAMP | DEFAULT NOW() |

#### Schools Table

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | PRIMARY KEY |
| name | VARCHAR(255) | NOT NULL |
| address | UUID | FOREIGN KEY (addresses) |
| district | VARCHAR(255) | |
| grade_levels | VARCHAR(50) | |
| enrollment_capacity | INTEGER | |
| current_enrollment | INTEGER | |

#### Library Holdings Table

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | PRIMARY KEY |
| title | VARCHAR(255) | NOT NULL |
| author | VARCHAR(255) | |
| isbn | VARCHAR(20) | UNIQUE |
| publisher | VARCHAR(255) | |
| publication_year | INTEGER | |
| format | ENUM('book', 'ebook', 'audiobook', 'dvd', 'magazine') | NOT NULL |
| category | VARCHAR(100) | |
| total_copies | INTEGER | DEFAULT 1 |
| available_copies | INTEGER | DEFAULT 1 |
| cover_image_url | VARCHAR(500) | |

#### Library Borrows Table

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | PRIMARY KEY |
| user_id | UUID | FOREIGN KEY (users) |
| holding_id | UUID | FOREIGN KEY (library_holdings) |
| borrow_date | DATE | NOT NULL |
| due_date | DATE | NOT NULL |
| return_date | DATE | |
| status | ENUM('borrowed', 'returned', 'overdue') | DEFAULT 'borrowed' |

#### Transit Schedules Table

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | PRIMARY KEY |
| route_name | VARCHAR(100) | NOT NULL |
| route_number | VARCHAR(20) | |
| service_type | ENUM('bus', 'rail', 'ferry', 'subway') | NOT NULL |
| direction | ENUM('inbound', 'outbound') | NOT NULL |
| stop_name | VARCHAR(255) | NOT NULL |
| stop_latitude | DECIMAL(10,8) | |
| stop_longitude | DECIMAL(11,8) | |
| arrival_time | TIME | NOT NULL |
| day_type | ENUM('weekday', 'saturday', 'sunday') | NOT NULL |

#### Notifications Table

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | PRIMARY KEY |
| user_id | UUID | FOREIGN KEY (users) |
| type | ENUM('billing', 'outage', 'enrollment', 'library', 'transit') | NOT NULL |
| title | VARCHAR(255) | NOT NULL |
| message | TEXT | NOT NULL |
| is_read | BOOLEAN | DEFAULT FALSE |
| action_url | VARCHAR(500) | |
| created_at | TIMESTAMP | DEFAULT NOW() |

### 5.2 Entity Relationships

```
Users (1) ----< (N) Addresses
Users (1) ----< (N) Utility Accounts
Users (1) ----< (N) Payments
Users (1) ----< (N) School Enrollments
Users (1) ----< (N) Library Borrows
Users (1) ----< (N) Notifications
Utility Accounts (1) ----< (N) Billing History
Utility Accounts (1) ----< (N) Payments
Billing History (1) ----< (N) Payments
Schools (1) ----< (N) School Enrollments
Library Holdings (1) ----< (N) Library Borrows
Transit Schedules: Route-based grouping
Service Outages: Geographic clustering
```

---

## 6. Pages and Interactions

### 6.1 Landing Page (Public)

**Layout Structure:**
- Hero section with service category cards (full-width)
- Quick action buttons for common tasks
- Service status summary banner
- Footer with contact information and links

**Key Elements:**
- Service category grid: 3 columns on desktop, 1 column on mobile
- Status indicator pills showing active/outage services
- "Sign In" button in header, "Register" button prominent

**Interactions:**
- Click category card -> Navigate to service hub
- Click status indicator -> Expand service status panel
- Scroll triggers subtle parallax on hero background

### 6.2 User Dashboard

**Layout Structure:**
- Welcome header with user name and account summary
- Quick action cards for each service category
- Recent notifications panel
- Payment due summary widget

**Key Elements:**
- 4-card grid layout for service categories
- Notification list with unread count badge
- Balance due prominent display with "Pay Now" CTA

**Interactions:**
- Click notification -> Mark as read and navigate to related service
- Click service card -> Navigate to detailed service view
- Click "Pay All" -> Process payment for all outstanding balances

### 6.3 Billing Dashboard

**Layout Structure:**
- Account selector dropdown (if multiple accounts)
- Current balance card with due date
- Payment history timeline
- Payment form section

**Key Elements:**
- Balance card: amount due, due date, minimum payment
- Transaction table: sortable by date, filterable by status
- Payment form: amount input, method selection, confirmation

**User Flow - Pay a Bill:**
1. User navigates to Billing Dashboard
2. System displays all linked utility accounts
3. User selects account from dropdown
4. System shows current balance and billing history
5. User clicks "Pay" on specific bill
6. System presents payment form with pre-filled amount
7. User selects payment method or adds new card
8. User confirms payment amount
9. User clicks "Submit Payment"
10. System processes payment via payment gateway
11. System updates bill status to "paid"
12. System sends confirmation notification
13. System displays success message with transaction ID

### 6.4 Service Outage Map

**Layout Structure:**
- Interactive map (full-width, 70vh height)
- Filter sidebar with service type toggles
- Active outage list panel (collapsible)
- Legend for map markers

**Key Elements:**
- Map markers color-coded by service type and status
- Cluster markers when zoomed out
- Detail popup on marker click
- Filter checkboxes for each service type

**User Flow - View Outage:**
1. User navigates to Outage Map
2. System loads map centered on user's area
3. System displays all active outages as markers
4. User clicks marker cluster to zoom in
5. User clicks individual marker
6. System displays popup with outage details
7. User clicks "Notify Me" to subscribe to updates
8. System saves notification preference

### 6.5 Payment Enrollment Form

**Layout Structure:**
- Multi-step form wizard
- Progress indicator (4 steps)
- Form sections that animate between steps
- Summary panel on final step

**Steps:**

**Step 1 - Service Selection:**
- Checkbox list of available services at user's address
- Address confirmation or correction

**Step 2 - Account Verification:**
- Input field for existing account number (optional)
- Option to create new account

**Step 3 - Payment Method:**
- Saved payment methods dropdown
- Add new card/bank account form
- Set as default payment method checkbox

**Step 4 - Review and Confirm:**
- Summary of all entered information
- Terms and conditions checkbox
- Submit button

**User Flow - Enroll in Auto-Pay:**
1. User navigates to Payment Enrollment
2. User selects services to enroll
3. User verifies or updates service address
4. User enters existing account number (or skips for new)
5. User selects or adds payment method
6. User reviews information and accepts terms
7. User clicks "Complete Enrollment"
8. System validates all information
9. System creates payment enrollment record
10. System sends confirmation email
11. System redirects to billing dashboard

### 6.6 Transit Schedule Search

**Layout Structure:**
- Search bar with route/stop autocomplete
- Filter panel: service type, day of week, time range
- Results list with real-time countdown
- Map view toggle

**Key Elements:**
- Search input with typeahead suggestions
- Filter chips showing active filters
- Route cards with next arrival times
- "Favorites" star icon on each route

**User Flow - Find Next Bus:**
1. User enters stop name or route number in search
2. System provides autocomplete suggestions
3. User selects stop or route from list
4. System displays schedule for selected stop/route
5. User toggles favorite by clicking star icon
6. System saves favorite to user's profile
7. User clicks "Get Directions" on route card
8. System opens map application with directions

### 6.7 Library Catalog Search

**Layout Structure:**
- Search bar with advanced search toggle
- Faceted filter sidebar: format, availability, category
- Results grid: 4 columns desktop, 2 columns tablet, 1 column mobile
- Pagination with 20 items per page

**Key Elements:**
- Search input with genre suggestions
- Filter checkboxes with result counts
- Item cards with cover image, title, author, availability badge
- "Borrow" button on each card

**User Flow - Borrow a Book:**
1. User searches by title, author, or ISBN
2. System displays matching results with availability
3. User filters by format (e.g., audiobook)
4. User clicks on item card
5. System displays detailed item view with description
6. User clicks "Borrow" button
7. System checks user borrowing limits (max 10 items)
8. System creates borrow record with due date (14 days)
9. System decrements available copies
10. System displays borrow confirmation with due date
11. System adds item to user's "My Borrowing" list

---

## 7. API Design

### 7.1 Authentication Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | /api/v1/auth/register | Create new user account |
| POST | /api/v1/auth/login | Authenticate user |
| POST | /api/v1/auth/logout | Invalidate session |
| POST | /api/v1/auth/refresh | Refresh access token |
| POST | /api/v1/auth/forgot-password | Send password reset email |
| POST | /api/v1/auth/reset-password | Reset password with token |
| GET | /api/v1/auth/me | Get current user profile |

### 7.2 Billing Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/v1/accounts | Get user's utility accounts |
| GET | /api/v1/accounts/{id} | Get account details |
| GET | /api/v1/accounts/{id}/billing | Get billing history |
| GET | /api/v1/billing/{id} | Get specific bill details |
| POST | /api/v1/payments | Process new payment |
| GET | /api/v1/payments | Get payment history |
| GET | /api/v1/payments/{id} | Get payment details |
| POST | /api/v1/enrollments | Create payment enrollment |
| DELETE | /api/v1/enrollments/{id} | Cancel auto-pay enrollment |

### 7.3 Service Status Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/v1/outages | Get all active outages |
| GET | /api/v1/outages/{id} | Get specific outage details |
| GET | /api/v1/outages/nearby | Get outages near coordinates |
| POST | /api/v1/outages/{id}/subscribe | Subscribe to outage notifications |

### 7.4 School Enrollment Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/v1/schools | Get list of schools |
| GET | /api/v1/schools/{id} | Get school details |
| GET | /api/v1/enrollments | Get user's enrollments |
| POST | /api/v1/enrollments | Submit new enrollment |
| GET | /api/v1/enrollments/{id} | Get enrollment details |
| PUT | /api/v1/enrollments/{id} | Update enrollment (draft only) |
| DELETE | /api/v1/enrollments/{id} | Delete enrollment (draft only) |

### 7.5 Library Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/v1/library/search | Search catalog |
| GET | /api/v1/library/items/{id} | Get item details |
| GET | /api/v1/library/my-borrows | Get user's borrowed items |
| POST | /api/v1/library/borrow | Borrow an item |
| POST | /api/v1/library/return | Return an item |
| GET | /api/v1/library/favorites | Get user's favorite items |

### 7.6 Transit Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/v1/transit/routes | Get all routes |
| GET | /api/v1/transit/routes/{id} | Get route details |
| GET | /api/v1/transit/stops | Search stops |
| GET | /api/v1/transit/schedules | Get schedules for stop/route |
| GET | /api/v1/transit/favorites | Get user's favorite routes/stops |
| POST | /api/v1/transit/favorites | Add to favorites |
| DELETE | /api/v1/transit/favorites/{id} | Remove from favorites |

### 7.7 Notifications Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/v1/notifications | Get user's notifications |
| PUT | /api/v1/notifications/{id}/read | Mark as read |
| PUT | /api/v1/notifications/read-all | Mark all as read |
| DELETE | /api/v1/notifications/{id} | Delete notification |

---

## 8. Error Handling

### 8.1 Error Response Format

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Human-readable error message",
    "details": [
      {
        "field": "email",
        "message": "Invalid email format"
      }
    ],
    "request_id": "req_abc123xyz"
  }
}
```

### 8.2 Error Codes

| Code | HTTP Status | Description |
|------|-------------|-------------|
| VALIDATION_ERROR | 400 | Input validation failed |
| UNAUTHORIZED | 401 | Authentication required |
| FORBIDDEN | 403 | Insufficient permissions |
| NOT_FOUND | 404 | Resource not found |
| CONFLICT | 409 | Resource conflict (e.g., duplicate) |
| RATE_LIMITED | 429 | Too many requests |
| INTERNAL_ERROR | 500 | Server error |

### 8.3 Empty States

| Scenario | Display Message | Action |
|----------|------------------|--------|
| No utility accounts | "You have no services enrolled. Add a service to get started." | "Add Service" button |
| No billing history | "No billing records found. Your first bill will appear here." | None |
| No search results | "No items match your search. Try different keywords or remove filters." | "Clear Filters" button |
| No enrollments | "No school enrollments yet. Start an enrollment application." | "New Enrollment" button |
| No borrowed items | "You have no borrowed items. Browse the catalog to borrow." | "Browse Catalog" button |
| No notifications | "You're all caught up! No new notifications." | None |

### 8.4 User-Facing Error Messages

| Scenario | Message |
|----------|---------|
| Login failed | "Incorrect email or password. Please try again." |
| Payment failed | "Payment could not be processed. Please check your payment method or try again." |
| Enrollment not found | "This enrollment application could not be found." |
| Item not available | "This item is currently unavailable. Join the waitlist or try another format." |
| Session expired | "Your session has expired. Please sign in again." |
| Network error | "Connection error. Please check your internet connection and try again." |

---

## 9. Real-Time Features

### 9.1 Notification System

- WebSocket connection for real-time notifications
- Notification badge updates without page refresh
- Sound alert option (user preference)
- Push notifications via browser API (opt-in)

### 9.2 Live Transit Updates

- WebSocket for real-time transit arrivals
- Automatic refresh every 30 seconds
- "Arriving now" highlight when < 5 minutes
- Service disruption alerts

### 9.3 Outage Map Updates

- Polling every 60 seconds for outage changes
- Marker color transition on status change
- Automatic notification on subscribed outage updates

---

## 10. Responsive Behavior

### 10.1 Breakpoints

| Name | Width | Description |
|------|-------|-------------|
| Mobile | < 768px | Smartphones |
| Tablet | 768px - 1024px | Tablets |
| Desktop | > 1024px | Desktop computers |

### 10.2 Mobile-Specific Changes

- Hamburger menu replaces top navigation
- Card grids: 1 column (mobile), 2 columns (tablet), 4 columns (desktop)
- Map: Full-screen toggle on mobile
- Forms: Single column layout
- Buttons: Full-width on mobile
- Tables: Horizontal scroll with sticky first column

### 10.3 Heavy Visual Handling

| Feature | Desktop | Mobile |
|---------|---------|--------|
| Outage Map | Full map with all markers | Reduced marker density, clustering |
| Transit Map | Real-time updates | Updates on manual refresh |
| Library Cover Images | Lazy-loaded high-res | Low-res placeholders until scroll |

---

## 11. Performance Requirements

### 11.1 Metrics

| Metric | Target |
|--------|--------|
| First Contentful Paint | < 1.5s |
| Time to Interactive | < 3.0s |
| Largest Contentful Paint | < 2.5s |
| Page load time | < 3.0s |
| API response time | < 500ms |
| Map interaction latency | < 100ms |

### 11.2 Asset Budget

| Asset Type | Budget |
|------------|--------|
| Initial HTML | < 100KB |
| CSS (critical) | < 50KB |
| JavaScript (critical) | < 150KB |
| Images (per page) | < 500KB total |
| Fonts | < 200KB |

### 11.3 Techniques

- Image lazy loading with blur-up placeholders
- Code splitting by route
- Service Worker for offline capability on static pages
- CDN for static assets
- API response caching (5 minutes for read operations)
- Database query optimization with proper indexing

---

## 12. Accessibility

### 12.1 WCAG 2.1 AA Compliance

- All interactive elements keyboard accessible
- Focus indicators visible on all focusable elements
- ARIA labels on icons and buttons without text
- Alt text for all images
- Color contrast ratio minimum 4.5:1 for text

### 12.2 prefers-reduced-motion

When user prefers reduced motion:
- Disable parallax scrolling effects
- Replace animations with instant transitions
- Remove auto-playing carousels (static display)
- Disable ambient background animations
- Map markers: no bounce animation, simple appearance

### 12.3 Screen Reader Support

- Semantic HTML structure
- Skip to main content link
- ARIA landmarks for main, navigation, content
- Live regions for dynamic content updates
- Form labels properly associated with inputs

---

## 13. Admin Features

### 13.1 Service Admin Capabilities

- View and manage department-specific outages
- Create and update outage reports
- View enrollment applications for department
- Approve or reject enrollment applications
- View department-specific billing reports

### 13.2 System Admin Capabilities

- All Service Admin capabilities
- User management (view, deactivate, delete)
- System-wide configuration settings
- Audit log access
- API key management
- Database backup and restore

### 13.3 Admin Dashboard

- Separate admin portal with different authentication
- Service overview cards with key metrics
- Queue lists for pending approvals
- Activity logs with filtering
- Export functionality for reports

---

## 14. Security

### 14.1 Data Protection

- All data encrypted in transit (TLS 1.3)
- Passwords hashed with bcrypt (cost factor 12)
- Sensitive fields encrypted at rest
- Session tokens cryptographically secure

### 14.2 Input Validation

- Server-side validation on all endpoints
- SQL injection prevention via parameterized queries
- XSS prevention via output encoding
- CSRF tokens on all state-changing operations

### 14.3 Rate Limiting

- Login attempts: 5 per minute per IP
- API requests: 100 per minute per user
- Search queries: 30 per minute per user
- Payment submissions: 3 per minute per user

---

## 15. Localization

- Default language: English (en-US)
- Date format: MM/DD/YYYY
- Time format: 12-hour with AM/PM
- Currency: USD
- Phone format: (XXX) XXX-XXXX
