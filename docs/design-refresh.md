# Design Refresh — Eshet Chayil Landing Page

## Rationale
The v2 page used a teal-dominant palette with strong glows, aggressive gradients,
and flashy hover transforms. For a spiritual non-profit (Merkaz Neshama) the goal
is **premium calm + authenticity**: think a high-end retreat brochure, not a SaaS
product page.

---

## 1. Colour Tokens

| Token | Hex | Usage |
|---|---|---|
| `--brand-blue-900` | `#004060` | Primary dark — headings, dark sections bg |
| `--brand-blue-800` | `#005070` | Footer bg |
| `--brand-blue-700` | `#006090` | Syllabus markers, badge borders |
| `--brand-blue-500` | `#2090C0` | Links, focus rings, stage numbers |
| `--accent-amber-700` | `#C05030` | Primary CTA gradient start |
| `--accent-amber-600` | `#D06030` | Primary CTA gradient end |
| `--accent-amber-500` | `#E09060` | Small highlights only (feature icons, quote marks) |
| `--paper` | `#F7F3ED` | Warm cream page background |
| `--paper-dark` | `#EDE8DF` | Alternating section / card bg |
| `--white` | `#FFFFFF` | Card surfaces |
| `--ink` | `#0B1F2A` | Body text |
| `--muted` | `#4A6070` | Secondary text |

Contrast checks (against `--paper` bg):
- `--ink` on `--paper` → 14.2:1 (AAA)
- `--muted` on `--paper` → 5.8:1 (AA large + normal)
- White on `--brand-blue-900` → 11.5:1 (AAA)
- White on `--accent-amber-700` → 3.1:1 (AA large — CTA buttons only)

---

## 2. Typography Rules

| Element | Size | Weight | Line-height | Letter-spacing |
|---|---|---|---|---|
| Body (`p`) | 1rem–1.15rem | 400 | 1.75 | 0 |
| h1 | clamp(2.2rem, 5vw, 3.4rem) | 700 | 1.2 | -0.02em |
| h2 | clamp(1.8rem, 4vw, 2.6rem) | 700 | 1.25 | -0.015em |
| h3 | clamp(1.3rem, 2.5vw, 1.7rem) | 700 | 1.3 | 0 |
| h4 | clamp(1.05rem, 1.8vw, 1.2rem) | 600 | 1.4 | 0 |

Fonts unchanged: **Heebo** (body) + **Frank Ruhl Libre** (display).

---

## 3. Spacing Scale

```
--space-xs:  4px
--space-sm:  8px
--space-md:  12px
--space-base: 16px
--space-lg:  24px
--space-xl:  32px
--space-2xl: 48px
--space-3xl: 64px
```

Section padding: `64px` desktop → `48px` mobile.
Container max-width: `1100px`.

---

## 4. Radius & Shadow

| Token | Value | Notes |
|---|---|---|
| `--radius-sm` | `8px` | Tags, badges |
| `--radius-md` | `12px` | Form inputs, FAQ items |
| `--radius-lg` | `18px` | Cards |
| `--radius-xl` | `24px` | Hero logo frame, form wrapper |
| `--shadow-soft` | `0 2px 12px rgba(11,31,42, 0.08)` | Default card |
| `--shadow-medium` | `0 6px 24px rgba(11,31,42, 0.12)` | Hover / journey card |

No `--shadow-strong` (too flashy). No `--radius-xl: 40px` (too bubbly).

---

## 5. Components

### Buttons
- **Primary (CTA):** amber gradient `135deg #C05030 → #D06030`, white text, `border-radius: 10px`.
  Hover: slight darken via `filter: brightness(0.92)`, no aggressive translateY.
- **Secondary (outline):** `border: 1.5px solid --brand-blue-700`, text `--brand-blue-700`,
  transparent bg. Hover: bg fills to `--brand-blue-700`, text white.
- Min touch target: 44px height on mobile.

### Cards (pain, method, testimonial, syllabus)
- bg `--white`, `border: 1px solid rgba(11,31,42,0.07)`, `box-shadow: --shadow-soft`.
- Hover: shadow upgrades to `--shadow-medium`. **No translateY**. Transition 0.25s.
- Pain cards: right-border accent → `--accent-amber-500` (2px, not 4px).

### Hero
- Background: `linear-gradient(170deg, --brand-blue-900, --brand-blue-800)`.
- Pseudo-element overlay: texture WebP at 6% opacity via `background-blend-mode`.
- Logo: white version, max 140px wide, no heavy drop-shadow (subtle `filter: drop-shadow(0 2px 8px rgba(0,0,0,0.25))`).
- Rays decoration removed (too flashy for calm aesthetic).
- Scroll indicator: keep but remove bounce animation → static chevron.

### Dark sections (methodology, logistics, CTA)
- bg: `--brand-blue-900` with very subtle radial warm highlight (amber 8% opacity).
- Method-card / logistics-item: `rgba(255,255,255,0.06)` bg, `1px solid rgba(255,255,255,0.08)` border.
- Method-icon bg: single `--accent-amber-700` (no gradient).

### Team section
- Photo: `rav-table.jpg` (portrait 1066×1600) in the 3:4 card — `object-position: center top`.
- Add small "moment" card below using `rav.jpg` (landscape), 100% width of the photo column, `border-radius: --radius-lg`, `object-fit: cover`, height ~160px. Caption via `figcaption`.
- Both images: `<picture>` element with WebP source + JPG fallback.

### Animations
- Keep IntersectionObserver fade-in but make it gentler: `translateY(16px)` (down from 30px), duration `0.6s` (down from 0.8s).
- Remove `bounce` keyframe on scroll indicator.
- Card hover: no transform, only shadow change.

---

## 6. Image Usage Map

| Image | Location | Treatment |
|---|---|---|
| `logo-white` | Hero | 140px, white version |
| `logo-transparent` | Footer | 120px |
| `rav-table.jpg` | Team photo card | 3:4 aspect, `object-fit: cover`, `object-position: center top` |
| `rav.jpg` | Team "moment" card | Landscape, 160px tall, below main photo |
| `texture.png/webp` | Hero overlay | 6% opacity pseudo-element |

All below-fold images get `loading="lazy"`. All `<img>` get `width`/`height` attributes.

---

## 7. Duplicate Code Fix
Lines 2073–2099 in original: orphaned duplicate `if/catch` block after the form handler.
Will be removed.

---

## 8. TODOs (not in scope)
- Copy / text changes (reserved for later per brief)
- Payment integration
- SEO meta tags refresh
- Cookie consent banner
