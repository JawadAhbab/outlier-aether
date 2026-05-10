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

- Full viewport height (100vh) with vertically centered content using flexbox (justify-content: center, align-items: center)
- Large typography hero text "WE CREATE DIGITAL EXPERIENCES" in Syne 700, 5rem size, split across 2 lines with line-height 1.0
- Subtitle text below: 1.5rem Inter, max-width 600px, color #737373, line-height 1.6
- Single CTA button "View Our Work" with 16px horizontal padding, 56px height, border-radius 4px, background #8b5cf6
- Button hover state: Background darkens to #7c3aed, translateY -2px, shadow appears (0 10px 30px rgba(139, 92, 246, 0.3))
- Scroll indicator at bottom center: Chevron icon (32px), animated bouncing with 2s interval, opacity 0.6
- Background: Subtle gradient from #faf9f7 to #f0efed (top to bottom), fixed position for parallax-ready

**Selected Work Section**

- Dark background (#1a1a1a) for visual contrast, full-width section with 64px vertical padding
- Section title "SELECTED WORK" positioned top-left, Syne 600, 2.5rem, color #ffffff with 0.9 opacity
- 3 featured project cards in asymmetric grid (2 columns: 60% left large card, 40% right stacked cards)
- First card: 60% width, full height (500px), positioned left with 24px gap from right column
- Second column: Contains 2 stacked cards (250px each), each 40% width, gap 24px between them
- Each card: project thumbnail (aspect 4:3, object-fit: cover), title overlay (bottom-left, white text), category tag (top-right pill badge)
- Card hover: Overlay darkens (opacity 0.6 to 0.8), title slides up 10px, scale 1.02
- Cards trigger page transition on click with shared element animation

**About Section**

- Light background (#faf9f7), two-column layout with max-width 1200px container
- Left column (55% width): Contains section tag "ABOUT" (Inter 500, 0.875rem, #8b5cf6), heading "We are Motion Collective" (Syne 600, 3rem), 3 paragraphs of body text (Inter 400, 1.125rem, line-height 1.7, color #1a1a1a)
- Right column (45% width): Portrait image with rounded corners (24px), subtle shadow (0 20px 60px rgba(0,0,0,0.1)), object-fit: cover
- Stats row below image: 4 items in horizontal layout (Years: 12, Projects: 200+, Clients: 85+, Awards: 24), each with large number (Syne 700, 2.5rem, #0a0a0a) and label below (Inter 400, 0.875rem, #737373)
- Stats animate on scroll entry: numbers count up from 0 over 2000ms with ease-out

**Services Section**

- Dark background section (#1a1a1a), section heading "WHAT WE DO" centered, Syne 600, 2rem, white
- Horizontal scrolling service cards container with scroll-snap-type: x mandatory
- 3 cards visible initially on desktop (100vw - 160px padding), each card 400px width, 500px height
- Card styling: background #262626, border-radius 16px, 32px padding, subtle border (1px solid #404040)
- Card content: Icon (64px, SVG or component), service title (Syne 600, 1.5rem, white), description (Inter 400, 1rem, #a0a0a0, line-height 1.6), "Learn More" link (color #8b5cf6, hover underline)
- Services list: Brand Identity, Web Experiences, Motion Design, Creative Direction
- Scroll arrows: Left/right chevron buttons (48px circles, #262626 background) appear on hover, positioned at container edges

**Contact Section**

- Light background (#faf9f7) with centered content, 96px vertical padding
- Large heading "Let's Create Something Together" (Syne 700, 3.5rem, #0a0a0a), centered, max-width 700px
- Contact form container: max-width 500px, centered, 48px margin top
- Form fields: Name (text input), Email (email input), Project Type (select dropdown), Message (textarea, 4 rows)
- Input styling: Full width, 56px height, transparent background, bottom border (1px solid #e5e5e5), transition to #8b5cf6 on focus
- Floating labels: Start inside input, animate up 20px and scale 0.85 on focus, color #737373 to #8b5cf6 transition
- Submit button: Width 320px on desktop (full width mobile), 56px height, background #8b5cf6, white text (Inter 500), border-radius 4px
- Button hover: background #7c3aed, translateY -2px, shadow appears
- Success state: Form fades out, checkmark animation (circular reveal, 400ms), "Message Sent" text fades in below

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
- Animation: Progress Bar Fill, Trigger: Scroll position, Description: Width percentage = (scrollY / (documentHeight - viewportHeight)) \* 100

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
