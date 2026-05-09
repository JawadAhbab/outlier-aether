# Procurement Platform - Product Requirements Document

## 1. Project Overview

### Project Name
ProSource - Enterprise B2B Procurement Platform

### Core Functionality
A comprehensive B2B purchasing platform enabling businesses to discover suppliers, request quotes, manage bulk orders, and streamline procurement workflows at enterprise scale.

### Target Users
- Procurement managers at mid-to-large enterprises
- Supply chain administrators
- Purchasing agents and buyers
- Supplier account managers
- Guest visitors researching suppliers and products

---

## 2. User Roles and Authentication

### User Roles

| Role | Description | Access Level |
|------|-------------|--------------|
| Guest | Unauthenticated browser | Public catalog, supplier discovery, limited product viewing |
| Buyer | Registered business user | Full catalog access, RFQ creation, order placement, order tracking |
| Supplier Admin | Supplier company administrator | Product management, quote responses, order fulfillment, analytics |
| Platform Admin | Internal platform staff | User management, platform analytics, supplier verification, compliance |

### Authentication Methods

| Method | Use Case | Flow |
|--------|----------|------|
| Email/Password | Standard login | Enter email, password, verify credentials, create session |
| Enterprise SSO | Corporate users | Redirect to IdP (Okta/Azure AD), receive SAML assertion, create session |
| Magic Link | Password-free login | Enter email, receive link, click link within 15 minutes, create session |
| MFA | High-security operations | After password verification, prompt for 6-digit TOTP code |

### Session Management
- Session tokens stored as HTTP-only secure cookies
- Session duration: 8 hours active, 30 days remember me
- Concurrent session limit: 3 per user
- Session invalidation on password change or logout from all devices

---

## 3. User Flows and Features

### 3.1 Guest Visitor Flow

1. Visitor lands on homepage
2. Can browse public supplier directory
3. Can search products by keyword, category, or supplier
4. Can view supplier profiles with certifications
5. Can view product listings with pricing tiers (exact prices hidden, show "Login for pricing")
6. Can filter by MOQ, certification, location, rating
7. Can sort by relevance, rating, newest, price range

### 3.2 Buyer Registration Flow

1. Click "Register" button
2. Select account type: Buyer or Supplier
3. Enter business email, create password (min 12 chars, uppercase, number, special)
4. Verify email via 6-digit code (expires in 10 minutes)
5. Complete company profile: name, address, tax ID, industry
6. Submit for verification (manual review within 24 hours)
7. Receive approval email with buyer dashboard access

### 3.3 Product Search and Discovery Flow

1. User enters search query in header search bar
2. Autocomplete suggests matching products and suppliers (debounce 300ms)
3. Search results page displays with:
   - Product cards in 4-column grid (desktop), 2-column (tablet), 1-column (mobile)
   - 40px gap between cards
   - Infinite scroll with 50 items per page
4. Filter sidebar with categories:
   - Category (hierarchical tree, max 3 levels deep)
   - Price range (min/max inputs)
   - MOQ range (slider: 1 to 10,000+)
   - Certifications (multi-select checkboxes)
   - Supplier location (country dropdown)
   - Rating (1-5 stars)
5. Sort options: Relevance, Price Low-High, Price High-Low, MOQ Low-High, Rating, Newest
6. Active filters shown as removable chips above results
7. Empty state: "No products match your filters" with suggestion to clear filters

### 3.4 Request for Quote (RFQ) Workflow

#### Step 1: Initiate RFQ
1. Buyer clicks "Request Quote" on product page
2. Form displays product details (name, SKU, base unit)
3. Enter quantity required (number input with validation)
4. Select unit of measure from dropdown (pcs, kg, tons, pallets)
5. Enter target price per unit (optional)
6. Enter required delivery date (date picker, min 14 days from today)
7. Enter shipping destination (address autocomplete)
8. Click "Continue to Requirements"

#### Step 2: Requirements Details
1. Enter detailed requirements (rich text, max 2000 chars)
2. Upload attachments (max 5 files, 10MB each, PDF/PNG/JPG)
3. Select preferred supplier criteria:
   - Any verified supplier
   - Suppliers with specific certifications (multi-select)
   - Preferred suppliers from saved list
4. Set quote deadline (date picker, min 3 days from now)
5. Click "Continue to Review"

#### Step 3: Review and Submit
1. Summary page shows all entered information
2. Edit buttons link back to each section
3. Checkbox: "I agree to RFQ terms and conditions"
4. Click "Submit RFQ"
5. Confirmation page with RFQ number (format: RFQ-2026-XXXXX)
6. Email notification sent to matching suppliers

#### Step 4: Quote Reception and Comparison
1. Supplier receives RFQ notification email
2. Supplier logs in, views RFQ in dashboard
3. Supplier submits quote with: unit price, total price, validity period, lead time, payment terms, notes
4. Buyer receives email when quote arrives
5. Buyer views all received quotes in RFQ detail page
6. Quote comparison table shows side-by-side columns: Supplier, Unit Price, Total, Lead Time, Payment Terms, Rating
7. Buyer can request clarification via message thread on each quote
8. Buyer accepts one quote, rejects others with optional feedback

### 3.5 Bulk Order Placement Flow

1. Buyer adds products to cart with specified quantities
2. Cart page displays items with:
   - Product name, SKU, supplier
   - Unit price (based on quantity tier)
   - Quantity input (validated against MOQ)
   - Line total
   - Remove button
3. Minimum order value check: $500 minimum, show warning if below
4. Checkout button disabled until minimum met
5. Shipping address selection (saved addresses or add new)
6. Payment method selection:
   - Corporate PO (enter PO number, subject to approval)
   - Credit card (Stripe integration)
   - Net 30 terms (subject to credit approval)
7. Order review page with final totals
8. Place order button creates purchase order
9. Confirmation page with order number (format: PO-2026-XXXXX)
10. Email confirmation sent to buyer and supplier

### 3.6 Order Tracking Flow

1. Buyer navigates to "My Orders" page
2. Order list displays with columns: Order #, Date, Supplier, Items, Total, Status
3. Status values: Pending, Confirmed, Processing, Shipped, Delivered, Cancelled
4. Click order to view details:
   - Line items with quantities and prices
   - Shipping address and tracking numbers
   - Supplier contact information
   - Order timeline showing all status changes
5. Status changes trigger email notifications
6. Buyer can cancel order if status is "Pending" (confirmation modal)
7. Buyer can leave review for supplier after delivery

### 3.7 Supplier Profile Management Flow

1. Supplier admin logs in to supplier dashboard
2. Navigate to "Company Profile" section
3. Edit company information:
   - Company name, logo, cover image
   - Description (rich text, max 3000 chars)
   - Address, phone, email, website
   - Year established, employee count range
4. Manage certifications:
   - Add certification: name, issuing body, issue date, expiry date, document upload
   - Certifications display as badges on profile
5. Manage product catalog:
   - Add product: name, SKU, description, category, images (max 10), specs
   - Set pricing tiers: quantity breaks (e.g., 1-99: $10, 100-499: $9, 500+: $8)
   - Set MOQ per product
   - Set inventory levels or "Made to Order"
   - Enable/disable products
6. View analytics: page views, quote requests, order conversion rate

---

## 4. Data Model

### Entity Relationship Overview

```
Users (1) ----< (M) CompanyAddresses
Users (1) ----< (M) UserCompanyRoles >---- (1) Companies
Users (1) ----< (M) Sessions
Companies (1) ----< (M) Products
Companies (1) ----< (M) Certifications
Companies (1) ----< (M) PurchaseOrders >---- (M) PurchaseOrderItems
Companies (1) ----< (M) RequestForQuotes >---- (M) QuoteResponses
Products (1) ----< (M) ProductPricingTiers
Products (M) ----< (M) ProductCategories
Products (M) ----< (M) ProductImages
Categories (1) ----< (M) Categories (self-referential for hierarchy)
```

### Data Tables

#### Users

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | UUID | PK, auto-generated | Unique identifier |
| email | VARCHAR(255) | UNIQUE, NOT NULL | Login email |
| password_hash | VARCHAR(255) | NOT NULL | Argon2 hashed password |
| first_name | VARCHAR(100) | NOT NULL | User first name |
| last_name | VARCHAR(100) | NOT NULL | User last name |
| phone | VARCHAR(20) | NULL | Contact phone |
| avatar_url | VARCHAR(500) | NULL | Profile image URL |
| email_verified | BOOLEAN | DEFAULT false | Email verification status |
| mfa_enabled | BOOLEAN | DEFAULT false | Two-factor enabled |
| created_at | TIMESTAMP | DEFAULT NOW() | Account creation time |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last update time |

#### Companies

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | UUID | PK, auto-generated | Unique identifier |
| name | VARCHAR(255) | NOT NULL | Company legal name |
| display_name | VARCHAR(255) | NOT NULL | Public facing name |
| slug | VARCHAR(100) | UNIQUE, NOT NULL | URL-friendly name |
| type | ENUM | 'buyer', 'supplier', 'both' | Company type |
| tax_id | VARCHAR(50) | NULL | Tax identification number |
| website | VARCHAR(500) | NULL | Company website |
| logo_url | VARCHAR(500) | NULL | Company logo |
| cover_image_url | VARCHAR(500) | NULL | Profile cover image |
| description | TEXT | NULL | Company description |
| address_line1 | VARCHAR(255) | NULL | Street address |
| address_line2 | VARCHAR(255) | NULL | Suite/building |
| city | VARCHAR(100) | NULL | City |
| state | VARCHAR(100) | NULL | State/province |
| country | VARCHAR(100) | NOT NULL | Country |
| postal_code | VARCHAR(20) | NULL | ZIP/postal code |
| phone | VARCHAR(20) | NULL | Contact phone |
| verification_status | ENUM | 'pending', 'verified', 'rejected' | KYC status |
| credit_limit | DECIMAL(15,2) | DEFAULT 0 | Net 30 credit limit |
| created_at | TIMESTAMP | DEFAULT NOW() | Registration time |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last update time |

#### Products

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | UUID | PK, auto-generated | Unique identifier |
| company_id | UUID | FK to Companies, NOT NULL | Supplier company |
| sku | VARCHAR(100) | NOT NULL | Stock keeping unit |
| name | VARCHAR(255) | NOT NULL | Product name |
| slug | VARCHAR(255) | NOT NULL | URL-friendly name |
| description | TEXT | NULL | Product description |
| category_id | UUID | FK to Categories | Primary category |
| moq | INTEGER | DEFAULT 1 | Minimum order quantity |
| unit | VARCHAR(50) | DEFAULT 'pcs' | Unit of measure |
| inventory_status | ENUM | 'in_stock', 'low_stock', 'made_to_order', 'out_of_stock' | Stock availability |
| inventory_count | INTEGER | DEFAULT 0 | Available quantity |
| lead_time_days | INTEGER | DEFAULT 7 | Production lead time |
| is_active | BOOLEAN | DEFAULT true | Visibility toggle |
| created_at | TIMESTAMP | DEFAULT NOW() | Creation time |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last update time |

#### Product Pricing Tiers

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | UUID | PK, auto-generated | Unique identifier |
| product_id | UUID | FK to Products, NOT NULL | Related product |
| min_quantity | INTEGER | NOT NULL | Minimum quantity for this tier |
| max_quantity | INTEGER | NULL | Maximum quantity (NULL = unlimited) |
| unit_price | DECIMAL(15,2) | NOT NULL | Price per unit |
| currency | VARCHAR(3) | DEFAULT 'USD' | Currency code |
| created_at | TIMESTAMP | DEFAULT NOW() | Creation time |

#### Product Categories

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | UUID | PK, auto-generated | Unique identifier |
| parent_id | UUID | FK to Categories, NULL | Parent category |
| name | VARCHAR(100) | NOT NULL | Category name |
| slug | VARCHAR(100) | NOT NULL | URL-friendly name |
| description | TEXT | NULL | Category description |
| icon | VARCHAR(100) | NULL | Icon identifier |
| sort_order | INTEGER | DEFAULT 0 | Display order |

#### Certifications

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | UUID | PK, auto-generated | Unique identifier |
| company_id | UUID | FK to Companies, NOT NULL | Company |
| name | VARCHAR(255) | NOT NULL | Certification name |
| issuing_body | VARCHAR(255) | NOT NULL | Certifying authority |
| issue_date | DATE | NOT NULL | Date issued |
| expiry_date | DATE | NULL | Expiration date |
| document_url | VARCHAR(500) | NULL | Certificate PDF |
| status | ENUM | 'active', 'expired', 'pending' | Verification status |
| created_at | TIMESTAMP | DEFAULT NOW() | Creation time |

#### Purchase Orders

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | UUID | PK, auto-generated | Unique identifier |
| order_number | VARCHAR(20) | UNIQUE, NOT NULL | Human-readable PO number |
| buyer_company_id | UUID | FK to Companies, NOT NULL | Buying company |
| supplier_company_id | UUID | FK to Companies, NOT NULL | Supplying company |
| status | ENUM | 'pending', 'confirmed', 'processing', 'shipped', 'delivered', 'cancelled' | Order status |
| subtotal | DECIMAL(15,2) | NOT NULL | Sum of line items |
| shipping_cost | DECIMAL(15,2) | DEFAULT 0 | Shipping charge |
| tax_amount | DECIMAL(15,2) | DEFAULT 0 | Tax amount |
| total | DECIMAL(15,2) | NOT NULL | Grand total |
| currency | VARCHAR(3) | DEFAULT 'USD' | Currency |
| shipping_address_id | UUID | FK to CompanyAddresses | Delivery address |
| po_number | VARCHAR(100) | NULL | Customer PO reference |
| payment_method | ENUM | 'credit_card', 'wire_transfer', 'net30' | Payment type |
| payment_status | ENUM | 'pending', 'paid', 'overdue', 'cancelled' | Payment status |
| notes | TEXT | NULL | Order notes |
| tracking_number | VARCHAR(255) | NULL | Shipping tracking |
| shipped_at | TIMESTAMP | NULL | Shipment time |
| delivered_at | TIMESTAMP | NULL | Delivery time |
| created_at | TIMESTAMP | DEFAULT NOW() | Order creation time |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last update time |

#### Purchase Order Items

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | UUID | PK, auto-generated | Unique identifier |
| purchase_order_id | UUID | FK to PurchaseOrders, NOT NULL | Parent order |
| product_id | UUID | FK to Products, NOT NULL | Product |
| sku | VARCHAR(100) | NOT NULL | SKU at time of order |
| product_name | VARCHAR(255) | NOT NULL | Name at time of order |
| quantity | INTEGER | NOT NULL | Ordered quantity |
| unit | VARCHAR(50) | NOT NULL | Unit at time of order |
| unit_price | DECIMAL(15,2) | NOT NULL | Price per unit |
| line_total | DECIMAL(15,2) | NOT NULL | quantity x unit_price |

#### Request for Quotes

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | UUID | PK, auto-generated | Unique identifier |
| rfq_number | VARCHAR(20) | UNIQUE, NOT NULL | Human-readable RFQ number |
| buyer_company_id | UUID | FK to Companies, NOT NULL | Requesting company |
| status | ENUM | 'open', 'quotes_received', 'awarded', 'cancelled', 'expired' | RFQ status |
| title | VARCHAR(255) | NOT NULL | RFQ title/subject |
| quantity | INTEGER | NOT NULL | Requested quantity |
| unit | VARCHAR(50) | NOT NULL | Unit of measure |
| target_price | DECIMAL(15,2) | NULL | Target unit price |
| delivery_date | DATE | NOT NULL | Required delivery date |
| shipping_address_id | UUID | FK to CompanyAddresses | Delivery address |
| requirements | TEXT | NULL | Detailed requirements |
| deadline | TIMESTAMP | NOT NULL | Quote submission deadline |
| awarded_at | TIMESTAMP | NULL | Winning quote selected time |
| created_at | TIMESTAMP | DEFAULT NOW() | Creation time |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last update time |

#### Quote Responses

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | UUID | PK, auto-generated | Unique identifier |
| rfq_id | UUID | FK to RequestForQuotes, NOT NULL | Related RFQ |
| supplier_company_id | UUID | FK to Companies, NOT NULL | Quoting company |
| status | ENUM | 'submitted', 'viewed', 'accepted', 'rejected', 'withdrawn' | Quote status |
| unit_price | DECIMAL(15,2) | NOT NULL | Quoted unit price |
| total_price | DECIMAL(15,2) | NOT NULL | Calculated total |
| currency | VARCHAR(3) | DEFAULT 'USD' | Currency |
| lead_time_days | INTEGER | NOT NULL | Production lead time |
| validity_days | INTEGER | DEFAULT 30 | Quote validity |
| payment_terms | VARCHAR(255) | NULL | Payment conditions |
| notes | TEXT | NULL | Additional notes |
| created_at | TIMESTAMP | DEFAULT NOW() | Submission time |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last update time |

#### RFQ Attachments

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | UUID | PK, auto-generated | Unique identifier |
| rfq_id | UUID | FK to RequestForQuotes, NOT NULL | Parent RFQ |
| file_name | VARCHAR(255) | NOT NULL | Original file name |
| file_type | VARCHAR(50) | NOT NULL | MIME type |
| file_size | INTEGER | NOT NULL | Size in bytes |
| file_url | VARCHAR(500) | NOT NULL | Storage URL |
| created_at | TIMESTAMP | DEFAULT NOW() | Upload time |

#### Company Addresses

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | UUID | PK, auto-generated | Unique identifier |
| company_id | UUID | FK to Companies, NOT NULL | Owner company |
| label | VARCHAR(100) | NOT NULL | Address label (e.g., "HQ", "Warehouse") |
| is_default | BOOLEAN | DEFAULT false | Primary address flag |
| address_line1 | VARCHAR(255) | NOT NULL | Street address |
| address_line2 | VARCHAR(255) | NULL | Suite/building |
| city | VARCHAR(100) | NOT NULL | City |
| state | VARCHAR(100) | NULL | State/province |
| country | VARCHAR(100) | NOT NULL | Country |
| postal_code | VARCHAR(20) | NOT NULL | ZIP/postal code |
| phone | VARCHAR(20) | NULL | Contact phone |
| created_at | TIMESTAMP | DEFAULT NOW() | Creation time |

#### User Company Roles

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | UUID | PK, auto-generated | Unique identifier |
| user_id | UUID | FK to Users, NOT NULL | User |
| company_id | UUID | FK to Companies, NOT NULL | Company |
| role | ENUM | 'admin', 'manager', 'buyer', 'viewer' | Role type |
| created_at | TIMESTAMP | DEFAULT NOW() | Assignment time |

#### Sessions

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | UUID | PK, auto-generated | Unique identifier |
| user_id | UUID | FK to Users, NOT NULL | User |
| token_hash | VARCHAR(255) | NOT NULL | Hashed session token |
| ip_address | VARCHAR(45) | NOT NULL | Client IP |
| user_agent | VARCHAR(500) | NULL | Browser client |
| expires_at | TIMESTAMP | NOT NULL | Expiration time |
| created_at | TIMESTAMP | DEFAULT NOW() | Login time |

---

## 5. API Design

### Authentication APIs

| Endpoint | Method | Description |
|----------|--------|-------------|
| /api/v1/auth/register | POST | Register new user and company |
| /api/v1/auth/login | POST | Email/password login |
| /api/v1/auth/logout | POST | Invalidate session |
| /api/v1/auth/verify-email | POST | Verify email code |
| /api/v1/auth/forgot-password | POST | Send reset link |
| /api/v1/auth/reset-password | POST | Reset with token |
| /api/v1/auth/mfa/setup | POST | Initialize MFA |
| /api/v1/auth/mfa/verify | POST | Verify MFA code |
| /api/v1/auth/sso/login | GET | Initiate SSO redirect |
| /api/v1/auth/sso/callback | POST | Handle SSO callback |

### Product APIs

| Endpoint | Method | Description |
|----------|--------|-------------|
| /api/v1/products | GET | List products with filtering |
| /api/v1/products/:id | GET | Get product details |
| /api/v1/products | POST | Create product (supplier) |
| /api/v1/products/:id | PUT | Update product (supplier) |
| /api/v1/products/:id | DELETE | Soft delete product (supplier) |
| /api/v1/products/:id/pricing | PUT | Update pricing tiers |
| /api/v1/products/:id/images | POST | Upload product images |

### Company/Supplier APIs

| Endpoint | Method | Description |
|----------|--------|-------------|
| /api/v1/companies | GET | List companies |
| /api/v1/companies/:id | GET | Get company profile |
| /api/v1/companies/:slug | GET | Get company by slug |
| /api/v1/companies/:id | PUT | Update company profile |
| /api/v1/companies/:id/certifications | GET | List certifications |
| /api/v1/companies/:id/certifications | POST | Add certification |

### Order APIs

| Endpoint | Method | Description |
|----------|--------|-------------|
| /api/v1/orders | GET | List orders (filtered by role) |
| /api/v1/orders/:id | GET | Get order details |
| /api/v1/orders | POST | Create order |
| /api/v1/orders/:id/cancel | POST | Cancel order |
| /api/v1/orders/:id/status | PUT | Update order status |

### RFQ APIs

| Endpoint | Method | Description |
|----------|--------|-------------|
| /api/v1/rfqs | GET | List RFQs (filtered by role) |
| /api/v1/rfqs | POST | Create RFQ |
| /api/v1/rfqs/:id | GET | Get RFQ details |
| /api/v1/rfqs/:id/quotes | GET | List quote responses |
| /api/v1/rfqs/:id/quotes | POST | Submit quote (supplier) |
| /api/v1/rfqs/:id/quotes/:quoteId/accept | POST | Accept quote |
| /api/v1/rfqs/:id/quotes/:quoteId/reject | POST | Reject quote |

### Cart APIs

| Endpoint | Method | Description |
|----------|--------|-------------|
| /api/v1/cart | GET | Get cart contents |
| /api/v1/cart/items | POST | Add item to cart |
| /api/v1/cart/items/:id | PUT | Update cart item quantity |
| /api/v1/cart/items/:id | DELETE | Remove from cart |
| /api/v1/cart | DELETE | Clear cart |

### Search APIs

| Endpoint | Method | Description |
|----------|--------|-------------|
| /api/v1/search/products | GET | Search products |
| /api/v1/search/companies | GET | Search companies |
| /api/v1/search/suggestions | GET | Autocomplete suggestions |

---

## 6. Error Handling

### Error Response Format

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "One or more fields failed validation",
    "details": [
      {
        "field": "quantity",
        "message": "Quantity must be at least MOQ (100 units)",
        "value": 50
      }
    ],
    "request_id": "req_abc123xyz"
  }
}
```

### Error Codes

| Code | HTTP Status | Description |
|------|-------------|-------------|
| VALIDATION_ERROR | 400 | Invalid input data |
| AUTHENTICATION_REQUIRED | 401 | Not logged in |
| PERMISSION_DENIED | 403 | Insufficient permissions |
| RESOURCE_NOT_FOUND | 404 | Entity does not exist |
| DUPLICATE_ENTRY | 409 | Conflict (e.g., email exists) |
| RATE_LIMIT_EXCEEDED | 429 | Too many requests |
| INTERNAL_ERROR | 500 | Unexpected server error |

### Input Validation Rules

| Field | Rules |
|-------|-------|
| email | Valid email format, max 255 chars |
| password | Min 12 chars, uppercase, lowercase, number, special char |
| phone | Optional, digits only, 7-20 chars |
| quantity | Integer, min 1, max 1,000,000 |
| price | Decimal, min 0.01, max 999,999,999.99 |
| date | ISO 8601 format, must be valid date |

### Empty States

| Page | Empty State Message | Action |
|------|---------------------|--------|
| Search Results | "No products found matching your criteria" | Suggest clearing filters |
| Orders | "You have not placed any orders yet" | Link to product catalog |
| RFQs | "You have no active quote requests" | Link to create RFQ |
| Cart | "Your cart is empty" | Link to product catalog |
| Quotes | "No suppliers have submitted quotes yet" | Suggest extending deadline |

---

## 7. Notifications

### Notification Channels

| Channel | Use Case |
|---------|----------|
| Email | All major events (orders, quotes, updates) |
| In-app | Real-time alerts, reminders |
| SMS | Critical alerts (order shipped, urgent RFQ) - opt-in |

### Notification Events

| Event | Recipient | Channel | Template |
|-------|-----------|---------|----------|
| Order placed | Buyer | Email | order_confirmation |
| Order shipped | Buyer | Email + SMS | order_shipped |
| Order delivered | Buyer | Email | order_delivered |
| RFQ created | Matching suppliers | Email | rfq_new |
| Quote received | Buyer | Email | quote_received |
| Quote accepted | Supplier | Email | quote_accepted |
| Quote rejected | Supplier | Email | quote_rejected |
| Profile verified | Company admin | Email | account_verified |
| Payment overdue | Buyer | Email | payment_reminder |

---

## 8. Responsive Behavior

### Breakpoints

| Name | Width | Target |
|------|-------|--------|
| Mobile | < 768px | Smartphones |
| Tablet | 768px - 1024px | Tablets, small laptops |
| Desktop | 1024px - 1440px | Standard screens |
| Large | > 1440px | Large monitors |

### Layout Changes by Breakpoint

| Component | Mobile | Tablet | Desktop |
|-----------|--------|--------|---------|
| Header | Hamburger menu, stacked logo | Collapsed nav, horizontal menu | Full navigation |
| Product Grid | 1 column | 2 columns | 4 columns |
| Sidebar Filters | Drawer overlay | Collapsible sidebar | Always visible |
| Product Detail | Stacked images, full-width | 2-column (images left, info right) | 2-column with gallery |
| Data Table | Horizontal scroll, sticky first column | Scrollable | Full width, sortable columns |
| Cart | Full-screen drawer | Slide-in panel | Sidebar section |

### Heavy Visual Handling

| Feature | Mobile Strategy | Tablet Strategy |
|---------|-----------------|-----------------|
| Supplier cover images | Compressed to 800px width, lazy load | 1200px width |
| Product images | WebP format, 600px max | 1200px max |
| Certification badges | Tap to expand modal | Hover for tooltip |
| Analytics charts | Simplified single-bar charts | Full interactive charts |

---

## 9. Performance Requirements

### Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| Page Load Time | < 2s for initial paint | Lighthouse |
| Time to Interactive | < 3.5s | Lighthouse |
| Search Response | < 500ms | Backend monitoring |
| API Response (p95) | < 200ms | APM tool |
| Image Load | < 1s (above fold) | Real user monitoring |
| Smooth scrolling | 60fps maintained | Performance monitor |

### Optimization Techniques

| Technique | Application |
|-----------|-------------|
| Image optimization | WebP with JPEG fallback, responsive srcset |
| Lazy loading | All images below fold, infinite scroll items |
| Code splitting | Route-based chunks, vendor libraries separated |
| Caching | Redis for session, CDN for static assets |
| Database indexes | Composite indexes on (company_id, is_active, created_at) |
| Pagination | Cursor-based for large datasets |
| Compression | Gzip/Brotli for text assets |

---

## 10. Accessibility

### Accessibility Requirements

| Requirement | Implementation |
|-------------|-----------------|
| WCAG 2.1 AA compliance | Full audit before launch |
| Keyboard navigation | All interactive elements focusable |
| Focus indicators | 2px solid outline, high contrast |
| Screen reader support | ARIA labels, live regions |
| Color contrast | 4.5:1 minimum for text |
| Form errors | Associated with inputs, clear descriptions |

### prefers-reduced-motion

| Animation | Reduced Motion Behavior |
|------------|------------------------|
| Page transitions | Instant, no fade |
| Hover effects | Remove transforms, keep color change |
| Loading spinners | Replace with static indicator |
| Scroll animations | Disable parallax, show static positions |
| Modal overlays | Instant appear, no fade |

---

## 11. Security

### Security Measures

| Measure | Implementation |
|---------|----------------|
| Password hashing | Argon2 with salt |
| Session tokens | 256-bit random, HTTP-only, Secure, SameSite=Strict |
| Input sanitization | All user input escaped before output |
| SQL injection prevention | Parameterized queries only |
| CSRF protection | Token in all state-changing requests |
| Rate limiting | 100 requests/minute per IP, 1000/minute per user |
| Audit logging | All authentication events, data changes logged |

---

## 12. Admin Tools

### Platform Admin Dashboard

| Feature | Description |
|---------|-------------|
| User management | View, suspend, delete users |
| Company verification | Approve/reject pending companies |
| Content moderation | Review flagged content |
| Analytics | Platform-wide metrics, user growth, transaction volume |
| System health | Server status, error rates, uptime |
| Feature flags | Toggle features for A/B testing |

### Supplier Admin Dashboard

| Feature | Description |
|---------|-------------|
| Order management | View and update order statuses |
| Quote management | Submit and track quotes |
| Product catalog | CRUD operations on products |
| Analytics | Page views, conversion rates, revenue |
| Messages | Customer inquiries |

### Buyer Admin Dashboard

| Feature | Description |
|---------|-------------|
| Order tracking | Real-time order status |
| Purchase history | Searchable order archive |
| Saved suppliers | Preferred vendor list |
| RFQ management | Create and track RFQs |
| Analytics | Spending reports, supplier performance |

---

## 13. Future Considerations (Out of Scope for v1)

- Real-time chat between buyer and supplier
- Automated price comparison tools
- Blockchain-based contract management
- AI-powered supplier recommendation
- Mobile native application
- Multi-currency support with live exchange rates
- Integration with ERP systems (SAP, Oracle)