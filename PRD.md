# PRD: Cool Transition

## 1. Concept & Vision

A cinematic portfolio experience for a creative agency that celebrates digital craftsmanship. The site moves like a film: deliberate, choreographed, and purposeful. Every page transition is a scene change, every scroll reveals a new frame. The overall feeling should be premium, unhurried, and visually dramatic — like walking through a museum where the exhibits come alive as you approach them.

Reference sites: Shopify Editions (scroll-triggered reveals), Dave Holloway (cinematic transitions), Shader (creative coding excellence), Artefakt (editorial motion design).

---

## 2. Visual Identity

### Color Palette

- Obsidian: #0D0D0D — Primary background
- Ivory: #FAFAF8 — Primary text
- Ember: #E85D04 — Accent / CTA hover state
- Slate: #3D3D3D — Secondary surfaces, borders
- Mist: #A3A3A3 — Tertiary text, disabled states

### Typography

- Display: Space Grotesk, 700, 80px / 48px (mobile), line height 0.95
- H1: Space Grotesk, 600, 56px / 32px (mobile), line height 1.1
- H2: Space Grotesk, 600, 40px / 24px (mobile), line height 1.15
- H3: Space Grotesk, 500, 24px / 20px (mobile), line height 1.3
- Body: Instrument Sans, 400, 18px / 16px (mobile), line height 1.6
- Caption: Instrument Sans, 400, 14px / 12px (mobile), line height 1.4
- Mono: JetBrains Mono, 400, 14px, line height 1.5

### Spacing System

Base unit: 8px

- xs: 8px — Tight element gaps
- sm: 16px — Component internal spacing
- md: 24px — Section element spacing
- lg: 48px — Section padding
- xl: 80px — Page section gaps
- 2xl: 120px — Major section dividers

### Border Radius

- sm: 4px — Badges, small tags
- md: 8px — Buttons, inputs
- lg: 16px — Cards, containers
- full: 9999px — Pills, avatars

---

## 3. Pages & Interactions

### Page Structure

The site consists of 4 main pages:

1. **Home** — Hero introduction with animated text, featured work grid
2. **Work** — Case study listing with hover previews
3. **About** — Team presentation, agency philosophy
4. **Contact** — Minimal contact form with location info

### Home Page

#### Hero Section
- Full viewport height (100vh)
- Large display text "We craft digital experiences" split into two lines
- Text reveals character-by-character on load (staggered, 30ms per character)
- Background: subtle noise texture overlay at 3% opacity
- Scroll indicator: animated arrow pointing down (bouncing, 2s infinite)

#### Featured Work Grid
- 2-column grid on desktop, 1-column on mobile
- Cards with 16px gap
- Each card: project thumbnail (16:10 aspect ratio), title, category tag
- Hover: image scales to 1.05, overlay fades in with project description
- Cards enter viewport: fade up with 100px translate, 600ms ease-out, 100ms stagger

### Work Page

#### Project Listing
- Full-width cards stacked vertically with 32px gap
- Each card: full-width image (4:1 aspect ratio), project title, year, brief description
- Horizontal rule between cards: 1px Slate color, animates width from 0% to 100% on scroll-into-view

#### Hover Interaction
- Image parallax: moves at 0.5x scroll rate (slower than page scroll)
- Title shifts right 8px with color change to Ember
- View project CTA appears with underline animation (width 0 to 100%, 300ms)

### About Page

#### Agency Philosophy Section
- Split layout: left side text content, right side floating image stack
- Three principles displayed as staggered list items
- Each principle: icon (SVG, 48px), title, description paragraph
- Scroll-triggered: each item slides in from left, 40px translate, 500ms ease-out

#### Team Grid
- 4-column grid (3 on tablet, 2 on mobile, 1 on small mobile)
- Team member cards: circular avatar (120px), name, role
- Cards animate in with scale (0.8 to 1) and opacity on scroll

### Contact Page

#### Form Section
- Centered layout, max-width 560px
- Fields: Name, Email, Subject (dropdown), Message (textarea)
- Labels positioned as floating labels (animate up on focus)
- Submit button: "Send Message" with ArrowRight icon

#### Form States
- Default: Ivory border, transparent background
- Focus: Ember border, subtle glow (box-shadow: 0 0 0 4px rgba(232, 93, 4, 0.1))
- Error: border color changes to #DC2626, error message appears below field
- Success: entire form fades out, success message fades in

---

## 4. Animation & Motion

### Global Transition System

All page transitions use a shared transition framework:
- Exit animation: 400ms, current page fades to black
- Middle black hold: 200ms (allows preparation for enter)
- Enter animation: 400ms, new page fades from black

Timing function for all transitions: cubic-bezier(0.76, 0, 0.24, 1)

### Page-to-Page Transitions

- Fade to Black: 400ms, cubic-bezier(0.76, 0, 0.24, 1) — page opacity 1 to 0, black overlay opacity 0 to 1
- Black Hold: 200ms, linear — brief black screen
- Fade from Black: 400ms, cubic-bezier(0.76, 0, 0.24, 1) — black overlay opacity 1 to 0, new page opacity 0 to 1

### Scroll-Triggered Animations

- Hero Text: character reveal, 30ms per char, 200ms initial delay, ease-out
- Section Headings: fade + translateY(40px), 600ms, ease-out
- Project Cards: fade + translateY(60px) + scale(0.95), 500ms, stagger 100ms, ease-out
- Team Cards: scale(0.8) to scale(1) + opacity, 400ms, stagger 75ms, ease-out
- Horizontal Rules: width 0% to 100%, 800ms, ease-in-out
- Floating Images: translateY oscillation, 3000ms, ease-in-out infinite

### Staggered Reveal Sequences

For list items (e.g., team grid, principles list):
1. Calculate total items and viewport position
2. Each item: translateX(-40px) + opacity 0 to translateX(0) + opacity 1
3. Stagger delay: index * 100ms
4. Total sequence should not exceed 800ms

### Micro-Interactions

#### Button States

- Default: transparent background, Ivory text
- Hover: Ember background, #0D0D0D text, scale(1.02), box-shadow 0 4px 20px rgba(232, 93, 4, 0.3)
- Active: #C44D03 background, #0D0D0D text, scale(0.98)
- Disabled: Slate background, Mist text
- Loading: Ember background, transparent text, pulse animation

#### Card Hover

- Image: scale(1.05), 400ms ease-out
- Overlay: opacity 0 to 0.8, 200ms ease-in
- Title: translateX(0) to translateX(8px), 200ms
- Description: opacity 0 to 1, translateY(10px) to translateY(0), 300ms, delay 100ms

#### Navigation Link Hover

- Underline: width 0% to 100%, 200ms ease-out
- Text color: Ivory to Ember, 150ms

#### Form Input Focus

- Border color: Ivory to Ember, 200ms
- Label: translateY(0) to translateY(-24px) + scale(0.85), 200ms
- Glow: box-shadow appears, 200ms

---

## 5. Technical Requirements

### Framework & Libraries

- Next.js 14.2 — Framework with App Router
- React 18.3 — UI components
- GSAP 3.12 — Animation engine (ScrollTrigger, timeline)
- Lenis 1.1 — Smooth scrolling
- Three.js 0.162 — 3D background effects
- @react-three/fiber 8.16 — React Three.js integration
- @react-three/drei 9.105 — R3F helpers

### Three.js Implementation

#### Background Particles

- Particle count: 5000
- Particle size: 1px
- Particle color: Ivory at 20% opacity
- Movement: slow drift using Perlin noise
- On scroll: particles flow upward at 0.3x scroll rate
- On mouse move: subtle parallax effect (max 20px offset)

#### Transition Overlay

- Full-screen quad geometry
- Custom shader for smooth fade effect
- Uniforms: uProgress (0 to 1), uColor (#0D0D0D)

### SVG Requirements

#### Animated Icons

All icons use inline SVG with CSS animations:
- Stroke animation: stroke-dashoffset from length to 0
- Fill animation: opacity 0 to 1
- Duration: 400ms
- Easing: ease-out

#### Decorative Elements

- Abstract curved lines in hero section
- Animated using stroke-dasharray and stroke-dashoffset
- Path length calculated programmatically

### Asset Specifications

- Hero Images: WebP format, max 400KB per image
- Thumbnails: WebP format, max 100KB per image
- Team Avatars: WebP format, 2x, max 50KB per image
- Background Textures: WebP format, max 200KB, tiling
- Fonts: WOFF2 format, subset to used characters

---

## 6. Application Logic

### Navigation System

#### Data Model

```
NavigationState {
  currentPath: string
  previousPath: string
  transitionPhase: 'idle' | 'exiting' | 'holding' | 'entering'
  scrollPosition: number
}
```

#### Transition Flow

1. User clicks nav link
2. intercept link click, prevent default
3. setNavigationState: transitionPhase = 'exiting'
4. trigger exit animation (400ms)
5. onAnimationComplete: setNavigationState transitionPhase = 'holding'
6. wait 200ms
7. setNavigationState transitionPhase = 'entering'
8. update router.push(newPath)
9. trigger enter animation (400ms)
10. onAnimationComplete: setNavigationState transitionPhase = 'idle'

### Scroll Detection

```
ScrollContext {
  scrollY: number
  scrollVelocity: number
  direction: 'up' | 'down' | 'idle'
  progress: number (0-1 per section)
}
```

- Use Lenis for smooth scroll with momentum
- Calculate velocity by comparing last 2 scroll positions
- Direction determined by velocity sign
- Throttle scroll handler to 60fps

### Project Data

```
Project {
  id: string
  slug: string
  title: string
  category: string
  year: number
  thumbnail: Image
  images: Image[]
  description: string
  challenge: string
  solution: string
  technologies: string[]
  liveUrl: string
  featured: boolean
}
```

### Contact Form

```
ContactSubmission {
  id: string
  name: string
  email: string
  subject: string
  message: string
  timestamp: Date
  status: 'pending' | 'reviewed' | 'responded'
}
```

#### Validation Rules

- name: required, min 2 chars, max 100 chars
- email: required, valid email format
- subject: required, one of predefined options
- message: required, min 10 chars, max 2000 chars

---

## 7. User Roles & Auth Flows

### User Roles

- Visitor: View all public pages, submit contact form
- Admin: Access dashboard, manage projects, view submissions

### Auth Flow (Admin)

1. Admin navigates to /admin
2. Redirected to login if not authenticated
3. Enters credentials (email + password)
4. Server validates, returns session token
5. Token stored in httpOnly cookie
6. Redirect to /admin/dashboard
7. Session expires after 7 days of inactivity

### Data Flow for Visitors

1. Visitor arrives at site
2. Smooth scroll initialized, particles start animation
3. ScrollTrigger monitors viewport intersections
4. Animations triggered based on scroll position
5. Page transitions managed by NavigationContext

---

## 8. Responsive Behavior

### Breakpoints

- mobile: 0px to 639px — Small phones
- tablet: 640px to 1023px — Tablets, large phones
- desktop: 1024px to 1439px — Standard laptops
- wide: 1440px and above — Large monitors

### Responsive Changes

#### Typography Scale

- Display: 80px (desktop), 56px (tablet), 48px (mobile)
- H1: 56px (desktop), 40px (tablet), 32px (mobile)
- H2: 40px (desktop), 32px (tablet), 24px (mobile)
- H3: 24px (desktop), 20px (tablet), 20px (mobile)
- Body: 18px (desktop), 16px (tablet), 16px (mobile)

#### Layout Adjustments

- Hero Grid: 2 columns (desktop), 1 column (tablet/mobile)
- Work Cards: Full width stacked (all breakpoints)
- Team Grid: 4 columns (desktop), 3 columns (tablet), 2/1 columns (mobile)
- About Split: 60/40 (desktop), 50/50 (tablet), stacked (mobile)
- Contact Form: 560px max (desktop), 100% - 48px padding (tablet), 100% - 32px padding (mobile)

### Performance Scaling

- Particle Count: 5000 (desktop), 2000 (tablet), 500 (mobile)
- Image Quality: 2x retina (desktop), 1.5x (tablet), 1x (mobile)
- Transition Duration: 400ms (desktop), 300ms (tablet), 250ms (mobile)
- Scroll Velocity: 1x (desktop), 0.8x (tablet), 0.6x (mobile)
- Parallax Intensity: 20px (desktop), 15px (tablet), 10px (mobile)

---

## 9. Performance & Accessibility

### Performance Metrics

- Lighthouse Score: 90+ (Performance, Accessibility)
- First Contentful Paint: < 1.5s
- Largest Contentful Paint: < 2.5s
- Total Blocking Time: < 200ms
- Cumulative Layout Shift: < 0.1
- Animation FPS: 60fps
- Time to Interactive: < 3s

### Optimization Techniques

- Image Optimization: Next.js Image component, WebP format, responsive srcset
- Code Splitting: Dynamic imports for heavy components (Three.js, GSAP)
- Lazy Loading: Intersection Observer for below-fold images
- Font Optimization: next/font with subset, display: swap
- Animation Optimization: will-change, transform/opacity only, GPU acceleration
- Bundle Size: Tree shaking, noUnusedLocals, minimal dependencies

### Accessibility

#### Reduced Motion

When prefers-reduced-motion is enabled:
- All page transitions: instant (0ms)
- Scroll animations: disabled
- Hover effects: color change only, no transform
- Form focus: no glow effect
- Particles: static or hidden
- Staggered reveals: all elements visible at once

#### Color Contrast

All text combinations meet WCAG AA:
- Ivory on Obsidian: 16.1:1 (pass AAA)
- Ember on Obsidian: 4.6:1 (pass AA)
- Mist on Obsidian: 8.5:1 (pass AAA)
- Slate on Ivory: 7.2:1 (pass AAA)

#### Keyboard Navigation

- Tab: Navigate focusable elements
- Enter: Activate focused element
- Escape: Close modals, cancel transitions
- Arrow Keys: Navigate within components

---

## 10. File Structure

```
/
├── app/
│   ├── layout.tsx
│   ├── page.tsx
│   ├── work/
│   │   └── page.tsx
│   ├── about/
│   │   └── page.tsx
│   ├── contact/
│   │   └── page.tsx
│   └── admin/
│       ├── layout.tsx
│       ├── login/
│       │   └── page.tsx
│       └── dashboard/
│           └── page.tsx
├── components/
│   ├── layout/
│   │   ├── Navigation.tsx
│   │   ├── Footer.tsx
│   │   └── PageTransition.tsx
│   ├── sections/
│   │   ├── Hero.tsx
│   │   ├── FeaturedWork.tsx
│   │   ├── ProjectListing.tsx
│   │   ├── Philosophy.tsx
│   │   ├── TeamGrid.tsx
│   │   └── ContactForm.tsx
│   ├── ui/
│   │   ├── Button.tsx
│   │   ├── Card.tsx
│   │   ├── Input.tsx
│   │   └── AnimatedText.tsx
│   └── three/
│       ├── Particles.tsx
│       └── TransitionOverlay.tsx
├── context/
│   ├── NavigationContext.tsx
│   └── ScrollContext.tsx
├── hooks/
│   ├── useScrollVelocity.ts
│   ├── useIntersectionObserver.ts
│   └── useReducedMotion.ts
├── lib/
│   ├── animations.ts
│   └── validation.ts
├── styles/
│   └── globals.css
├── public/
│   ├── fonts/
│   └── images/
└── prd.md
```
