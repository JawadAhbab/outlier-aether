# Product Requirements Document: Retail Online Store

## 1. Project Overview

### Project Name
ShopFlow - Modern Retail Platform

### Project Type
E-commerce web application with full shopping functionality

### Core Functionality Summary
A feature-rich online retail platform where consumers can browse products, compare options across categories, filter by price and attributes, manage a shopping cart, and complete purchases through a streamlined checkout flow. The platform supports both authenticated users and guest visitors with persistent cart functionality.

### Target Users
- **Primary**: Consumer shoppers aged 18-55 seeking convenient online purchasing
- **Secondary**: Store administrators managing inventory and orders
- **Tertiary**: Guest users exploring products before committing to account creation

---

## 2. Visual Identity

### Typography

- Headings H1: Inter, 700 (Bold), 48px, line-height 1.2
- Headings H2: Inter, 600 (Semibold), 36px, line-height 1.25
- Headings H3: Inter, 600 (Semibold), 24px, line-height 1.3
- Body Text: Inter, 400 (Regular), 16px, line-height 1.5
- Caption: Inter, 400 (Regular), 14px, line-height 1.4
- Button: Inter, 500 (Medium), 16px, line-height 1
- Price: Inter, 700 (Bold), 20px, line-height 1

### Color Palette

- Primary: #2563EB - CTAs, links, active states
- Primary Dark: #1D4ED8 - Hover states
- Secondary: #64748B - Secondary text, icons
- Accent: #F59E0B - Sale badges, highlights
- Success: #10B981 - Confirmations, in-stock
- Error: #EF4444 - Error messages, out-of-stock
- Background: #FFFFFF - Page background
- Surface: #F8FAFC - Cards, elevated surfaces
- Border: #E2E8F0 - Dividers, borders
- Text Primary: #0F172A - Headings, body
- Text Secondary: #64748B - Captions, metadata

### Spacing System

- xs: 4px - Tight gaps, inline spacing
- sm: 8px - Icon gaps, compact padding
- md: 16px - Standard padding, gaps
- lg: 24px - Section padding
- xl: 32px - Card padding
- 2xl: 48px - Section margins

### Border Radius

- sm: 4px - Badges, tags
- md: 8px - Buttons, inputs
- lg: 12px - Cards
- xl: 16px - Modals, drawers
- full: 9999px - Pills, avatars

---

## 3. User Roles and Authentication

### User Roles

- Guest: Unauthenticated browser - Product browsing, cart management, limited checkout
- Registered User: Authenticated shopper - Full browsing, order history, saved addresses, wishlist
- Admin: Store administrator - Product management, order management, user management, analytics

### Authentication Methods

- Email/Password: Standard - Email verification required
- Google OAuth: Social Login - One-tap sign-in via Google
- Apple Sign-In: Social Login - Face ID / Touch ID integration

### Authentication Flow

1. User visits site
2. System checks session cookie
3. If logged in: Show user dashboard
4. If not logged in: Show login/register UI

### Session Management
- JWT tokens stored in httpOnly cookies
- Access token: 15-minute expiry with silent refresh
- Refresh token: 7-day expiry with rotation on use
- Persistent cart: localStorage + server sync for logged-in users

---

## 4. Page Structure and Layout

### Site Architecture

```
Home
├── Product Listing Page (PLP)
│   ├── Category View
│   ├── Search Results
│   └── Brand Page
├── Product Detail Page (PDP)
├── Cart Page
├── Checkout Flow
│   ├── Shipping Information
│   ├── Payment Method
│   └── Order Review
├── User Account
│   ├── Dashboard
│   ├── Order History
│   ├── Addresses
│   └── Profile Settings
├── Wishlist Page
└── Admin Panel (protected)
    ├── Products Management
    ├── Orders Management
    └── Analytics Dashboard
```

### Page Specifications

#### 4.1 Home Page

**Hero Section**
- Full-width hero banner: 100vw width, 600px height (desktop), 400px height (mobile)
- Background: High-quality promotional image with dark overlay (rgba(0,0,0,0.4))
- Centered headline: H1 white text, max-width 800px
- CTA button: Primary color, 48px height, 24px horizontal padding

**Featured Categories**
- 4-column grid (desktop), 2-column (tablet), 1-column (mobile)
- Category cards: 280px height, image fill with gradient overlay
- Category name: White H3 text, bottom-left positioned

**Featured Products**
- 4-column product grid with 24px gap
- Product cards: White background, 8px border-radius, subtle shadow

**Promotional Banner**
- Full-width banner with 48px vertical padding
- Split layout: Image left (40%), content right (60%)
- Content: Badge, headline, description, CTA button

#### 4.2 Product Listing Page (PLP)

**Layout Structure**
- Left sidebar: 280px width for filters (collapsible on mobile)
- Main content: Flexible width, max 1200px centered
- Grid: 3 columns (desktop), 2 columns (tablet), 1 column (mobile)

**Filter Sidebar**
- Sticky positioning on scroll
- Sections: Categories, Price Range, Brand, Rating, Availability
- Price range: Dual-handle slider, min/max inputs
- Active filters: Pills with remove button above grid

**Sort Dropdown**
- Options: Featured, Price Low-High, Price High-Low, Newest, Best Selling, Rating
- Position: Top-right of product grid
- Style: 44px height, border, dropdown icon

**Pagination**
- Bottom of grid, centered
- Page numbers with prev/next arrows
- Show 24 products per page

#### 4.3 Product Detail Page (PDP)

**Layout Structure**
- Two-column layout: Image gallery (55%), Details (45%)
- Sticky product details on scroll (desktop)

**Image Gallery**
- Main image: 600px height, object-fit contain
- Thumbnail strip: 5 visible, horizontal scroll
- Thumbnails: 80px squares, 4px border on active
- Zoom: Hover to zoom 2x, pan on click
- Lightbox: Full-screen overlay on main image click

**Product Information**
- Breadcrumb: Home > Category > Product Name
- Title: H1, 36px
- Rating: 5-star display with review count link
- Price: 28px bold, sale price in accent color if discounted
- Short description: 2-3 lines below price
- Variant selectors: Color swatches (32px circles), Size buttons (pill style)
- Quantity selector: Number input with +/- buttons, 44px touch target
- Add to Cart button: Full-width, 56px height, primary color
- Wishlist button: Icon button, heart outline/filled toggle

**Product Tabs**
- Tabs: Description, Specifications, Reviews
- Tab content: Padded 24px, max-width 800px
- Reviews: Sortable by rating, with pagination

#### 4.4 Cart Page

**Layout Structure**
- Two-column: Cart items (65%), Order summary (35%)
- Mobile: Single column, summary at bottom (sticky)

**Cart Item Card**
- Product image: 120px square, object-fit cover
- Product details: Title, variant info, price
- Quantity: Inline +/- controls
- Remove: X icon, top-right corner
- Line total: Bold, right-aligned

**Order Summary**
- Subtotal row
- Estimated shipping row
- Tax row (calculated at checkout)
- Promo code input: Text input + Apply button
- Total row: Bold, larger font
- Checkout button: Full-width, primary color, 56px height

#### 4.5 Checkout Flow

**Step Indicator**
- Horizontal steps: Shipping > Payment > Review
- Active step: Primary color circle with number
- Completed step: Checkmark icon
- Inactive: Gray circle

**Shipping Step**
- Address form: Name, Street, Apartment, City, State, ZIP, Country
- Address autocomplete (Google Places API)
- Shipping method selection: Radio buttons with price and ETA
- Form validation: Real-time, inline error messages

**Payment Step**
- Payment methods: Credit Card, PayPal, Apple Pay, Shop Pay
- Credit card form: Number, Expiry, CVC, Name on card
- Card type auto-detection with icon display
- Billing address: Same as shipping checkbox or separate form

**Review Step**
- Order summary with all items
- Shipping and payment summary (editable)
- Edit links to previous steps
- Place Order button with lock icon
- Terms acceptance checkbox

#### 4.6 User Account Pages

**Dashboard**
- Welcome message with user name
- Quick stats: Total orders, Wishlist items, Points balance
- Recent orders list (3 items)
- Quick actions: Track Order, Continue Shopping

**Order History**
- Sortable table: Order #, Date, Status, Total
- Status badges: Processing (yellow), Shipped (blue), Delivered (green), Cancelled (red)
- Expandable row for order details
- Reorder button

**Addresses**
- Grid of address cards (2 columns)
- Default badge on primary address
- Edit/Delete actions
- Add New Address button

---

## 5. Component Specifications

### 5.1 Product Card

**Visual Structure**
- Container: White background, 12px border-radius, 1px border (--color-border)
- Image container: Aspect ratio 1:1, overflow hidden
- Image: Scale to 105% on hover, 300ms ease transition
- Quick actions: Add to Cart icon, Wishlist icon (top-right overlay on image)
- Content padding: 16px

**Content Elements**
- Product title: 16px, font-weight 500, 2-line clamp
- Rating: 5 stars with count
- Price: 18px bold, original price struck through if sale
- Color swatches: 20px circles, max 4 visible + "+N" indicator

**States**
- Default: As described
- Hover: Shadow elevation, image zoom, quick actions visible
- Out of stock: Grayscale image, "Out of Stock" overlay badge
- On sale: Accent color price, "Sale" badge top-left

### 5.2 Add to Cart Button

**Visual**
- Height: 48px
- Border-radius: 8px
- Background: Primary color
- Text: White, 16px, font-weight 500
- Icon: Shopping bag icon left of text

**States**
- Default: Primary background
- Hover: Primary dark background
- Loading: Spinner replacing icon, disabled state
- Success: Green background, checkmark icon, "Added!" text (reverts after 2s)
- Disabled: 50% opacity, cursor not-allowed

### 5.3 Quantity Selector

**Visual**
- Container: Inline-flex, border 1px, border-radius 8px
- Width: 120px
- Height: 44px

**Elements**
- Minus button: 44px square, icon centered
- Input: 40px width, text-center, no spinners
- Plus button: 44px square, icon centered

**States**
- Default: Border color
- Focus: Primary border
- Disabled minus: At quantity 1, 50% opacity
- Disabled plus: At max quantity, 50% opacity

### 5.4 Filter Panel

**Visual**
- Background: Surface color
- Padding: 24px
- Border-radius: 12px

**Filter Types**
- Checkbox group: Stacked checkboxes with count badges
- Price range: Dual-handle slider (rc-slider)
- Color swatches: Grid of circles, checkmark on selected
- Rating: Clickable star buttons with minimum rating

**Behavior**
- Instant apply on change (no submit button)
- URL params sync for shareable filtered views
- Clear all button when filters active
- Mobile: Slide-in drawer from left, 320px width

### 5.5 Toast Notifications

**Visual**
- Position: Bottom-right, 24px from edges
- Width: 360px max
- Padding: 16px
- Border-radius: 12px
- Shadow: 0 4px 12px rgba(0,0,0,0.15)

**Types**
- Success: Green left border, checkmark icon
- Error: Red left border, X icon
- Info: Blue left border, info icon
- Warning: Yellow left border, alert icon

**Animation**
- Enter: Slide in from right, 300ms ease-out
- Exit: Fade out, 200ms ease-in
- Auto-dismiss: 5 seconds

---

## 6. Application Logic

### 6.1 Product Browsing Flow

1. User lands on PLP
2. Load products from API: GET /api/products?page=1&limit=24
3. Display product grid with skeleton loaders during fetch
4. User applies filters
5. Update URL params, debounce 300ms, fetch filtered results
6. User clicks product card
7. Navigate to PDP: GET /api/products/{slug}
8. Load product details, variants, reviews

### 6.2 Cart Management Flow

1. User clicks Add to Cart
2. Validate variant selection (if applicable)
3. If invalid: Show error toast: "Please select size/color"
4. If valid: POST /api/cart/add
5. Update cart state in store
6. Show success toast: "Added to cart"
7. Update cart icon badge count
8. Persist to localStorage (guest) or server (user)

### 6.3 Checkout Flow

1. User clicks Checkout
2. Check if logged in
3. If no: Redirect to login
4. If yes: Load saved addresses
5. Display shipping form
6. User fills and submits shipping
7. POST /api/checkout/validate-shipping
8. Display payment step
9. User selects payment method
10. If Credit Card selected: Validate card with Stripe Elements
11. Display order review
12. User clicks Place Order
13. POST /api/checkout/complete
14. Create order, charge payment, send confirmation email
15. Redirect to confirmation page: /order-confirmation/{orderId}

### 6.4 Search Functionality

1. User types in search input
2. Debounce 300ms
3. Show loading indicator
4. GET /api/search?q={query}&type={product|collection}
5. Display dropdown with categories, products, suggestions
6. User presses Enter or clicks result
7. Navigate to result or PLP with search params

---

## 7. Data Model

### Entities and Relationships

Users

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | PK |
| email | String | unique |
| password_hash | String | |
| first_name | String | |
| last_name | String | |
| phone | String | nullable |
| created_at | Timestamp | |
| updated_at | Timestamp | |
| role | Enum | guest, user, admin |

Addresses

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | PK |
| user_id | UUID | FK -> Users |
| first_name | String | |
| last_name | String | |
| street_address | String | |
| apartment | String | nullable |
| city | String | |
| state | String | |
| postal_code | String | |
| country | String | |
| phone | String | |
| is_default | Boolean | |
| created_at | Timestamp | |
| updated_at | Timestamp | |

Categories

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | PK |
| name | String | |
| slug | String | unique |
| description | Text | |
| parent_id | UUID | FK -> Categories, nullable |
| image_url | String | nullable |
| sort_order | Integer | |
| is_active | Boolean | |

Products

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | PK |
| name | String | |
| slug | String | unique |
| description | Text | |
| price | Decimal | |
| compare_at_price | Decimal | nullable |
| cost_price | Decimal | nullable |
| sku | String | unique |
| barcode | String | nullable |
| inventory_quantity | Integer | |
| category_id | UUID | FK -> Categories |
| images | JSON Array | |
| metadata | JSON | |
| is_active | Boolean | |
| created_at | Timestamp | |
| updated_at | Timestamp | |

ProductVariants

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | PK |
| product_id | UUID | FK -> Products |
| name | String | |
| sku | String | unique |
| price | Decimal | |
| compare_at_price | Decimal | nullable |
| inventory_quantity | Integer | |
| option1 | String | nullable |
| option2 | String | nullable |
| option3 | String | nullable |
| is_active | Boolean | |

ProductOptions

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | PK |
| product_id | UUID | FK -> Products |
| name | String | |
| position | Integer | |
| values | JSON Array | |

Reviews

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | PK |
| product_id | UUID | FK -> Products |
| user_id | UUID | FK -> Users |
| rating | Integer | 1-5 |
| title | String | nullable |
| body | Text | |
| is_verified | Boolean | |
| created_at | Timestamp | |
| updated_at | Timestamp | |

Orders

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | PK |
| order_number | String | unique |
| user_id | UUID | FK -> Users, nullable for guest |
| email | String | |
| status | Enum | pending, processing, shipped, delivered, cancelled, refunded |
| subtotal | Decimal | |
| shipping_cost | Decimal | |
| tax | Decimal | |
| discount_amount | Decimal | |
| total | Decimal | |
| shipping_address | JSON | |
| billing_address | JSON | |
| shipping_method | String | |
| payment_method | String | |
| payment_status | Enum | pending, paid, failed, refunded |
| stripe_payment_intent_id | String | nullable |
| created_at | Timestamp | |
| updated_at | Timestamp | |

OrderItems

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | PK |
| order_id | UUID | FK -> Orders |
| product_id | UUID | FK -> Products |
| variant_id | UUID | FK -> ProductVariants, nullable |
| quantity | Integer | |
| unit_price | Decimal | |
| total_price | Decimal | |
| product_snapshot | JSON | |

Cart

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | PK |
| user_id | UUID | FK -> Users, nullable |
| session_id | String | for guest |
| created_at | Timestamp | |
| updated_at | Timestamp | |

CartItems

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | PK |
| cart_id | UUID | FK -> Cart |
| product_id | UUID | FK -> Products |
| variant_id | UUID | FK -> ProductVariants, nullable |
| quantity | Integer | |
| created_at | Timestamp | |
| updated_at | Timestamp | |

Wishlists

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | PK |
| user_id | UUID | FK -> Users |
| name | String | |
| is_public | Boolean | |
| created_at | Timestamp | |
| updated_at | Timestamp | |

WishlistItems

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | PK |
| wishlist_id | UUID | FK -> Wishlists |
| product_id | UUID | FK -> Products |
| variant_id | UUID | FK -> ProductVariants, nullable |
| created_at | Timestamp | |
| updated_at | Timestamp | |

PromoCodes

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | PK |
| code | String | unique |
| type | Enum | percentage, fixed_amount, free_shipping |
| value | Decimal | |
| min_order_amount | Decimal | nullable |
| max_uses | Integer | nullable |
| used_count | Integer | |
| starts_at | Timestamp | |
| expires_at | Timestamp | nullable |
| is_active | Boolean | |
| created_at | Timestamp | |

---

## 8. API Design

### Public Endpoints

#### Products
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/products | List products with pagination, filters |
| GET | /api/products/:slug | Get product by slug |
| GET | /api/products/:id/related | Get related products |
| GET | /api/products/:id/reviews | Get product reviews |

#### Collections
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/collections | List all collections |
| GET | /api/collections/:slug | Get collection with products |
| GET | /api/collections/:slug/products | Get products in collection |

#### Search
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/search | Search products, collections |

#### Cart
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/cart | Get current cart |
| POST | /api/cart/items | Add item to cart |
| PUT | /api/cart/items/:id | Update cart item quantity |
| DELETE | /api/cart/items/:id | Remove item from cart |

#### Checkout
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | /api/checkout/validate-shipping | Validate shipping address |
| POST | /api/checkout/calculate | Calculate totals |
| POST | /api/checkout/complete | Complete order |

### Authentication Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | /api/auth/register | Create new account |
| POST | /api/auth/login | Login with email/password |
| POST | /api/auth/logout | Logout current session |
| POST | /api/auth/refresh | Refresh access token |
| POST | /api/auth/forgot-password | Request password reset |
| POST | /api/auth/reset-password | Reset password with token |
| POST | /api/auth/google | Google OAuth callback |

### User Endpoints (Authenticated)

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/user/profile | Get user profile |
| PUT | /api/user/profile | Update user profile |
| GET | /api/user/addresses | List saved addresses |
| POST | /api/user/addresses | Add new address |
| PUT | /api/user/addresses/:id | Update address |
| DELETE | /api/user/addresses/:id | Delete address |
| GET | /api/user/orders | List user orders |
| GET | /api/user/orders/:id | Get order details |

### Admin Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/admin/products | List products (admin view) |
| POST | /api/admin/products | Create product |
| PUT | /api/admin/products/:id | Update product |
| DELETE | /api/admin/products/:id | Delete product |
| GET | /api/admin/orders | List all orders |
| PUT | /api/admin/orders/:id/status | Update order status |
| GET | /api/admin/analytics | Get analytics data |

---

## 9. Error Handling

### HTTP Status Codes

- 200: Successful GET, PUT
- 201: Successful POST (created)
- 204: Successful DELETE
- 400: Bad request, validation error
- 401: Unauthorized, not logged in
- 403: Forbidden, insufficient permissions
- 404: Resource not found
- 409: Conflict (e.g., duplicate email)
- 422: Unprocessable entity
- 429: Rate limited
- 500: Internal server error

### Error Response Format

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input data",
    "details": [
      {
        "field": "email",
        "message": "Email is required"
      },
      {
        "field": "password",
        "message": "Password must be at least 8 characters"
      }
    ]
  }
}
```

### Client-Side Error Handling

- Network error: Toast: "Connection error. Please check your internet."
- 401 Unauthorized: Modal: "Please log in to continue" with login button
- 403 Forbidden: Toast: "You do not have permission to perform this action"
- 404 Product: Page: "Product not found" with continue shopping CTA
- 422 Validation: Inline field errors, form highlight
- 500 Server Error: Toast: "Something went wrong. Please try again."
- Cart item out of stock: Toast: "Item is no longer available", remove from cart

### Empty States

- Search results: "No products match your search" - Show suggestions, clear filters link
- Cart: "Your cart is empty" - Continue Shopping CTA
- Wishlist: "Your wishlist is empty" - Browse Products CTA
- Orders: "You have not placed any orders yet" - Start Shopping CTA
- Reviews: "No reviews yet" - Be the first to review CTA

---

## 10. Responsive Breakpoints

### Breakpoint Definitions

- Mobile: 0px - 639px - Single column, bottom nav
- Tablet: 640px - 1023px - Two columns, sidebar collapsible
- Desktop: 1024px - 1279px - Full layout, 3-column grid
- Wide: 1280px+ - Max-width container, 4-column grid

### Responsive Changes Per Tier

**Product Grid**
- Mobile: 1 column, full-width cards
- Tablet: 2 columns, standard cards
- Desktop: 3 columns, comfortable spacing
- Wide: 4 columns, expanded cards

**Navigation**
- Mobile: Hamburger menu, bottom navigation bar
- Tablet: Side navigation drawer
- Desktop: Top navigation with mega menus
- Wide: Same as desktop with expanded dropdowns

**Product Detail Page**
- Mobile: Stacked layout, image carousel
- Tablet: Side-by-side, sticky details
- Desktop: Full two-column with gallery
- Wide: Larger images, expanded info

**Cart Page**
- Mobile: Stacked, sticky checkout button
- Tablet: Two-column, inline totals
- Desktop: Full two-column with summary sidebar

### Mobile-Specific Optimizations

- Touch targets: Minimum 44x44px
- Swipe gestures: Product image carousel, dismiss modals
- Pull to refresh: Cart, order history
- Bottom sheets: Filters, sort options
- Skeleton loaders: Immediate visual feedback

---

## 11. Performance Requirements

### Core Web Vitals Targets

| Metric | Target | Measurement |
|--------|--------|-------------|
| LCP (Largest Contentful Paint) | < 2.5s | 75th percentile |
| FID (First Input Delay) | < 100ms | 75th percentile |
| CLS (Cumulative Layout Shift) | < 0.1 | 75th percentile |
| INP (Interaction to Next Paint) | < 200ms | 75th percentile |

### Asset Budget

| Asset Type | Budget | Format |
|------------|--------|--------|
| HTML | < 100KB | Minified |
| CSS | < 50KB | Minified, critical inline |
| JavaScript | < 200KB | Minified, code-split |
| Product Images | < 100KB each | WebP, lazy-loaded |
| Hero Images | < 300KB | WebP with fallback |
| Icons | < 20KB total | SVG sprite |

### Performance Techniques

| Technique | Implementation |
|-----------|---------------|
| Image optimization | WebP with JPEG fallback, responsive srcset |
| Lazy loading | Native loading="lazy" + Intersection Observer |
| Code splitting | Route-based chunks with dynamic imports |
| CDN | Static assets served from Edge CDN |
| Caching | Service Worker for offline support, stale-while-revalidate |
| Prefetching | Prefetch linked pages on hover |
| Compression | Brotli for text assets |
| Font optimization | Font-display: swap, subset fonts |

---

## 12. Accessibility Requirements

### WCAG 2.1 AA Compliance

| Requirement | Implementation |
|-------------|----------------|
| Color contrast | 4.5:1 for text, 3:1 for UI elements |
| Focus indicators | Visible focus ring, 2px offset |
| Keyboard navigation | Full site accessible via Tab, Enter, Escape |
| Screen reader | Semantic HTML, ARIA labels, live regions |
| Form labels | Associated labels for all inputs |
| Error identification | Error messages linked to inputs |

### Keyboard Navigation

| Key | Action |
|-----|--------|
| Tab | Move to next focusable element |
| Shift+Tab | Move to previous focusable element |
| Enter | Activate buttons, links, form submissions |
| Space | Activate checkboxes, radio buttons |
| Escape | Close modals, dropdowns |
| Arrow keys | Navigate within menus, carousels |

### Reduced Motion Support

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

| Animation | Reduced Motion Behavior |
|-----------|------------------------|
| Page transitions | Instant switch, no fade |
| Product image zoom | No zoom on hover |
| Scroll animations | Disable parallax, show all content |
| Loading spinners | Static loading indicator |
| Toast notifications | Instant appear/disappear |
| Cart drawer | Instant open/close |

---

## 13. Technical Stack

### Frontend

| Technology | Version | Purpose |
|-----------|---------|---------|
| React | 18.2+ | UI framework |
| Next.js | 14+ | SSR, routing |
| TypeScript | 5.0+ | Type safety |
| Tailwind CSS | 3.4+ | Utility-first styling |
| Zustand | 4.5+ | State management |
| React Query | 5.0+ | Server state, caching |
| Stripe Elements | 13.0+ | Payment UI |

### Backend

| Technology | Version | Purpose |
|-----------|---------|---------|
| Node.js | 20 LTS | Runtime |
| Express | 4.18+ | API framework |
| PostgreSQL | 15+ | Primary database |
| Redis | 7+ | Caching, sessions |
| Stripe | 13.0+ | Payment processing |
| SendGrid | 4.7+ | Transactional email |

### Infrastructure

| Technology | Purpose |
|-----------|---------|
| Vercel | Frontend hosting |
| Railway | Backend hosting |
| AWS S3 | Image storage |
| Cloudflare | CDN, DDoS protection |
| Sentry | Error tracking |
| Datadog | Performance monitoring |
