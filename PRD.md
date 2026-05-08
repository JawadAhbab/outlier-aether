# PRD: Services Platform

## 1. Visual Identity

### Typography
- Primary Font: Inter (Google Fonts), weights 400, 500, 600, 700
- Heading sizes: H1: 48px / line-height 1.1, H2: 36px / line-height 1.2, H3: 24px / line-height 1.3
- Body text: 16px / line-height 1.6
- Caption: 14px / line-height 1.4

### Color Palette
- Primary: #2563EB (Blue 600)
- Primary Hover: #1D4ED8 (Blue 700)
- Secondary: #10B981 (Emerald 500)
- Accent: #F59E0B (Amber 500)
- Background: #FFFFFF
- Surface: #F8FAFC (Slate 50)
- Border: #E2E8F0 (Slate 200)
- Text Primary: #0F172A (Slate 900)
- Text Secondary: #64748B (Slate 500)
- Error: #EF4444 (Red 500)
- Success: #22C55E (Green 500)

### Spacing System
- Base unit: 4px
- Spacing scale: 4, 8, 12, 16, 24, 32, 48, 64, 96
- Card padding: 24px
- Section padding: 64px vertical, 24px horizontal
- Border radius: 8px (cards), 6px (buttons), 4px (inputs)

## 2. Pages and Interactions

### 2.1 Homepage

#### Layout
- Hero section: Full-width banner with background gradient #2563EB to #1D4ED8, white text, height: 600px desktop / 400px mobile
- Search bar centered in hero with category dropdown, location input, and search button
- Category cards: 4-column grid on desktop, 2-column on tablet, 1-column on mobile
- Featured providers section: Horizontal scroll carousel with 4 visible cards
- Trust indicators section: Logo strip with 6 partner logos

#### Interactions
- Search form submission navigates to search results page with query params
- Category cards hover: translateY(-4px), box-shadow increase, 200ms ease-out
- Carousel navigation: Left/right arrows appear on hover, 300ms fade transition

### 2.2 Service Catalog Page

#### Layout
- Left sidebar: 280px width with filters (desktop only)
- Main content: 3-column card grid, gap: 24px
- Cards: 320px min-width, image top (16:9 ratio), content below
- Pagination: Bottom right, shows 12 items per page

#### Filters
- Service category: Checkbox list with icons
- Price range: Dual-handle slider, min $0 max $500
- Rating: Star rating selector (1-5 stars)
- Availability: Toggle switch for "Available today"
- Location: Radius selector (5mi, 10mi, 25mi, 50mi)

#### Interactions
- Filter changes trigger URL param updates and instant content refresh (no page reload)
- Active filters shown as removable chips above grid
- Sort dropdown: "Relevance", "Price: Low to High", "Price: High to Low", "Rating"
- Card hover: border-color transitions to primary, 150ms

### 2.3 Provider Profile Page

#### Layout
- Header: Full-width cover image (400px height), profile photo overlapping (-80px offset), provider name, title, location
- Tab navigation: Overview, Services, Reviews, Credentials
- Content area: 2-column layout (main 65%, sidebar 35%)

#### Profile Information
- Avatar: 120px circle, bordered with white
- Star rating display with review count
- Response time badge
- Years of experience counter
- Languages spoken: Tag pills
- Bio section: Max 300 characters with "Read more" expansion

#### Credentials Display
- License number with verification badge
- Certifications as icon badges with tooltips
- Education history timeline
- Awards section with trophy icons

#### Booking Flow
- Calendar widget: Month view, available dates highlighted in primary color
- Time slot grid: 30-minute increments, 8:00 AM to 6:00 PM
- Selected slot: Primary background, white text, checkmark icon
- Book button: Full-width, 48px height, disabled until slot selected

### 2.4 Appointment Booking Flow

#### Step 1: Select Service
- Service cards with icons, names, durations, prices
- Multi-select allowed for bundled services
- Running total displayed in sticky footer

#### Step 2: Choose Date/Time
- Calendar with available slots
- Timezone selector dropdown
- Confirm button proceeds to step 3

#### Step 3: Enter Details
- Form fields: Full name, email, phone, date of birth, notes (textarea)
- Health questionnaire: 5 yes/no questions with conditional text fields
- Insurance provider dropdown with "Self-pay" option
- File upload for insurance card (JPG, PNG, PDF, max 5MB)

#### Step 4: Review & Confirm
- Summary card with all selections
- Edit links for each section
- Terms checkbox
- Confirm booking button

#### Step 5: Confirmation
- Success animation: Checkmark with confetti burst, 800ms
- Booking reference number prominently displayed
- Add to calendar buttons (Google, Apple, Outlook)
- Email confirmation notice

### 2.5 Quote Request Form

#### Layout
- Multi-step form with progress indicator (4 steps)
- Step nav buttons: Back and Continue

#### Steps
1. Service Type: Category selection cards
2. Coverage Details: Plan type radio buttons (Basic, Standard, Premium), coverage amount input
3. Personal Information: Name, email, phone, date of birth, address fields
4. Additional Info: Current provider dropdown, current monthly premium input, notes textarea

#### Validation
- Email format validation with regex
- Phone: (XXX) XXX-XXXX format auto-formatting
- Required fields: Red asterisk, error message below field
- Error state: 2px red border, red text below, shake animation 300ms
- Success state: Green checkmark icon inside input

### 2.6 User Dashboard

#### Layout
- Sidebar navigation: Profile, Appointments, Messages, Documents, Settings
- Main content area with page title
- Recent activity feed on dashboard home

#### Appointments View
- List view with status badges (Upcoming, Completed, Cancelled)
- Quick actions: Reschedule, Cancel, Leave Review
- Filter by status tabs

### 2.7 Authentication Pages

#### Login
- Email/password form with "Remember me" checkbox
- Social login buttons: Google, Apple (SVG icons, 44px height)
- Forgot password link
- "Don't have an account? Sign up" link

#### Signup
- Multi-step: Email verification code, then profile creation
- Fields: Name, email, password, confirm password, phone
- Password strength indicator: 4-segment bar, colors from red to green

## 3. Animation Specifications

### Page Transitions
- Fade out: 200ms ease-in
- Fade in: 300ms ease-out
- Content stagger: 50ms between elements

### Micro-interactions
- Button hover: Background lightens 10%, scale(1.02), 150ms
- Button click: scale(0.98), 100ms, then return
- Card hover: translateY(-4px), box-shadow 0 8px 30px rgba(0,0,0,0.12), 200ms ease-out
- Form field focus: Border color to primary, 150ms ease
- Dropdown open: max-height transition 200ms ease-out, opacity 0 to 1

### Loading States
- Skeleton screens: Pulsing animation, 1.5s infinite, background gradient #E2E8F0 to #F8FAFC
- Spinner: 24px, primary color, rotate animation 1s linear infinite

### Scroll Animations
- Fade in up: translateY(20px) to translateY(0), opacity 0 to 1, 400ms ease-out
- Trigger: When element is 20% in viewport
- Intersection Observer threshold: 0.2

## 4. Technical Requirements

### Frontend Stack
- Framework: Next.js 14 with App Router
- Styling: Tailwind CSS 3.4
- Animation: Framer Motion 11
- Calendar: react-big-calendar
- Form handling: React Hook Form with Zod validation
- Date handling: date-fns

### Backend
- API: Next.js API routes with TypeScript
- Database: PostgreSQL with Prisma ORM
- Auth: NextAuth.js with JWT
- File storage: AWS S3

### Third-party Integrations
- Payment: Stripe Checkout (for paid appointments)
- SMS: Twilio (appointment reminders)
- Email: SendGrid (transactional emails)
- Maps: Google Maps JavaScript API

## 5. Application Logic

### User Roles
- Guest: Browse services, view profiles, fill forms
- Registered User: Book appointments, manage profile, view history, receive notifications
- Provider: Manage availability, view bookings, update profile
- Admin: Manage users, review content, handle disputes

### Data Model

| Entity | Fields | Relationships |
|--------|--------|---------------|
| User | id (UUID), email, password_hash, name, phone, role, created_at, updated_at | has many Appointments, has one ProviderProfile |
| ProviderProfile | id, user_id, bio, credentials, languages, service_categories, rating, review_count | belongs to User, has many Services, has many Appointments |
| Service | id, provider_id, name, description, duration_minutes, price, category | belongs to ProviderProfile |
| Appointment | id, user_id, provider_id, service_id, date, time_slot, status, notes, created_at | belongs to User, belongs to Provider, belongs to Service |
| Review | id, appointment_id, user_id, provider_id, rating, comment, created_at | belongs to Appointment |
| QuoteRequest | id, user_id, service_type, coverage_details, status, created_at | belongs to User |

### Authentication Flow
1. User enters email/password or uses social login
2. Server validates credentials, generates JWT (24h expiry)
3. Token stored in httpOnly cookie
4. Protected routes check token validity via middleware
5. Refresh token rotation for extended sessions (7 days)

### Booking Flow (Step by Step)
1. User selects service from catalog
2. User views provider profile, selects available time slot
3. User fills intake form and confirms booking
4. System creates Appointment record with "Pending" status
5. Confirmation email sent via SendGrid
6. Provider receives notification via dashboard
7. Provider confirms appointment, status becomes "Confirmed"
8. Reminder SMS sent 24h before via Twilio

### Quote Request Flow (Step by Step)
1. User selects service category
2. User fills multi-step form with personal and coverage details
3. System validates all fields with Zod schema
4. QuoteRequest record created with "Submitted" status
5. User receives confirmation with reference number
6. Admin reviews request in admin dashboard
7. Admin updates status to "Under Review" then "Quote Ready"
8. User receives email with quote details and payment link

## 6. Responsive Behavior

### Breakpoints
- Mobile: 0px - 639px
- Tablet: 640px - 1023px
- Desktop: 1024px - 1279px
- Wide: 1280px+

### Layout Changes
- Navigation: Bottom tab bar on mobile, top header on desktop
- Cards: 1 column (mobile), 2 columns (tablet), 3-4 columns (desktop)
- Sidebar filters: Collapsible drawer on mobile, visible sidebar on desktop
- Typography: H1 scales from 32px (mobile) to 48px (desktop)

### 3D/SVG Scaling
- Hero background animation: Disabled on mobile (prefers-reduced-motion respected)
- Service icons: SVG sprites, single color version on mobile
- Decorative elements: Hidden on mobile to improve performance

## 7. Performance and Accessibility

### Performance Metrics
- Target: 90+ Lighthouse score
- First Contentful Paint: Under 1.5s
- Time to Interactive: Under 3s
- Bundle size: Under 200KB gzipped

### Techniques
- Image optimization: Next/Image with WebP, lazy loading
- Code splitting: Route-based automatic splitting
- Font optimization: next/font with font-display: swap
- Caching: SWC minification, ISR for static pages

### Accessibility
- Semantic HTML: Proper heading hierarchy, landmark regions
- ARIA labels: For interactive elements, form fields, modals
- Focus management: Visible focus rings, logical tab order
- prefers-reduced-motion: Disables all animations, shows static content
- Color contrast: Minimum 4.5:1 for normal text, 3:1 for large text
- Keyboard navigation: All interactions possible without mouse