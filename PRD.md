# PRD - News Portal

## 1. Concept & Vision

A premium news portal delivering breaking stories, in-depth articles, and editorial content across topics like World, Politics, Sports, Business, and Culture. The experience feels authoritative yet modern — like a refined digital version of a broadsheet newspaper. Users consume content through a structured grid-based layout with strong typographic hierarchy, real-time breaking news alerts, and seamless infinite scroll pagination.

Target audience: General news consumers aged 25-55 seeking credible, well-organized journalism. Admin editors publish and manage content through a headless CMS backend.

## 2. Visual Identity

### Typography
- **Headlines:** Playfair Display (Google Fonts), Bold 700, sizes: H1 48px/H2 36px/H3 28px/H4 22px
- **Body:** Source Sans 3 (Google Fonts), Regular 400, 18px line-height 1.6
- **UI/Meta:** Source Sans 3, Medium 500, 14px
- **Bylines/Dates:** Source Sans 3, Regular 400, 13px, color #6B7280

### Color Palette
- **Primary:** #1A1A1A (text/headlines)
- **Secondary:** #374151 (body text)
- **Accent:** #DC2626 (breaking news, live indicators)
- **Background:** #FFFFFF (main), #F9FAFB (sections)
- **Link:** #2563EB
- **Border:** #E5E7EB
- **Category Tags:** Politics #1D4ED8, Sports #059669, Business #D97706, Culture #7C3AED, World #6B7280

### Spacing System
- Base unit: 8px
- Section padding: 64px vertical, 24px horizontal
- Card gap: 24px
- Content max-width: 1280px
- Article body max-width: 720px

### Motion
- Page transitions: Fade 200ms ease-out
- Card hover lift: translateY(-4px), 150ms ease
- Breaking news pulse: 2s infinite glow animation
- Scroll reveal: opacity 0->1, translateY(20px->0), 400ms ease-out, staggered 80ms

## 3. Layout & Structure

### Homepage
```
[Breaking News Banner - 48px height, red background, marquee text]
[Header - Logo left, nav center (World|Politics|Sports|Business|Culture), search right]
[Hero Section - 2-column: Featured story (65%) + 2 secondary stories (35%)]
[Category Tabs - horizontal scrollable on mobile]
[Content Grid - 3-column card layout, 16px gap]
[Sidebar - Trending stories, Most Read list]
[Footer - 4-column links, newsletter signup]
```

### Article Page
```
[Header - same global header]
[Article Header - Category tag, Headline (H1), Byline, Date, Reading time]
[Featured Image - full-width, max-height 560px, object-fit cover]
[Article Body - 720px max-width, centered, 18px Source Sans 3]
[Share Bar - sticky left sidebar, vertical icons]
[Related Articles - 3-card grid]
[Comments Section - thread display, reply form]
[Footer]
```

### Category Page
```
[Header]
[Category Hero - colored banner with category name, article count]
[Filter Bar - Sort (Latest|Popular|Oldest), Date range picker]
[Article Grid - infinite scroll, 3-column desktop, 2-column tablet, 1-column mobile]
[Load More button - appears after 3s if no scroll trigger]
[Footer]
```

### Search Results Page
```
[Search Bar - prominent, pre-filled with query]
[Results Count - "124 results for 'climate'"]
[Filters Panel - left sidebar: Category checkboxes, Date slider, Author dropdown]
[Results Grid - list view with thumbnail, headline, excerpt, date]
[Pagination - numbered pages, prev/next arrows]
```

## 4. Features & Interactions

### Breaking News Banner
- Scrolling ticker with latest headlines, separated by red bullet
- Click headline -> opens article in new tab
- "LIVE" badge pulses with red glow animation
- Dismiss button (X) saves dismissal to sessionStorage, hides for 30 minutes

### Navigation
- Desktop: horizontal nav, category dropdowns on hover (150ms delay)
- Mobile: hamburger menu, full-screen overlay with slide-in animation (300ms ease-out)
- Active category highlighted with 3px bottom border in accent color
- Sticky header on scroll, shrinks from 80px to 60px height, 200ms ease

### Search
- Click search icon -> search bar expands from 0 to 320px width, 250ms ease
- Real-time suggestions appear after 300ms debounce, max 8 results
- Keyboard navigation: arrow keys cycle suggestions, Enter selects, Escape closes
- Empty state: "Start typing to search articles..." in gray text
- No results: "No articles found for '[query]'. Try different keywords."

### Article Cards
- Thumbnail image (16:9 ratio), lazy-loaded with blur placeholder
- Category tag (colored badge), headline, excerpt (2 lines clamped), byline, date
- Hover: card lifts 4px, shadow deepens, image scales to 1.05
- Click anywhere -> navigates to article

### Article Page
- Reading progress bar: fixed top, 3px height, accent color fill
- Estimated reading time calculated: words / 200 wpm
- Share buttons: Twitter, Facebook, LinkedIn, Copy Link (toast confirmation)
- Bookmark icon: hollow -> filled on click, saves to localStorage
- "Back to [Category]" breadcrumb link

### Comments
- Threaded replies (max 2 levels deep)
- "Reply" button expands inline reply form
- Character limit: 2000, counter shows remaining
- Submit disabled until 10+ characters
- Success: comment fades in, form clears
- Error: red border, error message below field

### Newsletter Signup (Footer)
- Email input + "Subscribe" button, inline layout
- Validation: email format check
- Submit: button shows spinner, text changes to "Subscribing..."
- Success: input clears, green checkmark, "Thanks for subscribing!" message
- Error: red border, "Please enter a valid email" message

### Admin Panel (Backend)
- Protected by JWT auth, role-based access (admin, editor)
- CRUD operations for articles, categories, authors
- Media uploader with drag-drop, progress bar
- Draft/Publish workflow with preview
- Article scheduling with date-time picker

## 5. Component Inventory

### Breaking News Banner
- States: visible (default), dismissed (hidden for 30min), minimized (single line)

### Global Header
- States: expanded (top), shrunk (scrolled), mobile-menu-open

### Navigation Link
- States: default, hover (underline animation), active (bottom border)

### Article Card
- States: default, hover (lifted), loading (skeleton shimmer)
- Variants: featured (larger), standard (grid), compact (list/sidebar)

### Search Bar
- States: collapsed, expanded, loading (spinner), has-results, no-results

### Category Tag
- States: default, hover (darken 10%)
- Each category has assigned color

### Share Button
- States: default, hover (scale 1.1), active (pressed), copied (checkmark, 2s)

### Bookmark Button
- States: unbookmarked (outline), bookmarked (filled), loading

### Comment
- States: default, editing, deleting (confirm modal), replied

### Newsletter Form
- States: default, loading, success, error (validation)

### Button
- States: default, hover, active, disabled, loading
- Variants: primary (accent bg), secondary (outline), ghost (text only)

### Input Field
- States: default, focused (blue border), error (red border + message), disabled

## 6. Technical Approach

### Frontend Stack
- Framework: Next.js 14 (App Router)
- Styling: Tailwind CSS 3.4
- Animation: Framer Motion 11
- State: Zustand for global state (bookmarks, user session)
- Data Fetching: React Query for server state
- Icons: Lucide React

### Backend (Headless CMS)
- CMS: Sanity.io or Contentful
- GraphQL API for content delivery
- CDN: Cloudflare for asset delivery
- Image CDN: Shopify CDN or Cloudinary

### Authentication
- JWT tokens stored in httpOnly cookies
- OAuth providers: Google, Apple (optional)
- Session refresh on app load

### Data Model

| Entity | Fields |
|--------|--------|
| User | id, email, name, avatar, role (guest/editor/admin), createdAt |
| Article | id, title, slug, excerpt, body (rich text), featuredImage, categoryId, authorId, publishedAt, status (draft/published/scheduled), readingTime, viewCount |
| Category | id, name, slug, color, description, articleCount |
| Author | id, name, bio, avatar, socialLinks |
| Comment | id, articleId, userId, parentId, body, createdAt |
| Bookmark | id, userId, articleId, createdAt |

### API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/articles | List articles (paginated, filterable) |
| GET | /api/articles/:slug | Single article with full body |
| GET | /api/categories | List all categories |
| GET | /api/search?q=&category=&dateFrom=&dateTo= | Search articles |
| POST | /api/comments | Create comment |
| POST | /api/bookmarks | Add bookmark |
| DELETE | /api/bookmarks/:articleId | Remove bookmark |
| POST | /api/newsletter | Subscribe email |

### Performance
- Target: 90+ Lighthouse score
- LCP: < 2.5s
- CLS: < 0.1
- Images: WebP format, srcset for responsive sizes, lazy loading
- Code splitting: per route with dynamic imports
- Caching: ISR with 60s revalidation for articles

### Accessibility
- Semantic HTML throughout
- ARIA labels on interactive elements
- Keyboard navigation for all features
- Focus visible outlines (2px blue offset)
- prefers-reduced-motion: disables all animations, uses instant transitions
- Color contrast: AA compliance (4.5:1 for text)