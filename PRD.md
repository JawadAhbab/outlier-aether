# Product Requirements Document: Community Platform

## 1. Project Overview

### 1.1 Project Name
Community Hub

### 1.2 Project Type
Forum and social discussion platform

### 1.3 Core Functionality Summary
A community-driven platform where users create threads, engage in threaded conversations, vote on content, build reputation through participation, and navigate categorized discussions. The platform emphasizes user autonomy, transparent moderation, and gamified engagement through karma systems.

### 1.4 Target Users
- Regular users seeking knowledge sharing and community engagement
- Topic experts contributing authoritative answers
- Community moderators maintaining discussion quality
- Guest visitors browsing public content
- Platform administrators overseeing system health

---

## 2. Visual Identity

### 2.1 Typography

| Element | Font Family | Weight | Size | Line Height |
| :------ | :---------- | :----- | :--- | :---------- |
| Headings H1 | Inter | 700 | 32px | 1.2 |
| Headings H2 | Inter | 600 | 24px | 1.3 |
| Headings H3 | Inter | 600 | 20px | 1.4 |
| Body Text | Inter | 400 | 16px | 1.6 |
| Secondary Text | Inter | 400 | 14px | 1.5 |
| Code/Monospace | JetBrains Mono | 400 | 14px | 1.5 |
| Button Text | Inter | 500 | 14px | 1.0 |

### 2.2 Color Palette

| Color Role | Hex Code | Usage |
| :--------- | :------- | :---- |
| Background Primary | #0F1419 | Main page background |
| Background Secondary | #1A1F26 | Card backgrounds, containers |
| Background Tertiary | #242B35 | Hover states, input fields |
| Text Primary | #FFFFFF | Headings, important text |
| Text Secondary | #8B98A5 | Timestamps, metadata |
| Text Muted | #6B7280 | Placeholders, disabled |
| Accent Blue | #3B82F6 | Links, primary actions |
| Accent Purple | #8B5CF6 | Reputation badges, special highlights |
| Upvote Green | #10B981 | Upvote indicators |
| Downvote Red | #EF4444 | Downvote indicators |
| Warning Orange | #F59E0B | Caution states |
| Border Color | #2D333B | Dividers, card borders |

### 2.3 Spacing System

| Token | Value | Usage |
| :----- | :---- | :---- |
| --space-xs | 4px | Tight element gaps |
| --space-sm | 8px | Compact spacing |
| --space-md | 16px | Standard padding |
| --space-lg | 24px | Section separation |
| --space-xl | 32px | Major section gaps |
| --space-2xl | 48px | Page section dividers |
| --corner-radius-sm | 4px | Badges, small elements |
| --corner-radius-md | 8px | Buttons, inputs |
| --corner-radius-lg | 12px | Cards, containers |
| --corner-radius-full | 9999px | Avatars, pill buttons |

### 2.4 Layout Grid
- Maximum content width: 1280px
- Main content area: 720px
- Sidebar width: 280px
- Card gap: 16px
- Column gap: 24px

---

## 3. User Roles and Permissions

### 3.1 Role Hierarchy

| Role | Description | Permissions |
| :--- | :---------- | :---------- |
| Guest | Unauthenticated visitor | View public threads, browse categories, read comments |
| Registered User | Standard account holder | Post threads, comment, vote, create content, edit own posts |
| Trusted User | Achieved after 100+ karma | Edit community wiki, flag content, reduced spam filters |
| Moderator | Community-appointed | Pin threads, lock discussions, remove content, issue warnings |
| Admin | Platform administrator | Full system access, user bans, theme changes, analytics |

### 3.2 Permission Matrix

| Action | Guest | Registered | Trusted | Moderator | Admin |
| :------ | :---- | :--------- | :------ | :-------- | :---- |
| View public content | Yes | Yes | Yes | Yes | Yes |
| Create threads | No | Yes | Yes | Yes | Yes |
| Comment on threads | No | Yes | Yes | Yes | Yes |
| Upvote/Downvote | No | Yes | Yes | Yes | Yes |
| Edit own posts | No | Yes (24h) | Yes (7d) | Yes (any) | Yes (any) |
| Delete own posts | No | Yes | Yes | Yes | Yes |
| Remove others' content | No | No | No | Yes | Yes |
| Pin/Lock threads | No | No | No | Yes | Yes |
| Ban users | No | No | No | No | Yes |

---

## 4. Authentication Flows

### 4.1 Sign Up Flow

```
Step 1: User clicks "Sign Up" button
  -> Button location: Navigation bar, top-right corner
  -> Button style: Background #3B82F6, text white, padding 12px 24px, corner-radius 8px

Step 2: Modal opens with form fields
  -> Email input field (placeholder: "Enter your email")
  -> Username input field (placeholder: "Choose a username", validation: alphanumeric + underscores, 3-20 chars)
  -> Password input field (placeholder: "Create password", requirements: 8+ chars, 1 uppercase, 1 number)
  -> Confirm password input field
  -> "Sign up with Email" primary button

Step 3: Form validation
  -> Real-time validation on each field blur
  -> Email format check (regex: ^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$)
  -> Username uniqueness check via API call (debounced 300ms)
  -> Password strength indicator (weak/medium/strong with color coding)

Step 4: Submit button clicked
  -> Button enters loading state (spinner icon, disabled)
  -> API call to POST /api/auth/signup
  -> Request body: { email, username, password }

Step 5: Success response
  -> Modal closes
  -> Toast notification: "Welcome to Community Hub! Check your email to verify your account."
  -> User redirected to homepage with logged-in state (username visible in nav)
  -> Email verification banner displayed at top of page

Step 6: Email verification
  -> User clicks link in verification email
  -> GET /api/auth/verify?token=xxx
  -> Success: Account fully activated, banner dismissed
  -> Failure: Error message, option to resend verification email
```

### 4.2 Sign In Flow

```
Step 1: User clicks "Sign In" button
  -> Button location: Navigation bar, top-right corner (next to Sign Up)

Step 2: Modal opens with form fields
  -> Email or Username input field
  -> Password input field
  -> "Remember me" checkbox
  -> "Forgot password?" link
  -> "Sign In" primary button
  -> Divider line with "or continue with"
  -> Social login buttons: Google (white bg, Google logo), GitHub (#242B35 bg, GitHub logo)

Step 3: Form submission
  -> API call to POST /api/auth/signin
  -> Request body: { identifier, password, rememberMe }

Step 4: Success response
  -> JWT token stored in httpOnly cookie (7 days if rememberMe, 24h if not)
  -> Modal closes
  -> Navigation bar updates: avatar replaces Sign In button, dropdown menu appears
  -> User redirected to previous page or homepage
  -> Toast: "Welcome back, {username}!"

Step 5: Error handling
  -> Invalid credentials: "Incorrect email/username or password. Please try again."
  -> Unverified email: "Please verify your email first. Resend verification?"
  -> Locked account: "Account locked due to multiple failed attempts. Try again in 15 minutes."
  -> Too many attempts: "Too many login attempts. Please wait {countdown} seconds."
```

### 4.3 Password Reset Flow

```
Step 1: User clicks "Forgot password?" link
  -> Link styled: color #3B82F6, underline on hover

Step 2: Modal switches to reset form
  -> Email input field
  -> "Send Reset Link" button

Step 3: Email submission
  -> API call to POST /api/auth/password-reset
  -> Request body: { email }

Step 4: Success
  -> "Check your email for a password reset link. The link expires in 1 hour."
  -> Email sent with reset link containing JWT token

Step 5: User clicks reset link in email
  -> Navigates to /reset-password?token=xxx
  -> Token validated (check expiry and usage count < 1)

Step 6: Reset password form
  -> New password input
  -> Confirm new password input
  -> "Reset Password" button

Step 7: Success
  -> Password updated in database (bcrypt hashed)
  -> Token marked as used
  -> User redirected to sign-in page
  -> "Password reset successful. Please sign in with your new password."
```

### 4.4 Session Management

| Aspect | Implementation |
| :------ | :------------- |
| Token storage | httpOnly secure cookie |
| Token type | JWT with 24h expiry (7d for rememberMe) |
| Refresh mechanism | Silent refresh 5 minutes before expiry |
| Logout | DELETE /api/auth/signout, clear cookie, redirect to home |
| Concurrent sessions | Maximum 3 sessions per user, oldest session invalidated |

---

## 5. Core Features and User Flows

### 5.1 Thread Listing and Browsing

```
Step 1: User lands on homepage
  -> Displays "Home" feed showing trending threads
  -> Feed layout: Single column, max-width 720px, centered
  -> Each thread card shows: title, author avatar+username, subreddit, timestamp, vote count, comment count, preview image (if attached)

Step 2: Thread card structure
  -> Card container: background #1A1F26, border-radius 12px, padding 16px, border 1px solid #2D333B
  -> Vote column on left: up arrow (color #6B7280, hover #10B981), vote count (color #8B98A5), down arrow (color #6B7280, hover #EF4444)
  -> Content column on right: title (font-size 18px, font-weight 600, color #FFFFFF), metadata line (author, subreddit, time posted in #8B98A5), comment count with icon

Step 3: Sorting options
  -> Tabs above feed: "Hot" (default), "New", "Top", "Rising"
  -> Active tab: background #242B35, text #FFFFFF, border-bottom 2px #3B82F6
  -> Inactive tabs: text #8B98A5, hover text #FFFFFF

Step 4: Category filtering
  -> Sidebar on left shows category list
  -> Categories: General, Technology, Science, Art, Music, Gaming, News, Help
  -> Each category: icon + name, unread count badge
  -> Click category -> filter feed to that category only

Step 5: User clicks on thread card
  -> Full page navigation to /thread/{thread_id}
  -> Page loads with skeleton loading state during fetch
```

### 5.2 Creating a Thread

```
Step 1: User clicks "Create Post" button
  -> Button location: Navigation bar (when logged in), top of feed
  -> Button style: background #3B82F6, text white, padding 10px 20px, border-radius 8px

Step 2: Create post page opens
  -> Route: /create-post
  -> Full page layout (not modal)

Step 3: Post form displayed
  -> Title input: max 300 characters, character counter shown
  -> Category dropdown: searchable select with category icons
  -> Content textarea: markdown-enabled, toolbar at top (bold, italic, link, code, image, quote)
  -> Image upload zone: drag-and-drop area, accepts jpg/png/gif/webp, max 10MB, max 20 images
  -> Tags input: multi-select, typeahead from existing tags, max 5 tags
  -> "Post" button: disabled until title and content filled

Step 4: Form interaction
  -> Title focus: border color changes to #3B82F6
  -> Markdown preview: toggle button shows live preview below editor
  -> Image upload: progress bar shown per image, thumbnails displayed after upload

Step 5: Form submission
  -> API call to POST /api/threads
  -> Request body: { title, categoryId, content, tags[], images[] }
  -> User redirected to newly created thread page
  -> Success toast: "Your post has been published!"
```

### 5.3 Viewing and Interacting with Thread

```
Step 1: Thread page loads
  -> Header: Thread title (font-size 24px, font-weight 700), author info, timestamp, category badge
  -> Content area: Rendered markdown content, images displayed inline
  -> Action bar below content: Share button, Bookmark button, Report button, Save button
  -> Vote controls: Large vote buttons on left side, large vote count

Step 2: Adding a comment
  -> Comment input at top of comment section
  -> Textarea with "Write a comment..." placeholder, markdown enabled
  -> "Comment" button (disabled until content entered)
  -> Logged-out state: "Sign in to comment" prompt with Sign In/Sign Up buttons

Step 3: Comment display
  -> Comments sorted by: Best (default), New, Controversial, Q&A
  -> Each comment shows: author avatar+username+karma badge, timestamp, content, vote buttons, reply button, share button
  -> Nested replies shown indented (max depth: 10 levels, then "Continue thread" link)

Step 4: Comment interaction
  -> Reply button click: inline reply form expands below comment
  -> Vote: immediate optimistic update, API call in background
  -> Collapse: clicking on comment body collapses/expands replies

Step 5: Comment sorting
  -> Dropdown selector with current sort option
  -> Options: Best, New, Controversial, Q&A, Old
```

### 5.4 Voting System

```
Step 1: User clicks upvote or downvote button
  -> On logged-out: "Sign in to vote" tooltip appears

Step 2: Vote registered
  -> Immediate visual feedback: arrow fills with color (green for up, red for down)
  -> Vote count updates immediately (optimistic update)
  -> API call: POST /api/votes with { targetType: 'thread'|'comment', targetId, value: 1|-1 }

Step 3: Changing vote
  -> Clicking same vote again: removes vote (toggle behavior)
  -> Clicking opposite vote: switches vote (from up to down or vice versa)
  -> API call: PUT /api/votes/{voteId} with new value

Step 4: Vote API response
  -> Success: vote recorded, karma updated for content author
  -> Failure: revert optimistic update, show error toast

Step 5: Karma calculation
  -> Thread upvote: +10 karma to author
  -> Thread downvote: -2 karma to author
  -> Comment upvote: +5 karma to author
  -> Comment downvote: -1 karma to author
  -> Daily karma cap: 500 (prevents vote manipulation)
```

### 5.5 User Profiles

```
Step 1: Clicking on username
  -> Navigation to /user/{username}
  -> Profile header: avatar (120px circle), username, member since date, total karma score, badge row

Step 2: Profile sections (tab navigation)
  -> "Posts" tab: grid of user's threads (sorted newest first)
  -> "Comments" tab: list of user's comments with context (parent thread title shown)
  -> "Saved" tab: user's bookmarked content (private, only visible to self)
  -> "Hidden" tab: user's hidden content (private)
  -> "Awarded" tab: gold/silver/bronze awards received

Step 3: Edit profile (own profile only)
  -> "Edit Profile" button (top right of profile header)
  -> Form fields: Display name, bio (max 500 chars), avatar upload, banner image upload
  -> "Save Changes" button
```

### 5.6 Moderation Tools

```
Step 1: Moderator accesses mod queue
  -> "Mod" link in navigation (visible only to moderators/admins)
  -> /mod/queue shows reported content and removed content awaiting review

Step 2: Mod queue items
  -> Each item shows: content preview, report reason, reporter username, timestamp
  -> Actions per item: "Approve" (restore content), "Remove" (confirm removal), "Ignore" (dismiss report)

Step 3: Thread moderation actions
  -> On thread page, moderators see additional buttons: "Pin", "Lock", "Spoiler", "Mod Note"
  -> Pin: thread appears at top of subreddit
  -> Lock: comments disabled, banner shown "This thread is locked"
  -> Spoiler: content hidden behind "Spoiler" overlay until clicked

Step 4: User moderation
  -> Mod profile page shows: ban history, warning history, karma trend
  -> Actions: "Ban User" (temporary or permanent), "Warn User", "Add Mod Note"
```

---

## 6. Search, Filter, and Sort Capabilities

### 6.1 Search

| Feature | Specification |
| :------- | :------------ |
| Search input | Location: top navigation bar, width 400px, placeholder "Search Community Hub" |
| Search trigger | Press Enter or click search icon |
| Search API | GET /api/search?q={query}&type={all|threads|comments|users}&sort={relevance|date} |
| Results display | Dropdown below input showing top 5 matches while typing (debounced 300ms) |
| Full results page | /search?q={query} with categorized results tabs |
| Filters | Date range (past 24h, week, month, year, all), minimum karma, subreddit |

### 6.2 Filter Options

| Filter | UI Element | Options |
| :------ | :---------- | :------ |
| Category | Horizontal scrollable pill buttons below header | All, General, Technology, Science, Art, Music, Gaming, News, Help |
| Post Type | Dropdown select | All, Text, Link, Image, Video |
| Time Period | Dropdown select | Now, Today, This Week, This Month, This Year, All Time |
| Content Status | Dropdown select | All, Spoiler, NSFW, Pinned, Locked |

### 6.3 Sort Options

| Sort | Description | Implementation |
| :---- | :---------- | :-------------- |
| Hot | Engagement-weighted by time decay | score / (hours_since_post + 2)^1.5 |
| New | Chronological newest first | created_at DESC |
| Top | Highest score overall | vote_score DESC |
| Rising | Recent high-engagement | (score - 2*downvotes) / (hours_since_post + 2)^2 |

---

## 7. Data Model

### 7.1 Entity Relationship Diagram

```
User 1 ----< Vote ---- >1 Content (Thread or Comment)
User 1 ----< Thread ---- >1 Category
User 1 ----< Comment ---- >1 Thread
User 1 ----< Award ---- >1 Content
User 1 ----< Report ---- >1 Content
User 1 ----< Ban
Thread 1 ----< Comment
Category 1 ----< Thread
Tag * ----< Thread
```

### 7.2 Database Schema Tables

#### Users Table

| Column | Type | Constraints | Description |
| :----- | :--- | :---------- | :---------- |
| id | UUID | PRIMARY KEY | Unique user identifier |
| username | VARCHAR(20) | UNIQUE, NOT NULL | User's display name |
| email | VARCHAR(255) | UNIQUE, NOT NULL | User's email address |
| password_hash | VARCHAR(255) | NOT NULL | Bcrypt hashed password |
| display_name | VARCHAR(50) | NULL | Optional display name |
| bio | TEXT | NULL | User biography |
| avatar_url | VARCHAR(500) | NULL | Avatar image URL |
| banner_url | VARCHAR(500) | NULL | Profile banner URL |
| role | ENUM | DEFAULT 'registered' | guest, registered, trusted, moderator, admin |
| karma | INTEGER | DEFAULT 0 | User's total karma score |
| created_at | TIMESTAMP | DEFAULT NOW() | Account creation date |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last profile update |
| email_verified | BOOLEAN | DEFAULT FALSE | Email verification status |
| is_banned | BOOLEAN | DEFAULT FALSE | Ban status |
| ban_expires_at | TIMESTAMP | NULL | Ban expiration (NULL = permanent) |
| last_active_at | TIMESTAMP | DEFAULT NOW() | Last activity timestamp |

#### Categories Table

| Column | Type | Constraints | Description |
| :----- | :--- | :---------- | :---------- |
| id | UUID | PRIMARY KEY | Unique category identifier |
| name | VARCHAR(50) | UNIQUE, NOT NULL | Category display name |
| slug | VARCHAR(50) | UNIQUE, NOT NULL | URL-friendly identifier |
| description | TEXT | NULL | Category description |
| icon | VARCHAR(50) | NULL | Icon class or emoji |
| color | VARCHAR(7) | NULL | Hex color code for category badge |
| thread_count | INTEGER | DEFAULT 0 | Total threads in category |
| sort_order | INTEGER | DEFAULT 0 | Display order |

#### Threads Table

| Column | Type | Constraints | Description |
| :----- | :--- | :---------- | :---------- |
| id | UUID | PRIMARY KEY | Unique thread identifier |
| title | VARCHAR(300) | NOT NULL | Thread title |
| content | TEXT | NULL | Thread body (markdown) |
| author_id | UUID | FOREIGN KEY (users.id), NOT NULL | Author reference |
| category_id | UUID | FOREIGN KEY (categories.id), NOT NULL | Category reference |
| type | ENUM | DEFAULT 'text' | text, link, image, video |
| url | VARCHAR(500) | NULL | External link for link type |
| is_pinned | BOOLEAN | DEFAULT FALSE | Pinned status |
| is_locked | BOOLEAN | DEFAULT FALSE | Locked status |
| is_spoiler | BOOLEAN | DEFAULT FALSE | Spoiler content flag |
| is_removed | BOOLEAN | DEFAULT FALSE | Removed by moderator |
| removal_reason | TEXT | NULL | Reason for removal |
| view_count | INTEGER | DEFAULT 0 | View counter |
| upvote_count | INTEGER | DEFAULT 0 | Cached upvote count |
| downvote_count | INTEGER | DEFAULT 0 | Cached downvote count |
| comment_count | INTEGER | DEFAULT 0 | Cached comment count |
| created_at | TIMESTAMP | DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last edit timestamp |

#### Comments Table

| Column | Type | Constraints | Description |
| :----- | :--- | :---------- | :---------- |
| id | UUID | PRIMARY KEY | Unique comment identifier |
| content | TEXT | NOT NULL | Comment body (markdown) |
| author_id | UUID | FOREIGN KEY (users.id), NOT NULL | Author reference |
| thread_id | UUID | FOREIGN KEY (threads.id), NOT NULL | Parent thread reference |
| parent_id | UUID | FOREIGN KEY (comments.id), NULL | Parent comment for nesting |
| depth | INTEGER | DEFAULT 0 | Nesting level (max 10) |
| upvote_count | INTEGER | DEFAULT 0 | Cached upvote count |
| downvote_count | INTEGER | DEFAULT 0 | Cached downvote count |
| is_removed | BOOLEAN | DEFAULT FALSE | Removed by moderator |
| is_locked | BOOLEAN | DEFAULT FALSE | Locked status |
| created_at | TIMESTAMP | DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last edit timestamp |

#### Votes Table

| Column | Type | Constraints | Description |
| :----- | :--- | :---------- | :---------- |
| id | UUID | PRIMARY KEY | Unique vote identifier |
| user_id | UUID | FOREIGN KEY (users.id), NOT NULL | Voter reference |
| target_type | ENUM | NOT NULL | thread, comment |
| target_id | UUID | NOT NULL | ID of voted content |
| value | SMALLINT | NOT NULL | 1 for upvote, -1 for downvote |
| created_at | TIMESTAMP | DEFAULT NOW() | Vote timestamp |
| UNIQUE | | (user_id, target_type, target_id) | One vote per user per content |

#### Tags Table

| Column | Type | Constraints | Description |
| :----- | :--- | :---------- | :---------- |
| id | UUID | PRIMARY KEY | Unique tag identifier |
| name | VARCHAR(30) | UNIQUE, NOT NULL | Tag display name |
| slug | VARCHAR(30) | UNIQUE, NOT NULL | URL-friendly identifier |
| thread_count | INTEGER | DEFAULT 0 | Usage count |

#### Thread_Tags Table

| Column | Type | Constraints | Description |
| :----- | :--- | :---------- | :---------- |
| thread_id | UUID | FOREIGN KEY (threads.id) | Thread reference |
| tag_id | UUID | FOREIGN KEY (tags.id) | Tag reference |
| PRIMARY KEY | | (thread_id, tag_id) | Composite key |

#### Thread_Images Table

| Column | Type | Constraints | Description |
| :----- | :--- | :---------- | :---------- |
| id | UUID | PRIMARY KEY | Unique image identifier |
| thread_id | UUID | FOREIGN KEY (threads.id), NOT NULL | Parent thread reference |
| url | VARCHAR(500) | NOT NULL | Image URL |
| alt_text | VARCHAR(200) | NULL | Image description |
| width | INTEGER | NULL | Original width |
| height | INTEGER | NULL | Original height |
| sort_order | INTEGER | DEFAULT 0 | Display order |

#### Awards Table

| Column | Type | Constraints | Description |
| :----- | :--- | :---------- | :---------- |
| id | UUID | PRIMARY KEY | Unique award identifier |
| name | VARCHAR(50) | NOT NULL | Award name (Gold, Silver, Bronze) |
| icon | VARCHAR(50) | NOT NULL | Icon identifier |
| cost | INTEGER | NOT NULL | Coin cost |
| giver_id | UUID | FOREIGN KEY (users.id), NOT NULL | User who gave award |
| receiver_id | UUID | FOREIGN KEY (users.id), NOT NULL | User who received award |
| thread_id | UUID | FOREIGN KEY (threads.id), NULL | Thread awarded (if thread award) |
| comment_id | UUID | FOREIGN KEY (comments.id), NULL | Comment awarded (if comment award) |
| created_at | TIMESTAMP | DEFAULT NOW() | Award timestamp |

#### Reports Table

| Column | Type | Constraints | Description |
| :----- | :--- | :---------- | :---------- |
| id | UUID | PRIMARY KEY | Unique report identifier |
| reporter_id | UUID | FOREIGN KEY (users.id), NOT NULL | User who reported |
| target_type | ENUM | NOT NULL | thread, comment, user |
| target_id | UUID | NOT NULL | ID of reported content/user |
| reason | VARCHAR(50) | NOT NULL | Report category |
| details | TEXT | NULL | Additional details |
| status | ENUM | DEFAULT 'pending' | pending, reviewed, dismissed, actioned |
| reviewed_by | UUID | FOREIGN KEY (users.id), NULL | Moderator who reviewed |
| created_at | TIMESTAMP | DEFAULT NOW() | Report timestamp |
| reviewed_at | TIMESTAMP | NULL | Review timestamp |

#### Bans Table

| Column | Type | Constraints | Description |
| :----- | :--- | :---------- | :---------- |
| id | UUID | PRIMARY KEY | Unique ban identifier |
| user_id | UUID | FOREIGN KEY (users.id), NOT NULL | Banned user |
| banner_id | UUID | FOREIGN KEY (users.id), NOT NULL | Moderator who issued ban |
| reason | TEXT | NOT NULL | Ban reason |
| is_permanent | BOOLEAN | DEFAULT FALSE | Permanent or temporary |
| expires_at | TIMESTAMP | NULL | Ban expiration (NULL for permanent) |
| created_at | TIMESTAMP | DEFAULT NOW() | Ban timestamp |

#### Mod_Notes Table

| Column | Type | Constraints | Description |
| :----- | :--- | :---------- | :---------- |
| id | UUID | PRIMARY KEY | Unique note identifier |
| user_id | UUID | FOREIGN KEY (users.id), NOT NULL | User the note is about |
| moderator_id | UUID | FOREIGN KEY (users.id), NOT NULL | Moderator who created note |
| content | TEXT | NOT NULL | Note content |
| created_at | TIMESTAMP | DEFAULT NOW() | Note timestamp |

#### User_Sessions Table

| Column | Type | Constraints | Description |
| :----- | :--- | :---------- | :---------- |
| id | UUID | PRIMARY KEY | Unique session identifier |
| user_id | UUID | FOREIGN KEY (users.id), NOT NULL | User reference |
| token_hash | VARCHAR(255) | NOT NULL | Hashed session token |
| user_agent | VARCHAR(500) | NULL | Browser user agent |
| ip_address | VARCHAR(45) | NULL | User IP address |
| expires_at | TIMESTAMP | NOT NULL | Session expiration |
| created_at | TIMESTAMP | DEFAULT NOW() | Login timestamp |

---

## 8. API Design

### 8.1 Authentication Endpoints

#### POST /api/auth/signup

Request:
```json
{
  "email": "user@example.com",
  "username": "newuser123",
  "password": "SecurePass123"
}
```

Response (201):
```json
{
  "success": true,
  "message": "Account created. Please verify your email.",
  "user": {
    "id": "uuid",
    "username": "newuser123",
    "email": "user@example.com"
  }
}
```

#### POST /api/auth/signin

Request:
```json
{
  "identifier": "user@example.com",
  "password": "SecurePass123",
  "rememberMe": true
}
```

Response (200):
```json
{
  "success": true,
  "user": {
    "id": "uuid",
    "username": "newuser123",
    "avatarUrl": "https://...",
    "role": "registered",
    "karma": 150
  }
}
```

Error (401):
```json
{
  "success": false,
  "error": "invalid_credentials",
  "message": "Incorrect email/username or password."
}
```

### 8.2 Thread Endpoints

#### GET /api/threads

Query params: `sort` (hot|new|top|rising), `category` (slug), `page` (number), `limit` (number, max 50)

Response (200):
```json
{
  "threads": [
    {
      "id": "uuid",
      "title": "Thread Title",
      "author": {
        "id": "uuid",
        "username": "author",
        "avatarUrl": "https://...",
        "karma": 500
      },
      "category": {
        "id": "uuid",
        "name": "Technology",
        "slug": "technology",
        "color": "#3B82F6"
      },
      "type": "text",
      "voteScore": 42,
      "commentCount": 15,
      "isPinned": false,
      "isLocked": false,
      "createdAt": "2026-05-10T12:00:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 25,
    "total": 150,
    "totalPages": 6
  }
}
```

#### POST /api/threads

Request:
```json
{
  "title": "My New Thread",
  "content": "Thread body in markdown",
  "categoryId": "uuid",
  "type": "text",
  "tags": ["help", "question"],
  "images": ["url1", "url2"]
}
```

Response (201):
```json
{
  "success": true,
  "thread": {
    "id": "uuid",
    "title": "My New Thread",
    "url": "/thread/uuid"
  }
}
```

#### GET /api/threads/:id

Response (200):
```json
{
  "thread": {
    "id": "uuid",
    "title": "Thread Title",
    "content": "Thread body...",
    "author": { ... },
    "category": { ... },
    "type": "text",
    "images": [...],
    "tags": [...],
    "voteScore": 42,
    "userVote": 1,
    "commentCount": 15,
    "isPinned": false,
    "isLocked": false,
    "isSpoiler": false,
    "isRemoved": false,
    "viewCount": 1250,
    "createdAt": "2026-05-10T12:00:00Z",
    "updatedAt": "2026-05-10T12:00:00Z"
  },
  "comments": [
    {
      "id": "uuid",
      "content": "Comment body...",
      "author": { ... },
      "voteScore": 10,
      "userVote": 0,
      "depth": 0,
      "replies": [...],
      "createdAt": "2026-05-10T12:30:00Z"
    }
  ]
}
```

### 8.3 Comment Endpoints

#### POST /api/comments

Request:
```json
{
  "threadId": "uuid",
  "content": "This is my comment",
  "parentId": null
}
```

Response (201):
```json
{
  "success": true,
  "comment": {
    "id": "uuid",
    "content": "This is my comment",
    "author": { ... },
    "voteScore": 0,
    "depth": 0,
    "createdAt": "2026-05-10T13:00:00Z"
  }
}
```

### 8.4 Vote Endpoints

#### POST /api/votes

Request:
```json
{
  "targetType": "thread",
  "targetId": "uuid",
  "value": 1
}
```

Response (200):
```json
{
  "success": true,
  "newVoteScore": 43,
  "userVote": 1
}
```

#### DELETE /api/votes/:id

Response (200):
```json
{
  "success": true,
  "newVoteScore": 42,
  "userVote": 0
}
```

### 8.5 User Endpoints

#### GET /api/users/:username

Response (200):
```json
{
  "user": {
    "id": "uuid",
    "username": "username",
    "displayName": "Display Name",
    "bio": "User bio text",
    "avatarUrl": "https://...",
    "bannerUrl": "https://...",
    "karma": 1500,
    "role": "registered",
    "memberSince": "2025-01-15T00:00:00Z",
    "badges": [
      { "name": "Verified", "icon": "check-circle", "color": "#10B981" }
    ]
  },
  "stats": {
    "postCount": 45,
    "commentCount": 230,
    "awardCount": 12
  }
}
```

### 8.6 Search Endpoint

#### GET /api/search

Query params: `q` (search query), `type` (all|threads|comments|users), `sort` (relevance|date), `category` (slug), `page`, `limit`

Response (200):
```json
{
  "results": {
    "threads": [...],
    "comments": [...],
    "users": [...]
  },
  "pagination": { ... }
}
```

---

## 9. Error Handling

### 9.1 Error Response Format

```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable message",
    "details": { }
  }
}
```

### 9.2 Error Codes

| Code | HTTP Status | Description | User Message |
| :--- | :---------- | :---------- | :----------- |
| validation_error | 400 | Invalid input data | "Please check your input and try again." |
| unauthorized | 401 | Not authenticated | "Please sign in to continue." |
| forbidden | 403 | Insufficient permissions | "You don't have permission to perform this action." |
| not_found | 404 | Resource not found | "This content could not be found." |
| thread_locked | 403 | Thread is locked | "This thread is locked and cannot receive new comments." |
| user_banned | 403 | User is banned | "Your account has been suspended." |
| rate_limited | 429 | Too many requests | "You're doing that too fast. Please wait a moment." |
| server_error | 500 | Internal error | "Something went wrong. Please try again later." |

### 9.3 Client-Side Error Handling

| Scenario | Behavior |
| :-------- | :------- |
| Network failure | Toast: "Unable to connect. Check your internet connection." Retry button |
| Form validation | Inline error messages below each invalid field |
| Vote failure | Revert optimistic update, show error toast |
| Comment post failure | Keep comment text in textarea, show error toast |
| Session expired | Redirect to sign-in, preserve intended destination |

### 9.4 Empty States

| View | Empty State |
| :---- | :---------- |
| No threads in category | Illustration + "Be the first to post in {category}" + Create Post button |
| No search results | "No results for '{query}'. Try different keywords or browse categories." |
| No comments yet | "No comments yet. Be the first to share your thoughts!" |
| User has no posts | "This user hasn't posted anything yet." |
| No notifications | "You're all caught up! Check back later for updates." |

---

## 10. Real-Time Features

### 10.1 WebSocket Events

| Event | Payload | Description |
| :----- | :------ | :---------- |
| new_comment | { threadId, comment } | New comment posted in thread |
| comment_deleted | { threadId, commentId } | Comment removed |
| vote_update | { targetType, targetId, newScore } | Vote count changed |
| thread_locked | { threadId } | Thread locked by moderator |
| thread_pinned | { threadId } | Thread pinned by moderator |
| mod_action | { threadId, action, moderatorName } | Moderation action taken |

### 10.2 Real-Time Updates Implementation

```
Client subscribes to: /ws/thread/{threadId}
Server sends heartbeat every 30 seconds
Reconnection with exponential backoff (1s, 2s, 4s, 8s, max 30s)
On reconnect: client sends last_event_id for event replay
```

### 10.3 Notifications

| Notification Type | Trigger | Display |
| :---------------- | :------ | :------- |
| Reply to your comment | Someone replies | "User replied to your comment in {thread}" |
| Mention | Someone @mentions you | "User mentioned you in {thread}" |
| Award received | Someone awards your content | "User gave you a {award} award!" |
| Karma milestone | Reach 100, 500, 1000 karma | "Congratulations! You've reached {n} karma!" |
| Mod action on your content | Mod removes/warns | "Your {content} was removed for: {reason}" |

---

## 11. Payments (Optional Feature)

### 11.1 Coin System

| Aspect | Implementation |
| :------ | :------------- |
| Currency | Coins (virtual currency) |
| Initial grant | 100 coins on account creation |
| Earn by | Posting quality content (1 coin per upvote received, max 50/day) |
| Spend on | Awards (Gold: 500 coins, Silver: 100 coins, Bronze: 25 coins) |

### 11.2 Award Prices

| Award Type | Cost | Karma Bonus to Receiver |
| :---------- | :--- | :----------------------- |
| Gold | 500 coins | +100 karma |
| Silver | 100 coins | +25 karma |
| Bronze | 25 coins | +10 karma |

---

## 12. Admin Tools

### 12.1 Admin Dashboard

| Feature | Description |
| :------- | :---------- |
| Analytics | Daily/weekly/monthly active users, post counts, engagement metrics |
| User management | Search users, view profiles, adjust roles, issue bans |
| Category management | Create/edit/delete categories, set icons and colors |
| System health | Server status, error rates, API response times |
| Audit log | All admin actions logged with timestamp and admin ID |

### 12.2 Moderation Queue

| Feature | Description |
| :------- | :---------- |
| Queue items | Reported content awaiting review |
| Filters | By report reason, by category, by reporter |
| Bulk actions | Select multiple items for approve/remove/dismiss |
| History | Previously reviewed items for audit |

---

## 13. Performance Requirements

### 13.1 Metrics

| Metric | Target |
| :------ | :----- |
| Page load time | < 2 seconds on 4G connection |
| First Contentful Paint | < 1.5 seconds |
| Time to Interactive | < 3 seconds |
| API response time | < 200ms for read operations, < 500ms for writes |
| WebSocket message latency | < 100ms |

### 13.2 Techniques

| Technique | Implementation |
| :--------- | :------------- |
| Caching | Redis for session storage, CDN for static assets, database query caching |
| Pagination | Cursor-based pagination for feeds (default 25 items) |
| Lazy loading | Images and comments loaded on scroll |
| Code splitting | Route-based chunk splitting |
| Compression | Gzip for API responses, WebP for images |

---

## 14. Accessibility

### 14.1 Keyboard Navigation

| Key | Action |
| :--- | :------ |
| Tab | Move focus to next interactive element |
| Enter/Space | Activate focused button or link |
| Escape | Close modals and dropdowns |
| Arrow Up/Down | Navigate within dropdown menus |
| J/K | Navigate between threads (vim-style) |

### 14.2 Screen Reader Support

- Semantic HTML elements (article, nav, main, aside)
- ARIA labels on icon-only buttons
- Live regions for dynamic content updates
- Alt text for all images

### 14.3 Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

---

## 15. Responsive Breakpoints

| Breakpoint | Width | Layout Changes |
| :---------- | :---- | :------------- |
| Mobile | < 640px | Single column, hamburger menu, bottom navigation bar |
| Tablet | 640px - 1024px | Sidebar collapsed by default, content area 100% |
| Desktop | > 1024px | Full sidebar visible, content max-width 720px |
| Wide | > 1440px | Centered layout with more whitespace |

---

## 16. Security Considerations

| Aspect | Implementation |
| :------ | :------------- |
| Password storage | Bcrypt with cost factor 12 |
| Session tokens | JWT with 24h expiry, httpOnly cookies |
| CSRF protection | SameSite cookie attribute, CSRF tokens for forms |
| XSS prevention | Content sanitization, CSP headers |
| SQL injection | Parameterized queries via ORM |
| Rate limiting | 100 requests/minute per IP, 1000 requests/minute per user |
| Input validation | Server-side validation for all inputs |