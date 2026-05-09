# Community PRD

## 1. Project Overview

### Project Name
Community Hub

### Project Type
Forum and Social Discussion Platform

### Core Functionality
A full-featured community forum where users can create posts, engage in threaded discussions, vote on content, build reputation through contributions, and discover topics through categories and tags.

### Target Users
- Guest visitors (read-only access to public content)
- Registered members (post, comment, vote, build reputation)
- Moderators (enforce community guidelines, manage posts/comments)
- Administrators (manage categories, users, platform settings)

### Target Platforms
Desktop browsers (primary), tablet and mobile browsers (responsive)

---

## 2. Visual Identity

### Color Palette

| Color | Hex | Purpose |
|-------|-----|---------|
| Primary | #2563EB | Buttons, links, active states |
| Primary Dark | #1D4ED8 | Hover states for primary |
| Secondary | #64748B | Secondary text, icons |
| Background | #0F172A | Main background (dark mode) |
| Surface | #1E293B | Cards, panels, elevated surfaces |
| Surface Light | #334155 | Borders, dividers |
| Text Primary | #F8FAFC | Main text color |
| Text Secondary | #94A3B8 | Muted text, metadata |
| Accent Success | #10B981 | Upvotes, success states |
| Accent Danger | #EF4444 | Downvotes, error states, destructive actions |
| Accent Warning | #F59E0B | Warnings, pending states |
| Accent Gold | #FBBF24 | Badges, achievements, highlights |

### Typography

| Element | Font Family | Weight | Size | Line Height |
|---------|-------------|--------|------|-------------|
| Heading 1 | Inter | 700 | 32px | 1.2 |
| Heading 2 | Inter | 600 | 24px | 1.3 |
| Heading 3 | Inter | 600 | 20px | 1.4 |
| Body | Inter | 400 | 16px | 1.6 |
| Small | Inter | 400 | 14px | 1.5 |
| Caption | Inter | 400 | 12px | 1.4 |
| Monospace | JetBrains Mono | 400 | 14px | 1.5 |

### Spacing System

| Token | Value | Usage |
|-------|-------|-------|
| xs | 4px | Tight gaps, inline spacing |
| sm | 8px | Between related elements |
| md | 16px | Standard padding, gaps |
| lg | 24px | Section spacing |
| xl | 32px | Major section breaks |
| 2xl | 48px | Page section separation |

### Border Radius

| Token | Value | Usage |
|-------|-------|-------|
| sm | 4px | Badges, small elements |
| md | 8px | Buttons, inputs, cards |
| lg | 12px | Large cards, modals |
| full | 9999px | Avatars, pills |

---

## 3. Layout Structure

### Page Architecture

#### Global Header
- Height: 64px fixed
- Logo (left): 32px height, links to home
- Navigation (center): Home, Categories, Users, Search icon, user menu
- Search bar: Expandable on click, 320px max width
- User menu (right): Avatar (32px), dropdown with profile/settings/logout
- Notification bell with unread count badge

#### Global Footer
- Height: 200px
- Four columns: About, Categories, Resources, Legal
- Social media links
- Copyright and version info

### Page Layouts

#### Home Page (/)
- Hero section: 400px height, featured/pinned posts carousel
- Main content (70%): Post feed with infinite scroll
- Sidebar (30%): Trending topics, top contributors, community stats
- Gap between posts: 16px

#### Category Page (/c/:category)
- Category header: 200px height, banner image, title, description, subscriber count
- Filter bar: Sort (New/Hot/Top/Rising), Time filter (24h/Week/Month/Year/All)
- Post list with sticky sidebar

#### Post Detail Page (/p/:postId)
- Post content: Full width, max 800px centered
- Vote column (left): Vertical upvote/downvote, score, bookmark
- Post body with rich text, images, embedded media
- Tag list below post
- Action bar: Share, Save, Report, Comments count
- Comments section: Nested up to 10 levels, with "Continue thread" links

#### User Profile Page (/u/:username)
- Profile header: 240px height, cover image, avatar (120px), display name, username, join date
- Stats bar: Karma, Posts, Comments, trophies
- Tab navigation: Posts, Comments, Saved, Hidden, Awards
- Activity feed with pagination

#### Create Post Page (/submit)
- Form: Title input (300 char limit), body editor (markdown/wysiwyg toggle)
- Category selector: Dropdown with search, required
- Tag selector: Multi-select, max 5 tags, autocomplete
- Preview toggle
- Submit button with loading state

---

## 4. User Roles and Permissions

### Role Hierarchy

| Role | Read | Post | Comment | Vote | Moderate | Admin |
|------|------|------|---------|------|----------|-------|
| Guest | Yes | No | No | No | No | No |
| User | Yes | Yes | Yes | Yes | No | No |
| Moderator | Yes | Yes | Yes | Yes | Yes | No |
| Admin | Yes | Yes | Yes | Yes | Yes | Yes |

### Moderator Capabilities
- Edit/delete any post or comment in their assigned categories
- Remove posts/comments (soft delete with reason)
- Lock threads (prevent new comments)
- Pin posts to category top
- View user notes and history
- Issue warnings and temporary suspensions

### Admin Capabilities
- All moderator capabilities
- Manage categories (create, edit, delete, merge)
- Manage users (promote/demote roles, permanent ban)
- Manage global settings
- View analytics dashboard
- Access audit logs

---

## 5. Authentication Flow

### Sign Up
1. User clicks "Sign Up" button
2. Modal opens with fields: Email, Username (3-20 chars, alphanumeric + underscore), Password (min 8 chars)
3. Email verification required before full access
4. Welcome email with onboarding tips
5. Optional: Username suggestions based on email

### Sign In
1. User clicks "Sign In"
2. Modal with Email and Password fields
3. "Forgot Password" link for reset flow
4. "Remember me" checkbox (extends session to 30 days)
5. OAuth options: Google, GitHub (optional future feature)

### Session Management
- JWT access token: 15 minute expiry, stored in memory
- Refresh token: 7 day expiry, httpOnly cookie
- CSRF protection via SameSite cookie attribute
- Concurrent session limit: 3 devices

### Password Reset Flow
1. User enters email on forgot password page
2. Email sent with unique reset token (1 hour expiry)
3. User clicks link, enters new password
4. All existing sessions revoked after password change

---

## 6. Core Features

### Post Creation Flow
1. User clicks "Create Post" button (visible to logged-in users)
2. Post editor page loads
3. User enters title (required, 10-300 chars)
4. User selects post type: Text, Link, Image, Poll
5. User selects category (required)
6. User adds tags (optional, 0-5 tags)
7. User enters body content based on type:
   - Text: Markdown editor with preview
   - Link: URL input with metadata preview (title, description, image)
   - Image: Drag-drop upload, max 5 images, 10MB each, supports JPEG/PNG/WebP
   - Poll: Question + 2-6 options, optional end date
8. User clicks "Submit"
9. Client-side validation runs
10. POST request to /api/posts
11. Server validates, creates post, returns 201 with post object
12. Redirect to post detail page
13. Success toast notification

### Voting Flow
1. User clicks upvote or downvote button
2. Immediate optimistic UI update (score changes)
3. POST request to /api/posts/:postId/vote or /api/comments/:commentId/vote
4. Request body: { vote: 1 | -1 | 0 }
5. Server validates user permission, calculates new score
6. If user already voted same direction, vote is removed (toggle behavior)
7. If user votes opposite direction, vote is changed
8. Response returns new vote state and score
9. On error, UI reverts to previous state with error toast

### Commenting Flow
1. User scrolls to comment section or clicks "Comment" button
2. Comment editor appears at top of thread (for root comments)
3. User types comment using markdown
4. User clicks "Reply" on existing comment to nest
5. Nested reply editor appears below parent
6. User submits reply
7. Comment appears in thread with 300ms fade-in animation
8. Parent comment shows reply count increment

### Thread Nesting Rules
- Maximum nesting depth: 10 levels
- Beyond level 10, "Continue this thread" link appears
- Collapsed threads show "[+] X more replies"
- User can collapse/expand any thread level
- Collapsed state persisted in localStorage

### Search Flow
1. User clicks search icon or presses "/" key
2. Search bar expands with focus
3. User types query (min 3 chars)
4. After 300ms debounce, search request sent
5. Results grouped by: Posts, Comments, Users, Categories
6. Each result shows: Title/snippet, author, category, timestamp, score
7. User can filter by type using tabs
8. Clicking result navigates to that content

### Filtering and Sorting
- Sort options: New (default), Hot (score + recency algorithm), Top (score only), Rising (new posts with momentum)
- Time filters: 24h, Week, Month, Year, All
- Category filter: Single category or multi-select
- Tag filter: Multi-select with OR logic
- Saved filters: User can save filter combinations

### Reputation/Karma System
- Post upvote: +10 karma to author
- Post downvote: -2 karma to author
- Comment upvote: +5 karma to author
- Comment downvote: -1 karma to author
- karma displayed on profile and next to username
- Karma thresholds unlock features (see Badges section)

---

## 7. Data Model

### Users Table

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | Primary Key |
| email | VARCHAR(255) | Unique, Not Null |
| username | VARCHAR(20) | Unique, Not Null |
| display_name | VARCHAR(50) | Not Null |
| password_hash | VARCHAR(255) | Not Null |
| avatar_url | VARCHAR(500) | Nullable |
| cover_url | VARCHAR(500) | Nullable |
| bio | TEXT | Nullable |
| karma | INTEGER | Default 0 |
| role | ENUM | 'user', 'moderator', 'admin' |
| created_at | TIMESTAMP | Default NOW() |
| updated_at | TIMESTAMP | Default NOW() |
| last_active_at | TIMESTAMP | Nullable |
| is_verified | BOOLEAN | Default false |
| is_suspended | BOOLEAN | Default false |

### Categories Table

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | Primary Key |
| name | VARCHAR(50) | Unique, Not Null |
| slug | VARCHAR(50) | Unique, Not Null |
| description | TEXT | Nullable |
| icon | VARCHAR(50) | Nullable |
| color | VARCHAR(7) | Hex color |
| banner_url | VARCHAR(500) | Nullable |
| parent_id | UUID | Foreign Key (self) |
| post_count | INTEGER | Default 0 |
| created_at | TIMESTAMP | Default NOW() |
| is_private | BOOLEAN | Default false |

### Posts Table

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | Primary Key |
| title | VARCHAR(300) | Not Null |
| body | TEXT | Nullable |
| post_type | ENUM | 'text', 'link', 'image', 'poll' |
| url | VARCHAR(500) | Nullable |
| user_id | UUID | Foreign Key |
| category_id | UUID | Foreign Key |
| score | INTEGER | Default 0 |
| comment_count | INTEGER | Default 0 |
| view_count | INTEGER | Default 0 |
| is_pinned | BOOLEAN | Default false |
| is_locked | BOOLEAN | Default false |
| created_at | TIMESTAMP | Default NOW() |
| updated_at | TIMESTAMP | Default NOW() |

### Comments Table

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | Primary Key |
| body | TEXT | Not Null |
| user_id | UUID | Foreign Key |
| post_id | UUID | Foreign Key |
| parent_id | UUID | Foreign Key (self), Nullable |
| score | INTEGER | Default 0 |
| depth | INTEGER | Default 0 |
| is_deleted | BOOLEAN | Default false |
| created_at | TIMESTAMP | Default NOW() |
| updated_at | TIMESTAMP | Default NOW() |

### Votes Table

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | Primary Key |
| user_id | UUID | Foreign Key |
| voteable_type | ENUM | 'post', 'comment' |
| voteable_id | UUID | Foreign Key |
| value | INTEGER | -1, 0, or 1 |
| created_at | TIMESTAMP | Default NOW() |
| UNIQUE | | | (user_id, voteable_type, voteable_id) |

### Tags Table

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | Primary Key |
| name | VARCHAR(30) | Unique, Not Null |
| slug | VARCHAR(30) | Unique, Not Null |
| post_count | INTEGER | Default 0 |

### PostTags Table

| Field | Type | Constraints |
|-------|------|-------------|
| post_id | UUID | Foreign Key |
| tag_id | UUID | Foreign Key |
| PRIMARY KEY | | (post_id, tag_id) |

### Badges Table

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | Primary Key |
| name | VARCHAR(50) | Unique, Not Null |
| description | VARCHAR(200) | Not Null |
| icon | VARCHAR(50) | Icon name |
| tier | ENUM | 'bronze', 'silver', 'gold', 'platinum' |
| requirement_type | ENUM | 'karma', 'posts', 'comments', 'votes' |
| requirement_value | INTEGER | Threshold |

### UserBadges Table

| Field | Type | Constraints |
|-------|------|-------------|
| user_id | UUID | Foreign Key |
| badge_id | UUID | Foreign Key |
| earned_at | TIMESTAMP | Default NOW() |
| PRIMARY KEY | | (user_id, badge_id) |

### Notifications Table

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | Primary Key |
| user_id | UUID | Foreign Key |
| type | ENUM | 'reply', 'mention', 'vote', 'mod_action', 'badge' |
| message | VARCHAR(500) | Not Null |
| link | VARCHAR(500) | Target URL |
| is_read | BOOLEAN | Default false |
| created_at | TIMESTAMP | Default NOW() |

### ModerationLog Table

| Field | Type | Constraints |
|-------|------|-------------|
| id | UUID | Primary Key |
| moderator_id | UUID | Foreign Key |
| action_type | ENUM | 'remove', 'lock', 'pin', 'warn', 'suspend' |
| target_type | ENUM | 'post', 'comment', 'user' |
| target_id | UUID | Foreign Key |
| reason | TEXT | Nullable |
| created_at | TIMESTAMP | Default NOW() |

### Entity Relationships

```
Users 1..* Posts
Users 1..* Comments
Users 1..* Votes
Users 1..* Notifications
Users 1..* UserBadges
Categories 1..* Posts
Posts 1..* Comments
Posts 1..* PostTags
Posts 1..* Votes
Comments 1..* Votes
Tags *..* Posts
Badges 1..* UserBadges
Users 1..* ModerationLog (as moderator)
```

---

## 8. API Design

### Authentication Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | /api/auth/register | Create new user account |
| POST | /api/auth/login | Authenticate user, return tokens |
| POST | /api/auth/logout | Invalidate refresh token |
| POST | /api/auth/refresh | Refresh access token |
| POST | /api/auth/forgot-password | Send password reset email |
| POST | /api/auth/reset-password | Reset password with token |

### Post Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/posts | List posts with pagination, filters, sort |
| GET | /api/posts/:id | Get single post with author info |
| POST | /api/posts | Create new post |
| PUT | /api/posts/:id | Update own post |
| DELETE | /api/posts/:id | Soft delete own post |
| POST | /api/posts/:id/vote | Vote on post |
| POST | /api/posts/:id/bookmark | Bookmark post |

### Comment Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/posts/:postId/comments | Get comments for post |
| POST | /api/posts/:postId/comments | Create comment |
| PUT | /api/comments/:id | Update own comment |
| DELETE | /api/comments/:id | Soft delete own comment |
| POST | /api/comments/:id/vote | Vote on comment |

### User Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/users/:username | Get user profile |
| PUT | /api/users/:username | Update own profile |
| GET | /api/users/:username/posts | Get user's posts |
| GET | /api/users/:username/comments | Get user's comments |
| GET | /api/users/:username/karma-history | Get karma change log |

### Category Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/categories | List all categories |
| GET | /api/categories/:slug | Get category with recent posts |
| POST | /api/categories | Create category (admin) |
| PUT | /api/categories/:id | Update category (admin) |

### Search Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/search | Global search |
| GET | /api/search/posts | Search posts only |
| GET | /api/search/users | Search users only |

### Notification Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/notifications | Get user's notifications |
| PUT | /api/notifications/:id/read | Mark as read |
| PUT | /api/notifications/read-all | Mark all as read |

### Pagination Response Format

```json
{
  "data": [],
  "pagination": {
    "page": 1,
    "limit": 25,
    "total": 100,
    "totalPages": 4,
    "hasNext": true,
    "hasPrev": false
  }
}
```

---

## 9. Error Handling

### HTTP Status Codes

| Code | Meaning |
|------|---------|
| 200 | Success |
| 201 | Created |
| 400 | Bad Request (validation error) |
| 401 | Unauthorized (not logged in) |
| 403 | Forbidden (insufficient permissions) |
| 404 | Not Found |
| 409 | Conflict (duplicate resource) |
| 429 | Too Many Requests (rate limited) |
| 500 | Internal Server Error |

### Error Response Format

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Human readable message",
    "details": [
      { "field": "email", "message": "Invalid email format" }
    ]
  }
}
```

### Client-Side Error States

| State | UI Treatment |
|-------|--------------|
| Network Error | Toast: "Connection failed. Check your internet and try again." with Retry button |
| 401 Error | Redirect to login page, toast: "Please sign in to continue" |
| 403 Error | Toast: "You don't have permission to perform this action" |
| 404 Error | Dedicated 404 page with search bar and popular posts |
| 422 Error | Inline field errors highlighted in red below inputs |
| 429 Error | Toast: "Too many requests. Please wait X seconds." with countdown |
| 500 Error | Toast: "Something went wrong. Our team has been notified." with Report button |

### Rate Limiting

| Endpoint | Limit | Window |
|----------|-------|--------|
| POST /api/auth/login | 5 | per minute |
| POST /api/posts | 10 | per hour |
| POST /api/comments | 30 | per hour |
| POST /api/votes | 100 | per hour |
| GET /api/search | 30 | per minute |

---

## 10. Empty States

### No Posts in Feed
- Illustration: Empty inbox icon
- Heading: "No posts yet"
- Body: "Be the first to post in this community!"
- CTA Button: "Create Post" (if logged in) or "Sign Up to Post" (if guest)

### No Search Results
- Illustration: Magnifying glass with X
- Heading: "No results found"
- Body: "Try adjusting your search or filters"
- Suggestions: Popular search terms as clickable chips

### No Notifications
- Illustration: Bell with checkmark
- Heading: "You're all caught up!"
- Body: "Notifications about your posts and comments will appear here"

### No Comments
- Illustration: Speech bubble outline
- Heading: "No comments yet"
- Body: "Start the discussion!"
- CTA: Comment input focused automatically

### User Has No Posts
- Heading: "No posts yet"
- Body: "Posts you create will appear here"
- CTA: "Create Your First Post"

---

## 11. Real-Time Features

### Live Updates
- New posts appearing in feed: 2 second polling or WebSocket
- New comments on viewed post: Real-time via WebSocket
- Vote count changes: Optimistic update with server reconciliation
- Notification bell: Unread count updates in real-time

### WebSocket Events

| Event | Direction | Payload |
|-------|-----------|---------|
| new_post | Server to Client | Post object |
| new_comment | Server to Client | Comment object |
| vote_update | Server to Client | { id, type, score } |
| notification | Server to Client | Notification object |

### Presence Indicators
- Show "User is viewing this post" when multiple users on same post
- Show typing indicator in comment thread when someone is composing

---

## 12. Responsive Behavior

### Breakpoints

| Name | Width | Description |
|------|-------|-------------|
| mobile | < 640px | Phones |
| tablet | 640px - 1024px | Tablets, small laptops |
| desktop | > 1024px | Standard desktops |

### Mobile-Specific Changes

| Element | Desktop | Mobile |
|---------|---------|--------|
| Navigation | Full header with text links | Hamburger menu, slide-out drawer |
| Sidebar | Always visible | Collapsed, accessible via tab |
| Post layout | 70/30 split | Single column |
| Comment nesting | Full width tree | Indented with left border |
| Vote buttons | Horizontal (left of post) | Fixed bottom bar |
| Create post | Modal or separate page | Full screen form |
| Images | Displayed in-post | Lightbox on tap |

### Heavy Visual Handling (Images/Videos)

| Content | Desktop | Mobile |
|---------|---------|--------|
| Post images | Inline, max 800px width | Lightbox on tap, lazy load |
| User avatars | 32px standard | 40px touch target |
| Cover images | Full banner | Cropped to 16:9 top portion |
| Embedded videos | Full width, autoplay allowed | Click to play, no autoplay |

---

## 13. Performance Metrics

### Targets

| Metric | Target |
|--------|--------|
| Largest Contentful Paint (LCP) | < 2.5s |
| First Input Delay (FID) | < 100ms |
| Cumulative Layout Shift (CLS) | < 0.1 |
| Time to Interactive (TTI) | < 3.5s |
| Page load (index) | < 1.5s on 3G |

### Asset Budgets

| Asset Type | Budget |
|------------|--------|
| HTML | < 100KB |
| CSS | < 50KB |
| JavaScript (initial) | < 150KB |
| Images (per post) | < 500KB total |
| Fonts | < 100KB (subset) |

### Performance Techniques

| Technique | Implementation |
|-----------|---------------|
| Lazy loading | Images below fold, comments, infinite scroll items |
| Code splitting | Route-based, separate chunks for editor, rich text |
| Image optimization | WebP with JPEG fallback, responsive srcset |
| Font optimization | Preload critical fonts, font-display: swap |
| Caching | Service worker for static assets, API response caching |
| Virtual scrolling | For post/comment lists > 50 items |
| Debouncing | Search input (300ms), scroll handlers (16ms) |

---

## 14. Accessibility

### Keyboard Navigation
- Tab order: Header -> Main Content -> Sidebar -> Footer
- Escape key: Close modals, dropdowns, cancel actions
- Arrow keys: Navigate within dropdowns, nested comments
- Enter: Activate buttons, expand collapsed threads
- "/" key: Focus search input from anywhere

### Screen Reader Support
- Semantic HTML elements (nav, main, article, aside, footer)
- ARIA labels for icon-only buttons
- Live regions for dynamic content (new posts, notifications)
- Role and state attributes for custom components

### Color Contrast
- All text meets WCAG AA (4.5:1 for body, 3:1 for large text)
- Interactive elements have visible focus states (2px outline, offset)
- Error states not conveyed by color alone (icon + text)

### Reduced Motion (prefers-reduced-motion)
- All animations disabled or reduced to opacity-only fades
- Carousels become static displays
- Vote buttons change color without scale animation
- Page transitions are instant
- Scroll reveals happen immediately

---

## 15. Notifications System

### Notification Types

| Type | Trigger | Content |
|------|---------|---------|
| reply | Someone replies to your post or comment | "{username} replied to your {type}" |
| mention | Username mentioned in post/comment | "{username} mentioned you" |
| vote | Post/comment reaches vote threshold | "Your {type} reached {n} upvotes" |
| mod_action | Moderator action on your content | "Your post was {action} for {reason}" |
| badge | New badge earned | "You earned the {badge_name} badge!" |

### Notification Preferences (User Settings)
- Enable/disable each notification type
- Email digest frequency: Immediate, Daily, Weekly, Never
- Push notifications (if PWA): Separate toggle per type

---

## 16. Moderation Tools

### Content Moderation
- mod.log: Full history of moderator actions
- Quick actions: Remove, Lock, Pin, Spam (with confirmation)
- Removal reason: Required dropdown + optional text
- Appeals process: User can appeal within 7 days

### User Moderation
- Warning system: Template + custom message
- Temporary suspension: 1, 7, 30 days with reason
- Permanent ban: Reserved for severe violations
- User notes: Internal notes visible to all mods/admins

### Spam Detection (Automated)
- Rate limiting enforcement
- Link filtering (configurable domain blacklist)
- Repeated content detection
- New user posting limits (first 24 hours)

---

## 17. Badges and Achievements

### Badge Tiers and Examples

| Tier | Name | Requirement | Description |
|------|------|-------------|-------------|
| Bronze | First Post | Create 1 post | Posted your first content |
| Bronze | Commenter | Leave 10 comments | Engaged in discussions |
| Silver | Trusted | Receive 100 upvotes total | Quality contributions recognized |
| Silver | Regular | Login 30 different days | Dedicated community member |
| Gold | Notable Post | Post reaches 500 score | Your post gained significant traction |
| Gold | Helper | Receive 50 best answers | Your answers were highly valued |
| Platinum | Veteran | 1 year membership | Sustained community contribution |
| Platinum | Legend | 10,000 karma | Top contributor status |

### Badge Display
- Profile page: Grid of earned badges
- Next to username: Top badge icon
- Hover: Badge name and description tooltip
- Milestone celebrations: Special toast for first badge