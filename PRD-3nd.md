# Product Requirements Document: Lumina - Editorial Photography Portfolio

## Project Overview

- **Project Name:** Lumina - Editorial Photography Portfolio
- **Type:** Cool Transition
- **Core Functionality:** An editorial photography portfolio featuring cinematic horizontal scrolling, image-heavy layouts with scroll-triggered parallax reveals, dramatic text animations, and smooth page-to-page transitions. The experience emphasizes visual storytelling with content appearing in choreographed sequences.
- **Target Users:** Potential clients seeking photography services, art directors, creative agencies, photography enthusiasts.

## Visual Identity

### Color Palette

- Primary Background, Color: Pure White, Hex: #ffffff, Usage: Main canvas, clean editorial feel
- Secondary Background, Color: Ink Black, Hex: #0a0a0a, Usage: Dark sections, full-bleed image overlays, contrast text
- Accent Primary, Color: Burnt Orange, Hex: #e85d04, Usage: Interactive elements, hover states, progress indicators
- Accent Secondary, Color: Deep Burgundy, Hex: #7b2cbf, Usage: Secondary highlights, category tags
- Text Primary, Color: Ink Black, Hex: #0a0a0a, Usage: Headings, body text on light backgrounds
- Text on Image, Color: Pure White, Hex: #ffffff, Usage: Text overlaid on photographs
- Muted Text, Color: Warm Gray, Hex: #6b7280, Usage: Captions, metadata, dates
- Border Color, Color: Light Gray, Hex: #e5e7eb, Usage: Section dividers, card borders

### Typography

- Headings H1, Font: Playfair Display, Weight: 700, Size: 4rem, Line Height: 1.1, Usage: Hero text, full-screen titles
- Headings H2, Font: Playfair Display, Weight: 600, Size: 2.5rem, Line Height: 1.2, Usage: Section titles, project names
- Headings H3, Font: Playfair Display, Weight: 500, Size: 1.5rem, Line Height: 1.3, Usage: Subheadings, captions
- Body Text, Font: Source Sans 3, Weight: 400, Size: 1.125rem, Line Height: 1.7, Usage: Descriptions, article content
- UI Text, Font: Source Sans 3, Weight: 500, Size: 0.875rem, Line Height: 1.4, Usage: Buttons, navigation, labels
- Caption Text, Font: Source Sans 3, Weight: 300, Size: 0.75rem, Line Height: 1.5, Usage: Photo credits, timestamps

### Spacing System

- Base unit: 8px
- Tight spacing: 8px (xs) - between related elements, icon gaps
- Small spacing: 16px (sm) - card padding, list items
- Medium spacing: 32px (md) - section gaps, component margins
- Large spacing: 64px (lg) - major section breaks
- XL spacing: 96px (xl) - hero spacing, full-viewport sections
- XXL spacing: 160px (xxl) - between major page divisions
- Container max-width: 1440px
- Content max-width: 1200px

## Layout & Structure

### Page Structure

**Homepage - Hero**

- Full-viewport hero (100vh) with featured image as background (object-fit: cover, 100% width/height)
- Image slightly zoomed (scale 1.05) for parallax depth effect
- Overlay gradient: linear-gradient(180deg, rgba(0,0,0,0.4) 0%, transparent 50%, rgba(0,0,0,0.6) 100%)
- Centered text block: Photographer name "LUMINA" in Playfair Display 700, 6rem, white, letter-spacing 0.3em
- Tagline below: "Editorial & Documentary Photography", Source Sans 3 400, 1.25rem, white at 0.9 opacity
- Scroll indicator: Minimal line animation (height 0 to 60px, 1.5s ease-in-out, infinite)
- Entry animation: Image fades in (0 to 1, 1000ms), text slides up (translateY 30px to 0, 800ms, 200ms delay)

**Gallery - Horizontal Scroll**

- Section height: 100vh with sticky positioning
- Horizontal scroll container with snap-type: x mandatory
- Scroll driven by vertical mouse wheel or trackpad scroll (translate vertical scroll to horizontal movement)
- 4 featured series displayed horizontally, each series card 80vw width, 70vh height
- Card structure: Full-bleed image (aspect 3:2), title overlay at bottom, category pill top-left
- Progress indicator: Horizontal bar at bottom showing scroll position (width percentage)
- Between cards: 64px gap with small dots navigation below

**Individual Series - Vertical Scroll**

- Full-width hero image (100vw, 80vh) with title and date overlaid
- Title: Series name (Playfair Display 700, 3rem), white text with text-shadow
- Image parallax: Moves at 0.7x scroll speed creating depth
- Content sections below: 2-column layout (60% image, 40% text on desktop)
- Image reveal: Fade up with 20px translate as enters viewport
- Text content: Description paragraph, date, location, equipment used
- Gallery grid: Masonry-style image grid (3 columns), images lazy-load with blur placeholder

**About Section**

- Split layout: Left side (50%) contains portrait image, right side (50%) contains text
- Image: Full-height, object-fit: cover, with subtle parallax on scroll (0.8x speed)
- Text block: Centered vertically, padding 64px
- Content: Section label "ABOUT", heading "The Story Behind the Lens", 3 paragraphs bio
- Stats row: 3 numbers (Projects: 150+, Years Active: 12, Exhibitions: 24) with labels
- Stats animate on scroll: Count up from 0, 1500ms duration, staggered 200ms

**Contact Section**

- Minimal design, centered content
- Large heading "Let's Work Together" (Playfair Display 700, 3.5rem)
- Contact info: Email (large, clickable), phone number, location
- Social links: Instagram, Behance, LinkedIn with hover animations (scale 1.1)
- No contact form - direct email approach

### Navigation

- Fixed header (height: 72px), transparent on hero, white with shadow after scroll (> 100px)
- Logo (text "LUMINA") left side, Playfair Display 700, letter-spacing 0.2em
- Nav links center: Work, Series, About, Contact
- Link hover: Orange underline grows from center (width 0 to 100%, 200ms ease-out)
- Mobile: Hamburger menu (3 lines), full-screen overlay with staggered text reveals
- Progress bar: Thin orange line (3px) at top of viewport showing horizontal scroll progress on gallery page

## Animation & Motion Specifications

### Page Transitions

- Transition: Home to Gallery, Duration: 1000ms, Easing: cubic-bezier(0.76, 0, 0.24, 1), Description: Hero image scales down (1.0 to 0.9) while fading, gallery container fades in from black
- Transition: Gallery to Series, Duration: 800ms, Easing: cubic-bezier(0.4, 0, 0.2, 1), Description: Current card slides left while fading, new page slides in from right with overlap
- Transition: Series to Series, Duration: 600ms, Easing: cubic-bezier(0.65, 0, 0.35, 1), Description: Crossfade with slight zoom (1.02 to 1.0)
- Transition: Any to About/Contact, Duration: 500ms, Easing: cubic-bezier(0.25, 0.1, 0.25, 1), Description: Content fades out (200ms), new content fades in with upward motion (300ms)
- Transition: Back to Home, Duration: 700ms, Easing: cubic-bezier(0.4, 0, 1, 1), Description: Reverse of entry animation, content scales down and fades

### Scroll-Triggered Animations

- Animation: Image Reveal, Trigger: 70% viewport entry, Description: translateY 30px to 0, opacity 0 to 1, duration 600ms, easing: ease-out
- Animation: Parallax Image, Trigger: Scroll position, Description: translateY calculation: scrollY * 0.3 for subtle depth, capped at 200px max movement
- Animation: Text Reveal, Trigger: 80% viewport entry, Description: Characters split and animate individually, 15ms stagger per character, translateY 40px to 0
- Animation: Stats Counter, Trigger: 50% viewport entry, Description: Number counts from 0 to target, 1500ms, easing: ease-out
- Animation: Progress Bar, Trigger: Scroll position, Description: Width = (scrollX / maxScroll) * 100, smooth updates via requestAnimationFrame

### Horizontal Scroll Mechanics

- Translation: Vertical scroll delta converted to horizontal movement via wheel event handler
- Acceleration: Momentum-based scrolling with ease-out deceleration
- Snap: snap-type: mandatory, snap-points: start of each card
- Keyboard: Left/Right arrow keys move between cards when gallery focused
- Touch: Swipe left/right on mobile, velocity-based momentum

### Staggered Reveal Sequences

- Sequence: Homepage Entry, Duration: 1500ms total, Steps: Image fade (0-500ms), Logo text letter-by-letter (300-900ms), Tagline (800-1200ms), Scroll indicator (1200-1500ms)
- Sequence: Gallery Cards, Duration: 800ms total, Steps: Card 1 title (0-200ms), Card 1 image (100-400ms), Card 2 title (200-400ms), Card 2 image (300-600ms), Progress dots (600-800ms)
- Sequence: Series Page, Duration: 1200ms total, Steps: Hero image scale animation (0-600ms), Title characters stagger (400-1000ms), Meta info fade (800-1200ms)

### Micro-Interactions

- Image Hover (Gallery cards): Scale 1.03, overlay opacity 0.4 to 0.6, Duration: 400ms
- Nav Link Hover: Underline width 0 to 100%, color #e85d04, Duration: 200ms ease-out
- Button Hover: Background darkens (#d45203), translateY -2px, Duration: 200ms
- Social Icon Hover: Scale 1.15, color transition to orange, Duration: 200ms
- Image Click: Ripple effect from click point, Duration: 400ms
- Scroll Indicator: Line height animates 0 to 60px, 1.5s ease-in-out, infinite alternate

### Loading States

- Page Load: Simple fade-in (opacity 0 to 1, 400ms)
- Image Load: Blur placeholder (20px blur, grayscale 50%), crossfade to loaded image over 300ms
- Gallery Load: Cards stagger in with 150ms delay between each

## Technical Requirements

### Animation Libraries

- GSAP (GreenSock Animation Platform): Core animations, page transitions, scroll triggers
- GSAP ScrollTrigger: Horizontal scroll, parallax effects, snap animations
- GSAP SplitText: Character-by-character text animations
- Native CSS Transitions: Simple hover states (for performance)

### Framework

- Next.js 14 with App Router
- React 18 with Framer Motion for component animations
- Tailwind CSS for styling
- Intersection Observer for lazy-loading images

### Page Routing

- App Router structure: / (Home), /work (Gallery), /work/[series] (Individual series), /about, /contact
- Horizontal scroll handled within /work page via GSAP ScrollTrigger
- Shared element transitions between gallery cards and series pages

### Performance Optimizations

- will-change: transform on horizontal scroll container
- GPU acceleration: transform: translateZ(0) on animated images
- Debounced wheel handler (16ms threshold)
- requestAnimationFrame for scroll-linked parallax calculations
- Image lazy loading with blur placeholder via blurhash

## Data Model

**Photo Series:**

| Field | Type | Description |
|-------|------|-------------|
| id | string | Unique series identifier (slug format) |
| title | string | Series display name |
| category | string | Category (editorial, documentary, commercial) |
| year | number | Year of creation |
| location | string | Primary location |
| coverImage | string | Image URL for gallery display |
| images | Array[string] | Array of image URLs |
| description | string | Series description paragraph |
| equipment | Array[string] | Camera/equipment used |

**Photo:**

| Field | Type | Description |
|-------|------|-------------|
| id | string | Unique photo identifier |
| seriesId | string | Reference to parent series |
| src | string | Image URL |
| width | number | Original image width |
| height | number | Original image height |
| caption | string | Photo caption text |
| featured | boolean | Show in highlights |

**Site Settings:**

| Field | Type | Description |
|-------|------|-------------|
| photographerName | string | Display name |
| tagline | string | Hero tagline |
| email | string | Contact email |
| socialLinks | Object | Instagram, Behance, LinkedIn URLs |

## Core Feature Flows

**Flow 1: Gallery Exploration**

1. User lands on homepage, hero image loads with parallax
2. User scrolls or clicks "View Work" CTA
3. Page transitions to /work gallery with horizontal scroll
4. User scrolls vertically, scroll converts to horizontal movement through series cards
5. Progress bar updates in real-time
6. User clicks a series card
7. Shared element animation: card image expands to fill hero
8. Series detail page loads with vertical scroll

**Flow 2: Series Navigation**

1. User views series detail page
2. Hero image with parallax effect
3. Scroll down reveals text content with stagger animation
4. Gallery grid loads progressively as user scrolls
5. Clicking any image opens lightbox overlay
6. Lightbox: Full-screen image, prev/next arrows, close button, caption
7. Keyboard navigation: Escape closes, arrows navigate

**Flow 3: Page Navigation**

1. User clicks navigation link
2. Current page content fades out (200ms)
3. Scroll position resets
4. New page content fades in with staggered animations
5. Header updates active state

## Responsive Behavior

### Breakpoints

- Name: Mobile, Width: < 640px, Changes: Horizontal scroll becomes vertical swipe carousel (1 card visible), typography 0.6x scale, hamburger menu
- Name: Tablet, Width: 640px - 1024px, Changes: Horizontal scroll with 2 card preview, typography 0.8x scale, condensed navigation
- Name: Desktop, Width: 1024px - 1440px, Changes: Full horizontal scroll experience, all animations enabled, full typography
- Name: Large, Width: > 1440px, Changes: Container max 1440px, increased spacing, larger type scale (1.1x)

### Mobile Adaptations

- Horizontal scroll: Converted to native horizontal swipe carousel (1 item at a time)
- Parallax: Reduced to 0.2x on mobile (performance optimization)
- Page transitions: Simplified to fade only (no slide)
- Gallery cards: Full-width, reduced height (60vh instead of 70vh)
- Text reveals: Reduced stagger, only essential animations
- Lightbox: Swipe gestures for prev/next

### Animation Scaling

- Element: Page transitions, Desktop: Full slide/fade combo, Mobile: Fade only
- Element: Parallax, Desktop: 0.3x multiplier, Mobile: 0.2x (simplified)
- Element: Horizontal scroll, Desktop: Full GSAP ScrollTrigger, Mobile: Native swipe
- Element: Text animations, Desktop: Character stagger, Mobile: Word stagger only
- Element: Micro-interactions, Desktop: Full hover effects, Mobile: Touch feedback only

## Performance

### Performance Metrics

- First Contentful Paint: < 1.0s
- Largest Contentful Paint: < 2.0s
- Cumulative Layout Shift: < 0.05
- Time to Interactive: < 2.5s
- Target FPS: 60fps for all animations

### Performance Techniques

- **Code Splitting:** Dynamic imports for series pages, GSAP loaded only on gallery
- **Image Optimization:** WebP format, srcset for responsive sizes, lazy loading with blur placeholder
- **Scroll Throttling:** requestAnimationFrame for scroll handlers, 16ms debounce
- **GPU Acceleration:** transform: translateZ(0) on animated elements, will-change: transform
- **Intersection Observer:** Lazy load images and trigger animations only when visible

## Accessibility

**Keyboard Navigation:**

- Tab: Navigate interactive elements in DOM order
- Enter/Space: Activate buttons, toggle mobile menu
- Escape: Close lightbox, close mobile menu
- Arrow Left/Right: Navigate gallery images when focused
- Arrow Up/Down: Navigate through nav links

**Screen Reader Support:**

- role="main" on main content, role="navigation" on header
- alt text for all images describing content
- aria-label on icon-only buttons
- aria-current="page" on active nav link
- Hide decorative images from screen readers (aria-hidden="true")

**Reduced Motion (prefers-reduced-motion):**

- Change: Page transitions, Standard: 800ms slide animations, Reduced: Instant (0ms)
- Change: Parallax, Standard: 0.3x scroll multiplier, Reduced: Disabled
- Change: Horizontal scroll, Standard: Smooth scroll with snap, Reduced: Native swipe
- Change: Text reveals, Standard: Character stagger animation, Reduced: No animation (visible immediately)
- Change: Image hover, Standard: Scale 1.03, Reduced: No scale change

**Color Contrast:**

- All text on white background: 4.5:1 minimum ratio
- Text on images: Shadow overlay ensures readability
- Interactive elements: Visible focus states with orange outline

## Browser Support

- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+
- Mobile Safari iOS 14+
- Chrome Android 90+

---

*Document Version: 1.0*
*Last Updated: 2026-05-11*
*Author: WebDev PRD Generator*