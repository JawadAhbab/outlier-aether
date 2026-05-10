# Product Requirements Document: 3D Interactive Experience

## 1. Project Overview

**Project Name:** Void Walker - Interactive 3D Portfolio Experience

**Type:** 3D & WebGL / Game

**Core Functionality:** An immersive 3D portfolio experience where users navigate through a floating cosmic void containing interactive sculptural elements. Users explore interconnected chambers by clicking on portal anchors, triggering smooth camera transitions and revealing hidden content pieces.

**Target Users:** Creative professionals, potential clients, design enthusiasts seeking memorable web experiences.

---

## 2. Visual Identity

### Color Palette

| Role | Color | Hex |
|------|-------|-----|
| Primary Background | Deep Void Black | #050508 |
| Secondary Background | Cosmic Navy | #0a0e1a |
| Accent Primary | Electric Cyan | #00f5d4 |
| Accent Secondary | Neon Magenta | #f72585 |
| Accent Tertiary | Warm Gold | #ffd60a |
| Text Primary | Pure White | #ffffff |
| Text Secondary | Muted Silver | #a0a0b0 |
| Glow Effect | Soft Cyan | #00f5d480 |

### Typography

| Element | Font | Weight | Size | Line Height |
|---------|------|--------|------|-------------|
| Headings H1 | Space Grotesk | 700 | 4.5rem | 1.1 |
| Headings H2 | Space Grotesk | 600 | 2.5rem | 1.2 |
| Body Text | Inter | 400 | 1rem | 1.6 |
| Captions | Inter | 300 | 0.75rem | 1.4 |
| UI Labels | JetBrains Mono | 500 | 0.875rem | 1.2 |

### Spacing System

- Base unit: 8px
- Small spacing: 8px (xs)
- Medium spacing: 24px (md)
- Large spacing: 48px (lg)
- Section padding: 64px
- Border radius (cards): 16px
- Border radius (buttons): 8px
- Content max-width: 1200px

---

## 3. Layout & Structure

### Main Sections

**3.1 Entry Chamber (Home)**
- Central floating orb (radius: 2 units) with reactive surface shader
- Four portal anchors positioned at cardinal directions
- Ambient particle field (count: 2000 particles)
- Entry title text floating above orb

**3.2 Gallery Chamber**
- Grid of 6 sculptural pieces arranged in 2x3 layout
- Each piece: 1.5 unit scale, unique geometry
- Interactive hover states with glow intensification
- Click reveals expanded detail modal

**3.3 Timeline Chamber**
- Linear arrangement of 5 milestone spheres along z-axis
- Connecting line with animated energy flow
- Scrolling triggers sequential illumination

**3.4 Contact Chamber**
- Central pedestal with contact form
- Floating label inputs with 3D depth effect
- Submit button with particle burst on click

### Navigation

- Fixed HUD overlay with 4 chamber indicators
- Active chamber highlighted with accent glow
- ESC key returns to Entry Chamber
- Mobile: swipe gestures for chamber switching

---

## 4. Animation & Motion Specifications

### Camera Transitions

| Transition | Duration | Easing | Description |
|------------|----------|--------|-------------|
| Chamber Entry | 2000ms | cubic-bezier(0.4, 0, 0.2, 1) | Smooth fly-in to default view |
| Chamber Switch | 1500ms | cubic-bezier(0.76, 0, 0.24, 1) | Arc path with slight zoom |
| Focus Element | 800ms | cubic-bezier(0.34, 1.56, 0.64, 1) | Spring-like approach |
| Return Home | 1200ms | cubic-bezier(0.4, 0, 0.6, 1) | Gentle pull to center |

### Object Animations

| Object | Animation | Duration | Loop | Trigger |
|--------|-----------|----------|------|---------|
| Central Orb | Slow rotation | 20000ms | Yes | Auto |
| Orb Surface | Vertex displacement | 3000ms | Yes | Wave pattern |
| Portal Anchors | Pulse glow | 2000ms | Yes | Sine wave |
| Particles | Drift motion | Variable | Yes | Perlin noise |
| Gallery Pieces | Float bob | 4000ms | Yes | Offset sine |

### Interaction Feedback

| Interaction | Visual Response | Duration |
|-------------|-----------------|----------|
| Hover (3D object) | Emissive intensity +50%, scale 1.05 | 200ms |
| Click (portal) | Radial burst particles, sound cue | 400ms |
| Focus (form input) | Border glow animate, label float up | 300ms |
| Submit | Button dissolve, particle explosion | 600ms |

### Scroll Animations

- Timeline Chamber: Each milestone sphere illuminates as it enters viewport center
- Progress indicator fills proportionally to scroll position
- Parallax depth: foreground 1.0x, midground 0.6x, background 0.2x

---

## 5. Technical Requirements

### Three.js Scene Configuration

**Renderer:**
- WebGLRenderer with antialias: true
- alpha: true, powerPreference: "high-performance"
- Pixel ratio: Math.min(window.devicePixelRatio, 2)

**Camera:**
- PerspectiveCamera, FOV: 50, near: 0.1, far: 1000
- Position: (0, 0, 8) default
- Controls: Custom orbit with damping (factor: 0.05)

**Lighting:**

| Light | Type | Color | Intensity | Position |
|-------|------|-------|------------|----------|
| Ambient | AmbientLight | #404060 | 0.4 | N/A |
| Key | DirectionalLight | #ffffff | 1.2 | (5, 5, 5) |
| Fill | DirectionalLight | #00f5d4 | 0.3 | (-3, 2, 4) |
| Accent | PointLight | #f72585 | 0.8 | (0, -2, 3) |

### Materials & Shaders

**Central Orb Material (Custom Shader):**
```
- Base: MeshPhysicalMaterial
- Roughness: 0.15, Metalness: 0.3
- Transmission: 0.6, Thickness: 2.0
- Vertex shader: Simplex noise displacement (amplitude: 0.15, frequency: 2.0)
- Fragment shader: Fresnel rim glow (power: 3.0, color: #00f5d4)
```

**Portal Anchor Material:**
```
- Emissive material with animated intensity
- Glow radius: 0.5 units
- Color interpolation between #00f5d4 and #f72585
```

**Particle System:**
```
- Count: 2000 particles
- Size: 0.02 - 0.08 units (random)
- Shape: Point geometry with circular texture
- Movement: Perlin noise field (speed: 0.5, scale: 0.3)
```

### Post-Processing Pipeline

| Effect | Settings | Purpose |
|--------|----------|---------|
| Bloom | strength: 1.5, radius: 0.4, threshold: 0.85 | Glow enhancement |
| Chromatic Aberration | offset: 0.002 | Slight color fringing |
| Vignette | darkness: 0.5, offset: 0.3 | Edge darkening |
| Film Grain | intensity: 0.08 | Organic texture |

### Physics Simulation

**Gravity:** None (floating environment)
**Collision:** Raycasting for click detection only
**Spring System:** For camera zoom and element focus
```
- stiffness: 0.08
- damping: 0.7
- mass: 1.0
```
**Particle Physics:** Simplified Verlet integration for burst effects

---

## 6. Application Logic

### Data Model

**User (Session):**

| Field | Type | Description |
|-------|------|-------------|
| id | UUID | Unique session identifier |
| visitedChambers | Array[String] | List of visited chamber IDs |
| lastPosition | Vector3 | Camera position on exit |
| preferences | Object | Motion preference (reduced/preferred) |

**Gallery Item:**

| Field | Type | Description |
|-------|------|-------------|
| id | String | Unique identifier |
| title | String | Display name |
| description | String | Detail text |
| geometry | String | Three.js geometry type |
| materialProps | Object | Material configuration |
| position | Vector3 | World position |
| rotation | Vector3 | Initial rotation |

**Timeline Milestone:**

| Field | Type | Description |
|-------|------|-------------|
| id | String | Unique identifier |
| year | Number | Year value |
| title | String | Milestone title |
| description | String | Detail description |
| position | Number | Z-axis position |

### Authentication Flow

- No user authentication required for MVP
- Session state stored in localStorage
- Future: OAuth integration (Google, GitHub)

### Core Feature Flows

**Flow 1: Chamber Navigation**

```
1. User enters site
2. Entry Chamber loads with central orb animation
3. User clicks portal anchor
4. System disables current controls
5. Camera animates along arc path (1500ms)
6. New chamber elements fade in (staggered, 200ms intervals)
7. New chamber controls enable
8. HUD updates active indicator
```

**Flow 2: Gallery Interaction**

```
1. User enters Gallery Chamber
2. 6 sculptural pieces render in 2x3 grid
3. User hovers over piece
   - Scale increases to 1.05 (200ms)
   - Emissive intensity increases
   - Cursor changes to pointer
4. User clicks piece
   - Camera zooms to focus position (800ms)
   - Detail modal slides in from right (400ms)
   - Background blur applies
5. User closes modal (X button or click outside)
   - Modal slides out (300ms)
   - Camera returns to default (600ms)
```

### State Management

**Global State (Zustand):**

```javascript
{
  currentChamber: 'entry' | 'gallery' | 'timeline' | 'contact',
  cameraTarget: Vector3,
  isTransitioning: boolean,
  selectedItem: GalleryItem | null,
  reducedMotion: boolean,
  audioEnabled: boolean
}
```

### Error Handling

| Scenario | Handling |
|----------|----------|
| WebGL not supported | Show fallback 2D version with CSS animations |
| Shader compilation error | Fallback to MeshStandardMaterial |
| Asset load failure | Retry 3 times, then show placeholder |
| Mobile performance drop | Auto-reduce particle count to 500 |

---

## 7. Responsive Behavior

### Breakpoints

| Name | Width | Changes |
|------|-------|---------|
| Mobile | < 768px | Single column, simplified 3D, touch controls |
| Tablet | 768px - 1024px | 2-column gallery, reduced particles |
| Desktop | 1024px - 1440px | Full experience |
| Large | > 1440px | Increased FOV to 55, larger typography |

### Mobile Adaptations

- Particles reduced from 2000 to 500
- Bloom strength reduced to 0.8
- Camera controls: touch drag instead of mouse
- Chamber switch: swipe left/right
- Gallery: 2x3 grid becomes 1x6 vertical scroll
- Typography scaled to 0.75x desktop size

### Heavy Visual Scaling

| Element | Desktop | Mobile |
|---------|---------|--------|
| Particles | 2000 | 500 |
| Bloom | 1.5 | 0.8 |
| Shadows | Enabled | Disabled |
| Post-processing | Full | Bloom only |

---

## 8. Performance & Accessibility

### Performance Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| Frame Rate | 60 FPS | requestAnimationFrame timing |
| First Contentful Paint | < 1.5s | Lighthouse |
| Largest Contentful Paint | < 2.5s | Lighthouse |
| Total Bundle Size | < 500KB | Build output (excluding textures) |
| Memory Usage | < 200MB | Chrome DevTools |

### Performance Techniques

- **Geometry Instancing:** For repeated elements (particles, portals)
- **Texture Compression:** KTX2 format for textures where applicable
- **Level of Detail (LOD):** Reduced poly models for distant objects
- **Frustum Culling:** Automatic via Three.js renderer
- **Dispose Pattern:** Proper cleanup on chamber exit
- **Lazy Loading:** Chamber assets loaded on first visit

### Accessibility

**Keyboard Navigation:**
- Tab: Navigate between interactive elements
- Enter/Space: Activate focused element
- Escape: Close modals, return to Entry Chamber
- Arrow keys: Navigate gallery items

**Screen Reader Support:**
- ARIA labels on all interactive 3D elements
- Live region announcements for chamber transitions
- Alt text for all visual content in modals

**Reduced Motion (prefers-reduced-motion):**

| Change | Standard | Reduced |
|--------|----------|---------|
| Camera transitions | 1500ms animated | Instant |
| Particle animation | Continuous | Static |
| Orb rotation | 20000ms loop | Paused |
| Hover effects | 200ms transition | 0ms (no transition) |
| Bloom effect | Enabled | Disabled |

**Color Contrast:**
- All text meets WCAG AA standard (4.5:1 minimum)
- Interactive elements have visible focus states
- Glow effects do not reduce text readability

---

## 9. Audio (Optional Enhancement)

| Sound | Trigger | Duration | Volume |
|-------|---------|----------|--------|
| Ambient drone | Page load | Loop | 0.15 |
| Portal activation | Click portal | 800ms | 0.4 |
| Hover chime | Hover interactive | 150ms | 0.2 |
| Modal open | Gallery item click | 300ms | 0.3 |
| Chamber arrival | Transition complete | 500ms | 0.25 |

Audio disabled by default, toggle in HUD.

---

## 10. Implementation Notes

### Required Dependencies

```json
{
  "three": "^0.160.0",
  "zustand": "^4.5.0",
  "@react-three/fiber": "^8.15.0",
  "@react-three/drei": "^9.92.0",
  "@react-three/postprocessing": "^2.16.0",
  "gsap": "^3.12.0"
}
```

### Browser Support

- Chrome 90+
- Firefox 88+
- Safari 15+
- Edge 90+
- Mobile Safari iOS 15+
- Chrome Android 90+

---

*Document Version: 1.0*
*Last Updated: 2026-05-10*
*Author: WebDev PRD Generator*