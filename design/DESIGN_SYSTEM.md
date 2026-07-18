# Kiban Design System
**Last updated:** 2026-04-26
**Status:** Foundation — evolves with build

> Kiban (基盤) = Japanese for "foundation" or "substrate."
> This design system is the visual sibling of Kagami's.
> They share construction language but have distinct identities.
> Kagami = the mirror (personal, warm, reflective).
> Kiban = the frame (structural, cool, foundational).

---

## Brand Essence

**Name:** Kiban (基盤) — Japanese for foundation / substrate
**Tagline:** "The ground everything runs on."
**Positioning:** Model-agnostic personal AI operating system — the framework
  underneath Kagami and every AI tool you use.
**Tone:** Architectural, precise, quietly powerful. Not flashy. Not minimal
  for the sake of it. Purposeful like good infrastructure.
**Emotional register:** The confidence of a well-designed system. The satisfaction
  of infrastructure that just works. Like a clean terminal, a well-typed codebase,
  a perfectly organized config file.

### What Kiban Feels Like
- The concrete foundation before the building goes up
- A circuit board — intricate, purposeful, invisible when working
- The silence of a well-tuned system
- Not a product you show off. Infrastructure you trust.

### What Kiban Does NOT Feel Like
- Enterprise B2B software (cold, gray, lifeless)
- Consumer app (playful, colorful, ephemeral)
- Generic developer tool (GitHub-blue or VS Code-dark clone)
- Kagami (don't confuse them — they are siblings, not twins)

---

## Color Palette

All colors defined in `tokens.css`. Source of truth is there.

### Primary Colors

| Token | Hex | Usage |
|-------|-----|-------|
| `--slate-deep` | `#151B2E` | Primary dark surface, deep foundation |
| `--slate` | `#1E2640` | Secondary surface, elevated panels |
| `--slate-mid` | `#2A3350` | Borders on dark, subtle separators |
| `--teal` | `#2D7D6F` | Primary accent — action, links, active states |
| `--teal-light` | `#3D9B8B` | Hover state for teal |
| `--silver` | `#8FA3B4` | Muted accent, metadata, secondary elements |

### Surface Colors

| Token | Hex | Usage |
|-------|-----|-------|
| `--surface-light` | `#F2F5F8` | Light mode primary surface |
| `--surface-mid` | `#E8EDF2` | Secondary surface, elevated cards |
| `--border-light` | `#D0D8E0` | Light mode borders |
| `--border-dark` | `#2A3350` | Dark mode borders |

### Text Colors

| Token | Hex | Usage |
|-------|-----|-------|
| `--text` | `#1A1F2E` | Primary text (light mode) |
| `--text-sub` | `#5A6578` | Secondary / muted text |
| `--text-on-dark` | `#E8EDF2` | Text on dark surfaces |
| `--text-on-dark-sub` | `#8FA3B4` | Muted text on dark |

### Logo Colors

| Token | Hex | Usage |
|-------|-----|-------|
| `--logo-primary` | `#1E2640` | Primary logo mark on white |
| `--logo-accent` | `#2D7D6F` | Accent layer in logo (the substrate lines) |
| `--logo-reverse` | `#E8EDF2` | Logo reversed on dark backgrounds |

### Relationship to Kagami Palette

| Dimension | Kagami | Kiban |
|-----------|--------|-------|
| Dominant hue | Indigo (warm blue-violet) | Slate (cool blue-grey) |
| Accent | Gold (warm, premium) | Teal (cool, technical) |
| Secondary | Terracotta (warm) | Silver (neutral-cool) |
| Surface | Cream (organic warmth) | Light grey-blue (clinical clarity) |
| Character | Warm, personal, human | Cool, structural, architectural |

---

## Typography

### Display / Wordmark
- **Font:** DM Mono or Space Mono
- **Weight:** Regular (400) for display; Medium (500) for emphasis
- **Usage:** The "kiban" wordmark, technical display contexts, code references
- **Rationale:** Where Kagami uses a serif (editorial/warm), Kiban uses a mono
  (technical/precise). Same visual weight class, opposite typographic register.
  Signals: this is infrastructure.
- **Source:** Google Fonts (`family=DM+Mono:wght@400;500`)
- **Fallback:** `'JetBrains Mono', 'Fira Code', 'Courier New', monospace`

### Body / UI
- **Font:** Inter (shared with Kagami — interoperability)
- **Weights:** 400, 500, 600, 700
- **Usage:** All body copy, UI labels, navigation, descriptions
- **Note:** Using Inter for both products ensures documents and shared contexts
  feel consistent. The mono wordmark is the differentiator.

### Type Scale
*(Same scale as Kagami — shared system)*

| Name | Size | Weight | Font | Usage |
|------|------|--------|------|-------|
| `--text-display` | 48–72px | 400 | DM Mono | Wordmark, CLI/terminal display |
| `--text-h1` | 36px | 400 | DM Mono | Page titles |
| `--text-h2` | 28px | 600 | Inter | Section headings |
| `--text-h3` | 22px | 600 | Inter | Subsections |
| `--text-body` | 16px | 400 | Inter | Body copy |
| `--text-small` | 14px | 400 | Inter | Metadata, captions |
| `--text-code` | 14px | 400 | DM Mono | Code, config, technical strings |

---

## Logo

### Concept
Abstract horizontal layers — like geological strata, circuit board traces, or
the architecture diagram of a layered system. Three to four parallel lines of
decreasing opacity, from solid at the base to lighter at the top, conveying
depth, substrate, and foundation.

The layers echo Kiban's four-layer architecture (Universal Config → Tool Adapters
→ Vaults → Project overrides). The logo IS the architecture diagram, abstracted.

### Pairing with Kagami
The two logos form a deliberate pair:
- Kagami = two organic curves facing each other (faces, mirror, human)
- Kiban = parallel horizontal structure (layers, substrate, architecture)
- Same visual weight, same white background, same single-color treatment
- Together they read: the human layer (Kagami) rests on the foundation (Kiban)

### Style
- 2D flat — no gradients, no shadows, no 3D effects
- Two colors: primary slate mark + teal accent line (the "active layer")
- White background
- Works at all sizes: 16px favicon to 512px icon
- Geometric precision: lines are exactly parallel, consistent spacing

### Forms
1. **Primary Mark** — layered structure symbol alone
2. **Horizontal Lockup** — mark + "kiban" wordmark in DM Mono
3. **Vertical Lockup** — mark above wordmark, centered
4. **App / CLI Icon** — mark only, on white or slate-deep background
5. **Favicon** — mark only, 32×32

### Color Applications
- Light background: `#1E2640` primary + `#2D7D6F` accent on white (`#FFFFFF`)
- Dark background: `#E8EDF2` primary + `#3D9B8B` accent on `#151B2E`
- Monochrome: `#1A1F2E` single-color (print / accessibility)

### Design Research Notes — Color (2026-04-26)

**What distinguishes Kiban's palette from the saturated developer tool space:**
- Slate `#1E2640` is cooler and more architectural than HashiCorp's warm indigo, distinct from Vercel's pure black
- Teal `#2D7D6F` is rare — "startup teal" (#00B4D8 range) is different; Kiban's teal reads as *patina*, not *neon*
- The slate-to-teal relationship communicates the foundational/active layer split meaningfully

**Oversaturated in developer tools — avoid:**
- Blue-to-purple gradients (HashiCorp, Terraform, every cloud-native startup)
- Hexagonal grid patterns as primary mark (overused across all infrastructure tooling)
- Circuit board trace aesthetics (peaked 2019, now dated)
- Generic "dark mode gray" (#1A1A1A) without personality — signals no design thinking

**Research-identified accent: warm brass `#9E7D3A`**
A warm brass accent (iron with gold warmth) would distinguish premium Kiban contexts
from cold infrastructure tools. Consider for: premium tier callouts, milestone markers,
marketing materials. NOT for primary UI use — keep UI to slate/teal only.

**Comparable brands to study:**
- **Linear** — monochromatic near-black, single geometric mark, zero gradient; the gold standard
- **Vercel** — pure black/white, triangle motif; too minimal to copy but the confidence level to match
- **HashiCorp** — isometric cube mark, deep indigo; closest in category but warmer and more product-forward

---

## Component Philosophy

1. **Configuration-first** — every component has sensible defaults but is overridable
2. **Legibility at density** — designed for information-dense views (config files, layer maps)
3. **Neutral expressiveness** — not cold, but not warm; precise without being clinical
4. **Interoperability signals** — visual language signals it works with multiple tools

---

## Relationship to Kagami Design System

| Aspect | Kagami | Kiban |
|--------|--------|-------|
| Logo form | Sacred circle + reflection axis OR organic face profiles | Kanji-derived cross-base OR horizontal strata |
| Primary color | Indigo `#3D3A8A` | Slate `#1E2640` |
| Accent | Gold `#D4A843` | Teal `#2D7D6F` |
| Display font | Gloock (serif) | DM Mono (monospace) |
| Body font | Inter | Inter (shared) |
| Tone | Warm, personal | Cool, structural |
| User | Personal identity | Developer infrastructure |

Kiban's design system is intentionally subordinate to Kagami visually — Kiban is the
foundation, Kagami is what the user sees. When both appear together, Kagami takes
the prominent position.

See `C:\Dev\kagami\design\DESIGN_SYSTEM.md` for Kagami's spec.
