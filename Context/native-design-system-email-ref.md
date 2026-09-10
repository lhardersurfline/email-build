# Native Design System — Email Reference

Quick-reference for building and updating email templates using the Native design system.
Covers **typography**, **colorway**, **buttons**, and **MJML implementation patterns**.
Token names match Figma variables. MJML attribute equivalents are noted where relevant.

> **Figma source:** [`Native Design System`](https://www.figma.com/design/ad0b6PGUq9qq7PYqJ2E7yC/Native-Design-System?node-id=0-1&m=dev)  
> Key node IDs: Full Width Buttons `9766:21479` · Control Buttons `8323:18503` · Logo glyph `14238:12106`

---

## Typography

**Font Family:** `Linear Sans`  
**Fallback stack (email-safe):** `Linear Sans, Arial, Helvetica, sans-serif`

> **Note on quoting:** Do not wrap `Linear Sans` in inner quotes inside MJML attribute values (e.g. `'"Linear Sans"'`). MJML injects them literally into compiled `style=""` HTML attributes, producing malformed output like `<div linear sans",arial...>`. Use `font-family='Linear Sans, Arial, Helvetica, sans-serif'` — unquoted multi-word font names resolve correctly in CSS.

All button and UI text uses **Semi Bold (600)** weight. Body copy and supporting text use the scale below.

### Design system UI scale (buttons and labels)

| Token Name | Size | Line Height | Weight | Use Case |
|---|---|---|---|---|
| `Font Size/Sub Title 1` | 16px | 20px | 600 | Primary button labels, large UI text |
| `Font Size/Sub Title 2` | 14px | 18px | 600 | Medium button labels, secondary UI |
| `Font Size/Sub Title 3` | 12px | 16px | 600 | Small button labels, captions |

### Email type scale

Applied in the feature update template. Use these values for email body composition.

| Role | Size | Line Height | Weight | MJML css-class |
|---|---|---|---|---|
| Section label | 11px | 16px | 600 | `muted-text` |
| H1 — email/section title | 28px | 36px | 600 | `heading-h1 body-text` |
| H2 — card headline | 22px | 30px | 600 | `heading-h2 body-text` |
| Body paragraph | 16px | 25px | 400 | `body-text` |
| CTA button label | 16px | 20px | 600 | — |

---

## Loading Linear Sans in Email

**Do not use `<mj-font>`** — that helper only works for Google Fonts hosted URLs. Linear Sans is served from Surfline's CDN and must be declared via `<mj-raw><style>@font-face</style></mj-raw>`.

**Do not use a CSS4 weight range** (`font-weight: 400 600`). The CDN serves separate files per weight, not a variable font. A range declaration causes email clients to apply synthetic bold (artificially thickened glyphs) to whichever single file loads. Outlook ignores range syntax entirely and falls back to Arial.

**Declare three separate `@font-face` blocks — one per weight:**

```xml
<mj-raw>
  <style>
    @font-face {
      font-family: 'Linear Sans';
      src: url('https://wa.cdn-surfline.com/quiver/0.20.6/fonts/linear-sans/linear-sans-regular.woff2') format('woff2');
      font-weight: 400;
      font-style: normal;
    }
    @font-face {
      font-family: 'Linear Sans';
      src: url('https://wa.cdn-surfline.com/quiver/0.20.6/fonts/linear-sans/linear-sans-semibold.woff2') format('woff2');
      font-weight: 600;
      font-style: normal;
    }
    @font-face {
      font-family: 'Linear Sans';
      src: url('https://wa.cdn-surfline.com/quiver/0.20.6/fonts/linear-sans/linear-sans-bold.woff2') format('woff2');
      font-weight: 700;
      font-style: normal;
    }
  </style>
</mj-raw>
```

Outlook will ignore the web font and fall back to Arial — always verify the fallback stack reads correctly at the declared sizes.

---

## Colorway

### Core Palette

| Token | Hex | Usage |
|---|---|---|
| `Colour/Utility/Accent` | `#3A70E0` | Primary CTA, focus rings, links |
| `Colour/Utility/White` | `#FFFFFF` | Text on accent/dark backgrounds |
| `Colour/Base & Surfaces/Grey-100` | `#171717` | Heading text, upsell/dark button bg |
| `Colour/Base & Surfaces/Grey 200` | `#424242` | Body text, secondary/tertiary button labels |
| `Colour/Base & Surfaces/Grey 300` | `#98A2AF` | Section labels, inactive/disabled text |
| `Colour/Base & Surfaces/Grey 400` | `#DDE4F1` | Secondary button bg, dividers, BETA badge bg |
| `Colour/Base & Surfaces/Grey 500` | `#EEF4FB` | Tertiary button bg, secondary card surface |
| `Colour/Base & Surfaces/Card Elevation` | `#FFFFFF` | Primary card/container backgrounds |

### Email-specific surface tokens

| Token | Hex | Usage |
|---|---|---|
| Banner background | `#171717` | Top logo bar (near-black; dark mode safe — `#0A1628` deep navy caused inversion issues on some clients) |
| Email body background | `#F5F7FA` | Outer wrapper behind card columns |

### Status / Semantic

| Token | Hex | Usage |
|---|---|---|
| `Colour/Status/Callout High Contrast` | `#D50000` | Error text, callout button label |
| `Colour/Status/Callout Transparent` | `rgba(213, 0, 0, 0.10)` | Callout button background |

### Transparency

| Token | Value | Usage |
|---|---|---|
| `Colour/Utility/Light Border` | `rgba(152, 162, 175, 0.24)` | Outline button border |
| `Colour/Base & Surfaces/Transparency/Transparent Dark-48` | `rgba(23, 23, 23, 0.48)` | Overlay button bg (glass effect) |

---

### Dark Mode

The design system supports both light and dark modes. Use the MJML dark mode pattern with `<mj-style>` overrides.

| Element | Light Mode | Dark Mode override |
|---|---|---|
| Email background | `#FFFFFF` | `#121212` |
| Card/container surface | `#FFFFFF` (Card Elevation) | `#1E1E1E` |
| Body text | `#424242` (Grey 200) | `#F1F1F1` |
| Muted text | `#98A2AF` (Grey 300) | `#6B7280` |
| Accent | `#3A70E0` | `#5B8AE8` (lighten for contrast) |
| Primary button bg | `#3A70E0` | `#3A70E0` (unchanged) |
| Secondary button bg | `#DDE4F1` | `#2A2A3A` |
| Tertiary button bg | `#EEF4FB` | `#1A1A2A` |

**MJML dark mode block** — place in `<mj-head>`:

```xml
<mj-raw>
  <meta name="color-scheme" content="light dark">
  <meta name="supported-color-schemes" content="light dark">
</mj-raw>
<mj-style>
  @media (prefers-color-scheme: dark) {
    .email-body { background-color: #121212 !important; }
    .email-card { background-color: #1E1E1E !important; }
    .body-text  { color: #F1F1F1 !important; }
    .muted-text { color: #6B7280 !important; }
  }
</mj-style>
```

---

## Buttons

**Border radius:** `99px` (fully pill-shaped — use `border-radius="99px"` on `<mj-button>`)  
**Font:** Linear Sans, Semi Bold (600)  
**All buttons:** `overflow: hidden`, text is `white-space: nowrap`

### Size Scale

| Size | Height | Padding (H) | Font Size | Line Height |
|---|---|---|---|---|
| Large | 48px | 32px | 16px (`Sub Title 1`) | 20px |
| Medium | 40px | 24px | 14px (`Sub Title 2`) | 18px |
| Small | 32px | 16px | 12px (`Sub Title 3`) | 16px |

---

### Button Variants

> **font-family in all button examples:** Use `font-family='Linear Sans, Arial, Helvetica, sans-serif'` (single-quoted attribute, no inner double quotes). See the typography note above.

#### Primary Active
The default CTA. Use for the single highest-priority action per email.

| Property | Value |
|---|---|
| Background | `#3A70E0` (Accent) |
| Text color | `#FFFFFF` (White) |

```xml
<mj-button background-color="#3A70E0" color="#FFFFFF"
  font-family='Linear Sans, Arial, Helvetica, sans-serif'
  font-size="16px" font-weight="600" line-height="20px"
  inner-padding="14px 32px" border-radius="99px">
  Button label
</mj-button>
```

---

#### Secondary
Softer CTA for paired actions (e.g. "Learn more" alongside a primary).

| Property | Value |
|---|---|
| Background | `#DDE4F1` (Grey 400) |
| Text color | `#424242` (Grey 200) |

```xml
<mj-button background-color="#DDE4F1" color="#424242"
  font-family='Linear Sans, Arial, Helvetica, sans-serif'
  font-size="16px" font-weight="600" line-height="20px"
  inner-padding="14px 32px" border-radius="99px">
  Button label
</mj-button>
```

---

#### Tertiary
Low-emphasis, lightest background. Use for optional/supplemental actions.

| Property | Value |
|---|---|
| Background | `#EEF4FB` (Grey 500) |
| Text color | `#424242` (Grey 200) |

```xml
<mj-button background-color="#EEF4FB" color="#424242"
  font-family='Linear Sans, Arial, Helvetica, sans-serif'
  font-size="16px" font-weight="600" line-height="20px"
  inner-padding="14px 32px" border-radius="99px">
  Button label
</mj-button>
```

---

#### Inactive / Disabled
Visually suppressed; not interactive (emails don't support real disabled states).

| Property | Value |
|---|---|
| Background | `#DDE4F1` (Grey 400) |
| Text color | `#98A2AF` (Grey 300) |

```xml
<mj-button background-color="#DDE4F1" color="#98A2AF"
  font-family='Linear Sans, Arial, Helvetica, sans-serif'
  font-size="16px" font-weight="600" line-height="20px"
  inner-padding="14px 32px" border-radius="99px">
  Button label
</mj-button>
```

---

#### Upsell / Dark
Used for premium upgrade prompts. Near-black background.

| Property | Value |
|---|---|
| Background | `#171717` (Grey 100) |
| Text color | `#FFFFFF` (White) |

```xml
<mj-button background-color="#171717" color="#FFFFFF"
  font-family='Linear Sans, Arial, Helvetica, sans-serif'
  font-size="16px" font-weight="600" line-height="20px"
  inner-padding="14px 32px" border-radius="99px">
  Button label
</mj-button>
```

---

#### Callout (Error / Urgent)
Reserved for destructive actions, alerts, or urgent warnings.

| Property | Value |
|---|---|
| Background | `rgba(213, 0, 0, 0.10)` |
| Text color | `#D50000` (Callout High Contrast) |

> **Outlook compatibility:** `rgba` backgrounds are not supported in Outlook. Use `#FFE5E5` as a solid hex fallback. This applies to any button or section using `rgba` — always provide a hex fallback.

```xml
<mj-button background-color="#FFE5E5" color="#D50000"
  font-family='Linear Sans, Arial, Helvetica, sans-serif'
  font-size="16px" font-weight="600" line-height="20px"
  inner-padding="14px 32px" border-radius="99px">
  Button label
</mj-button>
```

---

#### Outline / Focus
Transparent background with accent border. Use for secondary emphasis.

| Property | Value |
|---|---|
| Background | transparent |
| Border | `1.5px solid #3A70E0` |
| Text color | `#3A70E0` (Accent) |

```xml
<mj-button background-color="transparent" color="#3A70E0"
  border="1.5px solid #3A70E0"
  font-family='Linear Sans, Arial, Helvetica, sans-serif'
  font-size="16px" font-weight="600" line-height="20px"
  inner-padding="14px 32px" border-radius="99px">
  Button label
</mj-button>
```

---

#### Overlay (Glass)
Used over images or dark/blurred backgrounds. Semi-transparent dark.

| Property | Value |
|---|---|
| Background | `rgba(23, 23, 23, 0.48)` |
| Text color | `#FFFFFF` (White) |

> **Note:** `backdrop-filter: blur` is not supported in email. Simulate with a dark semi-transparent bg over an image section. Provide a solid fallback for Outlook.

```xml
<mj-button background-color="rgba(23,23,23,0.48)" color="#FFFFFF"
  font-family='Linear Sans, Arial, Helvetica, sans-serif'
  font-size="16px" font-weight="600" line-height="20px"
  inner-padding="14px 32px" border-radius="99px">
  Button label
</mj-button>
```

---

### Button Variant Quick-Reference

| Variant | Background | Text | When to use |
|---|---|---|---|
| Primary Active | `#3A70E0` | `#FFFFFF` | Main CTA |
| Secondary | `#DDE4F1` | `#424242` | Paired secondary action |
| Tertiary | `#EEF4FB` | `#424242` | Low-emphasis / optional |
| Inactive | `#DDE4F1` | `#98A2AF` | Disabled state (visual only) |
| Upsell | `#171717` | `#FFFFFF` | Premium upgrade prompts |
| Callout | `rgba(213,0,0,0.10)` / `#FFE5E5` fallback | `#D50000` | Alerts, errors |
| Outline/Focus | transparent | `#3A70E0` | Secondary emphasis with border |
| Overlay | `rgba(23,23,23,0.48)` | `#FFFFFF` | On image or dark bg |

### Chevron button (icon CTA)

Used within feature cards as a left-aligned secondary tap target. Not an `mj-button` — rendered as a hosted PNG image to ensure consistent rendering across clients. SVG is not supported in email `<img>` tags.

```xml
<mj-image
  src="https://braze-images.com/appboy/communication/assets/image_assets/images/6a18d458256bf70083ce8b2b/original.png?1780012119"
  alt="Go to [Feature]"
  width="32px" height="32px"
  align="left"
  href="DESTINATION_URL"
  target="_blank"
  padding="0 0 32px 0" />
```

Display size is 32×32px. The Braze-hosted asset is a blue circle with a white right-pointing chevron, sourced from the Native DS Figma component.

---

## MJML `mj-attributes` Starter Block

Drop this into `<mj-head>` to apply Native DS defaults globally across a template. All padding defaults to `0` — this prevents MJML's built-in defaults from adding unexpected space. Every section and element then declares its own padding explicitly.

```xml
<mj-attributes>
  <mj-all font-family='Linear Sans, Arial, Helvetica, sans-serif' />
  <mj-text font-size="16px" line-height="24px" color="#424242" padding="0" />
  <mj-image padding="0" />
  <mj-section padding="0" />
  <mj-divider border-color="#DDE4F1" border-width="1px" border-style="solid" padding="0" />
</mj-attributes>
```

> **Changes from previous version:** `font-family` no longer uses inner double quotes (see typography note). `padding="0"` added to `mj-text`, `mj-image`, and `mj-section`. `mj-divider` defaults added. `mj-button` removed from global defaults — declare button styles inline per instance.

---

## Assets

| Asset | URL | Display size |
|---|---|---|
| Surfline logo (white, transparent bg) | `https://braze-images.com/appboy/communication/assets/image_assets/images/6a18d4583fd053008385595d/original.png?1780012119` | 29×40px |
| Chevron CTA button | `https://braze-images.com/appboy/communication/assets/image_assets/images/6a18d458256bf70083ce8b2b/original.png?1780012119` | 32×32px |

All assets are hosted on Braze CDN. Do not embed images as base64 data URIs in production — many email security filters block or strip them.

If sourcing new icon assets from Figma SVGs: use `sharp` to rasterize at 2× the intended display size for retina output, then export as PNG.

---

## Implementation Notes

**`border-radius` on sections:** MJML compiles section `border-radius` into VML for Outlook. Avoid rounded corners on `mj-section`. Use `border-radius` only on `mj-image` (12px for feature images) and `mj-button` (99px for pill shape).

**SVG in email:** Not supported in `<img>` tags across email clients. All icon assets must be rasterized PNG.

**Unsubscribe block:** Manage as a separate repeatable component appended in Braze or the sending platform. Do not include inline in the template MJML.

**Output formatting:** MJML's `--config.minify=true` flag is required for correct mobile column stacking behavior but produces single-line HTML. Always prettify before handoff using BeautifulSoup. Target size under 102KB compiled to avoid Gmail clipping.

---

## Figma Source

[Native Design System — Figma](https://www.figma.com/design/ad0b6PGUq9qq7PYqJ2E7yC/Native-Design-System?node-id=0-1&m=dev)

Key node IDs for direct inspection:
- **Full Width Buttons:** `9766:21479`
- **Control Buttons (all sizes/states):** `8323:18503`
- **Color variables:** accessible via any button node's variable panel
- **Surfline logo glyph:** `14238:12106`
