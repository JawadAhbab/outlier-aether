# Product Requirements Document: Motion Collective - Creative Agency Portfolio

## Project Overview

- **Project Name:** Motion Collective - Creative Agency Portfolio
- **Type:** Cool Transition
- **Core Functionality:** A portfolio site featuring cinematic page transitions, scroll-triggered sequences, staggered reveal animations, and choreographed micro-interactions. Users experience smooth, theatrical movement between sections with content revealing in calculated timing patterns.
- **Target Users:** Potential clients seeking creative services, design enthusiasts, collaborators looking for agency partnerships.

## Visual Identity

### Color Palette

- Primary Background, Color: Off-White Cream, Hex: #faf9f7, Usage: Main canvas, body background, creates warm paper-like feel
- Secondary Background, Color: Charcoal Black, Hex: #1a1a1a, Usage: Dark sections, contrast blocks, text on light backgrounds
- Accent Primary, Color: Electric Violet, Hex: #8b5cf6, Usage: CTA buttons, active states, progress indicators
- Accent Secondary, Color: Coral Pink, Hex: #fb7185, Usage: Hover states, secondary highlights, decorative elements
- Accent Tertiary, Color: Teal Green, Hex: #14b8a6, Usage: Success states, confirmations, completed indicators
- Text Primary, Color: Deep Black, Hex: #0a0a0a, Usage: Headings, primary body text on light backgrounds
- Text Secondary, Color: Warm Gray, Hex: #737373, Usage: Captions, metadata, tertiary information
- Text on Dark, Color: Pure White, Hex: #ffffff, Usage: Text on dark background sections
- Divider Lines, Color: Light Gray, Hex: #e5e5e5, Usage: Section separators, card borders

### Typography

- Headings H1, Font: Syne, Weight: 700, Size: 5rem, Line Height: 1.0, Usage: Hero sections, main titles (desktop: 80px equivalent)
- Headings H2, Font: Syne, Weight: 600, Size: 3rem, Line Height: 1.1, Usage: Section headers, page titles
- Headings H3, Font: Syne, Weight: 600, Size: 2rem, Line Height: 1.2, Usage: Card titles, subsection headers
- Body Text, Font: Inter, Weight: 400, Size: 1.125rem, Line Height: 1.7, Usage: General content, descriptions, paragraphs
- Small Text, Font: Inter, Weight: 400, Size: 0.875rem, Line Height: 1.5, Usage: Captions, metadata, timestamps
- UI Elements, Font: Inter, Weight: 500, Size: 0.875rem, Line Height: 1.2, Usage: Buttons, navigation items, form labels

### Spacing System

- Base unit: 8px
- Micro spacing: 4px (xxs) - tight gaps, icon padding
- Small spacing: 16px (sm) - between related elements
- Medium spacing: 32px (md) - between sections, card padding
- Large spacing: 64px (lg) - major section gaps
- XL spacing: 96px (xl) - hero spacing, dramatic gaps
- XXL spacing: 128px (xxl) - between major page divisions
- Border radius (small): 4px - buttons, inputs
- Border radius (medium): 12px - cards, modals
- Border radius (large): 24px - featured elements
- Container max-width: 1400px

## Layout & Structure

### Page Structure

**Hero Section**
- Full viewport height (100vh) with vertically centered content
- Large typography hero text "WE CREATE DIGITAL EXPERIENCES" in Syne 700, split across 2 lines
- Subtitle text below: 1.5rem Inter, max-width 600px
- Single CTA button "View Our Work" with 16px padding horizontal, 56px height
- Scroll indicator at bottom center: animated chevron bouncing with 2s interval
- Background: Subtle gradient from #faf9f7 to #f0efed (top to bottom)

**Selected Work Section**
- Dark background (#1a1a1a) for visual contrast
- Section title "SELECTED WORK" positioned top-left, Syne 600, 2.5rem
- 3 featured project cards in asymmetric grid (2 columns: 60% + 40%)
- First card spans full height, second card split into 2 stacked items
- Each card shows: project thumbnail (aspect 4:3), title overlay, category tag
- Cards trigger page transition on click

**About Section**
- Light background (#faf9f7)
- Two-column layout: left side text content, right side portrait image
- Text column: section tag "ABOUT", heading "We are Motion Collective", 3 paragraphs of text
- Image: Rounded corners (24px), slight shadow, 60% container width
- Stats row below image: 4 items (Years, Projects, Clients, Awards) with large numbers

**Services Section**
- Dark background section
- Horizontal scrolling service cards (3 cards visible, drag/scroll to see more)
- Each card: 400px width, 500px height, dark card (#262626)
- Card content: Icon (64px), service title (Syne 600), description (Inter), "Learn More" link
- Services: Brand Identity, Web Experiences, Motion Design, Creative Direction

**Contact Section**
- Light background with centered content
- Large heading "Let's Create Something Together"
- Contact form: Name, Email, Project Type (dropdown), Message (textarea)
- Submit button: Full width on mobile, 320px on desktop, Electric Violet background
- Success state: Checkmark animation, "Message Sent" confirmation

### Navigation

- Fixed header (height: 80px) with logo left, nav links center, CTA button right
- Nav links: Work, About, Services, Contact with underline hover animation
- Header background: transparent on hero, solid #faf9f7 with shadow after scroll (when scrolled > 100px)
- Mobile: Hamburger menu (3 lines), expands to full-screen overlay with staggered link reveals
- Page progress: Thin progress bar (3px height) at top of viewport showing scroll position

## Animation & Motion Specifications

### Page Transitions

- Transition: Home to Work, Duration: 800ms, Easing: cubic-bezier(0.77, 0, 0.175, 1), Description: Current page content slides left while fading, new page slides in from right with slight overlap
- Transition: Home to About, Duration: 600ms, Easing: cubic-bezier(0.4, 0, 0.2, 1), Description: Content fades out (200ms), brief pause (100ms), new content fades in with upward motion
- Transition: Work to Project Detail, Duration: 700ms, Easing: cubic-bezier(0.65, 0, 0.35, 1), Description: Grid collapses inward to reveal detail view, shared element animation on project image
- Transition: Any to Contact, Duration: 500ms, Easing: cubic-bezier(0.25, 0.1, 0.25, 1), Description: Content scales down slightly (0.98), blurs (4px), then contact form fades in
- Transition: Modal Open, Duration: 400ms, Easing: cubic-bezier(0.34, 1.56, 0.64, 1), Description: Scale 0.9 to 1.0, opacity 0 to 1, spring overshoot at end
- Transition: Modal Close, Duration: 250ms, Easing: cubic-bezier(0.4, 0, 1, 1), Description: Scale 1.0 to 0.95, opacity 1 to 0, ease-in curve

### Scroll-Triggered Animations

- Animation: Section Title Reveal, Trigger: 80% viewport entry, Description: Characters animate in sequentially with 20ms stagger per character, y-offset 40px to 0, opacity 0 to 1
- Animation: Image Parallax, Trigger: Scroll position, Description: Images move at 0.6x scroll speed creating depth illusion, translateY calculated per element
- Animation: Card Stagger Reveal, Trigger: 80% viewport entry, Description: Cards animate in with 150ms stagger, translateY 60px to 0, opacity 0 to 1, rotation -2deg to 0deg
- Animation: Stats Counter, Trigger: 50% viewport entry, Description: Numbers count up from 0 to target value over 2000ms, easing: ease-out
- Animation: Progress Bar Fill, Trigger: Scroll position, Description: Width percentage = (scrollY / (documentHeight - viewportHeight)) * 100

### Staggered Reveal Sequences

- Sequence: Hero Text, Duration: 1200ms total, Steps: Line 1 slides up (0-400ms), Line 2 slides up (200-600ms), Subtitle fades in (600-900ms), Button fades in (900-1200ms)
- Sequence: Service Cards, Duration: 1000ms total, Steps: Card 1 enters (0-300ms), Card 2 enters (150-450ms), Card 3 enters (300-600ms), Card 4 enters (450-750ms), Arrows appear (750-1000ms)
- Sequence: About Section, Duration: 800ms total, Steps: Tag line (0-200ms), Heading (100-400ms), Paragraphs stagger (300-700ms), Image (400-800ms)

### Micro-Interactions

- Button Hover, Animation: Background slides in from left (100% to 0% width), Duration: 300ms, Easing: ease-out, Background color: accent primary, Text color: white
- Button Click, Animation: Scale 0.97 on mousedown, Scale 1.02 on mouseup with spring, Duration: 150ms total
- Card Hover, Animation: translateY -8px, box-shadow increase (0 20px 40px rgba(0,0,0,0.1)), Duration: 300ms, Easing: ease-out
- Nav Link Hover, Animation: Underline grows from center (width 0 to 100%), Duration: 200ms, Easing: ease-out
- Input Focus, Animation: Border color transition to accent (#8b5cf6), label scales 0.85 and moves up 20px, Duration: 250ms
- Image Hover (Work cards), Animation: Scale 1.05, overlay fade in (0 to 0.8 opacity), Duration: 400ms

### Loading States

- Page Load: Logo scales from 0.8 to 1.0, opacity 0 to 1, Duration: 600ms, then content reveals
- Image Load: Skeleton placeholder (#e5e5e5) fades out as image loads, crossfade effect
- Button Loading: Text changes to "Loading...", spinner icon appears left of text, button disabled

## Technical Requirements

### Animation Libraries

- GSAP (GreenSock Animation Platform): Primary animation library, handles page transitions, scroll triggers, timelines
- GSAP ScrollTrigger: Scroll-based animations, parallax effects, pinned sections
- GSAP SplitText (optional plugin): Character/word/line splitting for text reveals
- CSS Transitions: Simple hover states, button backgrounds, focus states (for performance)

### Framework

- Next.js 14 with App Router for page transitions
- React 18 with Framer Motion for component animations
- Tailwind CSS for styling with custom animation utilities
- Intersection Observer API for scroll-triggered reveals (with GSAP fallback)

### Page Routing

- App Router with layout.tsx managing persistent elements (header, footer)
- Page transitions handled via Framer Motion AnimatePresence in template files
- Route segments: / (Home), /work (Selected Work), /about (About), /contact (Contact)
- Project detail: /work/[slug] dynamic route with shared element transitions

### Performance Optimizations

- will-change: transform on animated elements (applied via GSAP)
- transform: translateZ(0) for GPU acceleration on carousel items
- requestAnimationFrame for scroll-linked animations
- Debounced scroll handlers (16ms threshold for 60fps)
- Intersection Observer for lazy animation triggering (only animate when visible)

## Application Logic

### Data Model

**Project:**

| Field | Type | Description |
|-------|------|-------------|
| id | string | Unique project identifier (slug format) |
| title | string | Project display name |
| category | string | Project category (brand, web, motion, etc.) |
| thumbnail | string | Image URL for grid display |
| year | number | Completion year |
| description | string | Brief project summary |
| client | string | Client name |
| services | Array[string] | List of services provided |
| images | Array[string] | Gallery image URLs |
| featured | boolean | Whether shown on homepage |

**Service:**

| Field | Type | Description |
|-------|------|-------------|
| id | string | Unique service identifier |
| title | string | Service name |
| description | string | Service description |
| icon | string | Icon component name |
| features | Array[string] | Key service features |

**ContactSubmission:**

| Field | Type | Description |
|-------|------|-------------|
| id | string | UUID for tracking |
| name | string | Sender name |
| email | string | Sender email |
| projectType | string | Selected project type |
| message | string | Message content |
| submittedAt | Date | Timestamp of submission |

### Core Feature Flows

**Flow 1: Page Navigation**

1. User clicks nav link or CTA button
2. Current page content begins exit animation (slide/fade based on route)
3. Scroll position resets to top (0)
4. URL updates via Next.js router
5. New page content enters with reveal animation
6. Header state updates (active link highlighting)

**Flow 2: Project Detail View**

1. User clicks project card in work section
2. Card image expands via shared element transition (shared element animation)
3. Other page elements fade/slide out simultaneously (300ms)
4. Project detail page content loads in background
5. Detail content fades in (staggered: hero image, title, description, gallery)
6. URL updates to /work/[project-slug]

**Flow 3: Contact Form Submission**

1. User fills form fields with floating labels
2. Real-time validation shows error/success states
3. Submit button enabled when all fields valid
4. On submit: button shows loading state (spinner)
5. Form data POSTed to API route
6. Success: form replaced with checkmark animation and confirmation message
7. Error: toast notification appears with retry option

### State Management

- URL state via Next.js router (current page, project slug)
- Scroll state: useScrollPosition hook for progress bar, parallax calculations
- Animation state: GSAP timeline references for play/pause control
- Form state: React useState for controlled inputs, validation status

### Error Handling

- Scenario: Page transition fails, Handling: Fallback to instant page change without animation, log error for debugging
- Scenario: Image load timeout, Handling: Show skeleton for 10s, then display placeholder with retry button
- Scenario: Form submission error, Handling: Display toast "Something went wrong. Please try again.", re-enable form
- Scenario: Scroll animation jank, Handling: Detect via fps monitoring, disable non-essential animations on low-end devices

## Responsive Behavior

### Breakpoints

- Name: Mobile, Width: < 640px, Changes: Single column layout, stacked navigation (hamburger menu), reduced typography scale (0.7x), swipe gestures on carousels
- Name: Tablet, Width: 640px - 1024px, Changes: 2-column grid for work section, side navigation collapses, typography 0.85x
- Name: Desktop, Width: 1024px - 1440px, Changes: Full layout, all animations enabled, full typography
- Name: Large, Width: > 1440px, Changes: Max container 1400px, increased spacing, larger type scale (1.1x)

### Mobile Adaptations

- Page transitions: Simplified to fade only (no slide) for performance
- Scroll animations: Reduced to essential only (text reveals, card entrances)
- Parallax: Disabled on mobile (causes performance issues)
- Service carousel: Touch swipe enabled, arrows hidden
- Hero typography: 3.5rem instead of 5rem
- Card grid: 1 column instead of 2-column asymmetric

### Animation Scaling

- Element: Page transitions, Desktop: Full slide/fade combo, Mobile: Fade only
- Element: Scroll reveals, Desktop: All elements animate, Mobile: Text and cards only
- Element: Parallax, Desktop: Enabled (0.6x speed), Mobile: Disabled
- Element: Micro-interactions, Desktop: All hover states, Mobile: Touch feedback only (scale 0.98)
- Element: Loading animations, Desktop: Logo animation, Mobile: Simple spinner only

## Performance

### Performance Metrics

- First Contentful Paint: < 1.2s
- Largest Contentful Paint: < 2.5s
- Cumulative Layout Shift: < 0.1
- Time to Interactive: < 3.0s
- Target FPS: 60fps for all animations

### Performance Techniques

- **Code Splitting:** Dynamic imports for page components, GSAP loaded only when animations needed
- **Image Optimization:** WebP format, srcset for responsive images, lazy loading with blur placeholder
- **Animation Throttling:** requestAnimationFrame for scroll-linked, will-change for transform-only
- **Debouncing:** Scroll handlers debounced at 16ms, resize handlers at 100ms
- **CSS Containment:** contain: layout style on animated containers to limit reflows

## Accessibility

**Keyboard Navigation:**

- Tab: Navigate through interactive elements in DOM order
- Enter/Space: Activate buttons, toggle mobile menu
- Escape: Close modals, close mobile menu
- Arrow keys: Navigate within carousels (left/right), form fields (up/down)

**Screen Reader Support:**

- role="main" on main content area, role="navigation" on header nav
- aria-label on icon-only buttons, aria-current="page" on active nav link
- Announce page transitions via aria-live region (polite mode)
- Form error messages linked via aria-describedby

**Reduced Motion (prefers-reduced-motion):**

- Change: Page transitions, Standard: Slide/fade animations, Reduced: Instant (0ms)
- Change: Scroll reveals, Standard: Staggered fade-up animations, Reduced: No animation (elements visible immediately)
- Change: Parallax, Standard: Enabled, Reduced: Disabled (static positioning)
- Change: Button hover, Standard: Background slide animation, Reduced: Color change only (no movement)
- Change: Modal, Standard: Scale + fade animation, Reduced: Opacity fade only

**Color Contrast:**

- All text meets WCAG AA standard (4.5:1 for body, 3:1 for large text)
- Interactive elements have visible focus states (2px outline offset 2px)
- Error states use both color (coral pink) AND icon (warning triangle) for accessibility

## Browser Support

- Chrome 90+
- Firefox 88+
- Safari 15+
- Edge 90+
- Mobile Safari iOS 15+
- Chrome Android 90+

## Dependencies

```json
{
  "next": "^14.0.0",
  "react": "^18.2.0",
  "framer-motion": "^10.16.0",
  "gsap": "^3.12.0",
  "tailwindcss": "^3.3.0"
}
```

---

*Document Version: 1.0*
*Last Updated: 2026-05-11*
*Author: WebDev PRD Generator*