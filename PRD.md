# PRD: News Portal

## 1. Concept & Vision

A premium news portal delivering breaking stories, investigative journalism, and editorial content across multiple categories. The experience prioritizes readability, fast navigation, and real-time updates. Users feel informed, not overwhelmed. The design evokes trust and authority like NYTimes and The Guardian while delivering a modern, fast digital experience.

## 2. Visual Identity

### Typography
- Headlines: Playfair Display, 700 weight, 48px desktop / 32px mobile
- Subheadlines: Inter, 600 weight, 24px desktop / 20px mobile
- Body: Inter, 400 weight, 18px, line-height 1.7
- Metadata: Inter, 400 weight, 14px, uppercase tracking 0.05em
- Font roles: Headlines for article titles, subheadlines for section headers, body for article content

### Colors
- Background: #FAFAFA
- Surface: #FFFFFF
- Primary Text: #1A1A1A
- Secondary Text: #6B7280
- Accent: #DC2626 (breaking news, CTAs)
- Accent Hover: #B91C1C
- Link: #2563EB
- Link Hover: #1D4ED8
- Border: #E5E7EB
- Breaking Banner: #DC2626

### Spacing
- Page max-width: 1280px
- Content max-width: 720px (article body)
- Grid gap: 24px
- Section padding: 64px vertical desktop / 40px mobile
- Card padding: 24px
- Border radius: 8px (cards), 4px (buttons/inputs)

## 3. Layout & Structure

### Header
- Logo left, primary nav center, search + user menu right
- Sticky on scroll with subtle shadow
- Mobile: hamburger menu, logo center

### Homepage Layout
1. Breaking news ticker (if active) below header
2. Hero section: Featured story (large image, headline overlay)
3. Top Stories: 3-column grid with image cards
4. Category Sections (repeatable):
   - Section header with category name + "See All" link
   - 4-card horizontal row with thumbnails
5. Opinion/Latest sidebar (desktop only)
6. Newsletter signup section

### Category Page
- Category hero banner with name + description
- Filter bar: Sort (Latest, Popular), Date range
- Infinite scroll article grid (3 columns desktop, 2 tablet, 1 mobile)
- Load more button as fallback

### Article Page
- Full-width hero image
- Article header: Category tag, headline, byline, date, reading time
- Body content with pull quotes, embedded media
- Share buttons (floating left sidebar desktop, bottom bar mobile)
- Related articles grid at bottom
- Comment section (if enabled)

### Search Results
- Search bar prominent at top
- Filters: Category, Date range, Author
- Results list with highlighted matching text

### Responsive Breakpoints
- Desktop: 1024px+
- Tablet: 768px - 1023px
- Mobile: < 768px

## 4. Features & Interactions

### Navigation
- Primary nav: Home, World, Politics, Business, Tech, Sports, Culture, Opinion
- Dropdown on hover (desktop) / tap to expand (mobile)
- Active state: underline accent color

### Breaking News Banner
- Red background, white text, "BREAKING" label
- Auto-scrolls headlines if multiple
- Dismiss button (X) stores preference for session

### Article Cards
- Hover: image scale 1.03, shadow lift, 300ms ease-out
- Click: navigate to article
- Category tag clickable to category page

### Search
- Click search icon: modal overlay appears
- Input auto-focuses
- Debounced search (300ms) shows live results
- Escape or click outside closes modal

### Newsletter Signup
- Email input + "Subscribe" button
- Validation: valid email format required
- Success: input replaced with confirmation message
- Error: red border, error message below

### Article Sharing
- Platforms: Twitter, Facebook, LinkedIn, Copy Link
- Copy shows toast notification "Link copied"

### User Authentication
- Login/Signup modal
- Methods: Email + Password, Google OAuth
- Form validation inline
- Login success: modal closes, header updates to show user avatar
- Protected actions (bookmarks, comments): prompt login if not authenticated

### Bookmarks
- Click bookmark icon on article card/article page
- Saves to user profile
- Bookmarks page shows saved articles in grid

### Comments (if enabled)
- Text area for new comment
- Submit: requires authentication
- Comments displayed newest first
- Reply threads nested 1 level

### Error States
- 404: "Page not found" with search bar and popular articles
- Network error: "Connection failed" with retry button
- Empty search: "No results for [query]" with suggestions

## 5. Component Inventory

### Breaking Banner
- States: visible (red bg, scrolling text), dismissed (hidden)
- Animation: fade in 200ms

### Article Card (Standard)
- Elements: image, category tag, headline, excerpt, byline, date
- States: default, hover (scale + shadow), loading (skeleton)

### Article Card (Featured)
- Larger image, bigger headline
- States: same as standard

### Navigation Link
- States: default (secondary text), hover (primary text + underline), active (accent underline)

### Button
- Primary: accent bg, white text
- Secondary: transparent bg, accent text, accent border
- States: default, hover (darken 10%), active (darken 15%), disabled (50% opacity)

### Search Modal
- Overlay: rgba(0,0,0,0.5)
- Modal: white bg, 90% width max 600px
- States: entering (fade + scale from 0.95), visible, exiting (fade out)

### Toast Notification
- Position: bottom right
- Auto-dismiss: 3 seconds
- Types: success (green accent), error (red accent), info (blue accent)

### Skeleton Loader
- Animated gradient shimmer
- Matches layout of content being loaded

## 6. Technical Requirements

### Libraries
- React 18 for UI components
- Next.js 14 for routing and SSR
- Tailwind CSS for styling
- Zustand for client state (bookmarks, user session)
- Lucide React for icons
- Date-fns for date formatting

### API Design
- GET /api/articles - list with pagination, filters
- GET /api/articles/:slug - single article
- GET /api/categories - all categories
- GET /api/search?q= - search articles
- POST /api/auth/login - email login
- POST /api/auth/register - email registration
- POST /api/auth/google - Google OAuth
- GET /api/user/bookmarks - user bookmarks
- POST /api/articles/:id/bookmark - toggle bookmark

### Data Model

| Entity | Fields |
|--------|--------|
| User | id, email, name, avatar_url, created_at, role |
| Article | id, slug, title, excerpt, content, featured_image, category_id, author_id, published_at, reading_time, is_breaking |
| Category | id, name, slug, description |
| Author | id, name, bio, avatar_url |
| Comment | id, article_id, user_id, content, created_at, parent_id |
| Bookmark | user_id, article_id, created_at |

## 7. Animation Specifications

### Page Load
- Content fades in: opacity 0->1, translateY 20px->0, 400ms ease-out, staggered 50ms

### Cards
- Hover scale: transform scale(1.03), 300ms ease-out
- Hover shadow: box-shadow increase, 300ms ease-out

### Modal
- Enter: opacity 0->1, scale 0.95->1, 200ms ease-out
- Exit: opacity 1->0, 150ms ease-in

### Breaking Banner
- Slide down from top: translateY -100%->0, 300ms ease-out
- Dismiss: translateY 0->-100%, 200ms ease-in

### Toast
- Enter: opacity 0->1, translateX 100%->0, 200ms ease-out
- Exit: opacity 1->0, 150ms ease-in

## 8. Performance & Accessibility

### Metrics
- LCP target: under 2.5s
- FID target: under 100ms
- CLS target: under 0.1
- Image lazy loading for below-fold images

### Techniques
- Next.js Image component for optimized images
- Code splitting per route
- Service worker for caching (optional)

### Accessibility
- Semantic HTML: article, nav, main, header, footer
- ARIA labels on interactive elements
- Focus visible outlines
- Keyboard navigation for all interactions
- prefers-reduced-motion: disable animations, keep essential transitions only (150ms max)
- Color contrast: minimum 4.5:1 for body text, 3:1 for large text