# Surfline Email Template — Context & Component Reference

Built and iterated in a live session. Use this as context when generating or modifying Surfline email templates.

---

## Design System Source

- **Figma:** `https://www.figma.com/design/ad0b6PGUq9qq7PYqJ2E7yC/Native-Design-System`
- **Reference file used:** `native-design-system-email-ref.md` (covers typography, colorway, buttons)
- **Template type:** Feature update / What's New

---

## Hosted Assets

| Asset | URL |
|---|---|
| Surfline logo (white, transparent bg) | `https://braze-images.com/appboy/communication/assets/image_assets/images/6a18d4583fd053008385595d/original.png?1780012119` |
| Chevron CTA button (blue circle, white arrow) | `https://braze-images.com/appboy/communication/assets/image_assets/images/6a18d458256bf70083ce8b2b/original.png?1780012119` |

Both are hosted on Braze CDN and should be referenced by URL, not embedded as base64.

---

## Toolchain

- **Format:** MJML 4.x → compiled to HTML
- **Compilation:** `npx mjml input.mjml -o output.html --config.minify=true --config.validationLevel=strict`
- **Prettification:** Always run BeautifulSoup + CSS block expansion on the compiled output before delivering. Minified output is valid but unreadable. See the prettify script below.
- **Size limit:** Keep compiled HTML under 102KB (Gmail clip threshold). Current template: ~48KB.

### Prettify script

```python
import re
from bs4 import BeautifulSoup

with open('feature-update.html', 'r') as f:
    raw = f.read()

soup = BeautifulSoup(raw, 'html.parser')
pretty = soup.prettify()

def expand_css(match):
    tag_open, css, tag_close = match.group(1), match.group(2), match.group(3)
    css = re.sub(r'\{', ' {\n      ', css)
    css = re.sub(r';(?!\s*\n)', ';\n      ', css)
    css = re.sub(r'\}', '\n    }\n    ', css)
    return f'{tag_open}\n    {css.strip()}\n   {tag_close}'

pretty = re.sub(r'(<style[^>]*>)(.*?)(</style>)', expand_css, pretty, flags=re.DOTALL)

with open('feature-update-pretty.html', 'w') as f:
    f.write(pretty)
```

---

## Typography

**Font family:** `Linear Sans, Arial, Helvetica, sans-serif`
**CDN:** `https://wa.cdn-surfline.com/quiver/0.20.6/fonts/linear-sans/`

### Font-face declaration (critical)

Declare three separate `@font-face` blocks — one per weight. Do **not** use a CSS4 range (`font-weight: 400 600`) because:
- The CDN serves separate files per weight, not a variable font.
- Range syntax causes clients to apply synthetic bold to the single loaded file.
- Outlook ignores range syntax entirely and falls back to Arial.

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

### Type scale

| Role | Size | Line height | Weight | CSS class |
|---|---|---|---|---|
| Section label | 11px | 16px | 600 | `muted-text` |
| H1 (email/section title) | 28px | 36px | 600 | `heading-h1 body-text` |
| H2 (card headline) | 22px | 30px | 600 | `heading-h2 body-text` |
| Body paragraph | 16px | 25px | 400 | `body-text` |
| CTA button label | 16px | 20px | 600 | — |

---

## Color Tokens

| Token | Hex | Usage |
|---|---|---|
| `#171717` | Near-black | Banner background, H1, H2 heading text (`#0A1628` deep navy retired — caused dark mode inversion issues) |
| `#EEF4FB` | Light blue-grey | Secondary card background (Other Updates section) |
| `#F5F7FA` | Off-white | Email body background (`email-body`) |
| `#424242` | Dark grey | Body copy |
| `#98A2AF` | Mid grey | Section labels (`muted-text`), dividers |
| `#DDE4F1` | Light grey-blue | Divider lines, BETA badge background |
| `#3A70E0` | Accent blue | Primary CTA button, links |
| `#FFFFFF` | White | Button label on accent blue |

### Dark mode overrides

```css
@media (prefers-color-scheme: dark) {
  .email-body { background-color: #121212 !important; }
  .email-card { background-color: #1E1E1E !important; }
  .body-text  { color: #F1F1F1 !important; }
  .muted-text { color: #6B7280 !important; }
}
```

---

## Global MJML Defaults

```xml
<mj-attributes>
  <mj-all font-family='Linear Sans, Arial, Helvetica, sans-serif' />
  <mj-text font-size="16px" line-height="24px" color="#424242" padding="0" />
  <mj-image padding="0" />
  <mj-section padding="0" />
  <mj-divider border-color="#DDE4F1" border-width="1px" border-style="solid" padding="0" />
</mj-attributes>
```

All padding is explicit on each section/element. Global defaults are all `0` to prevent MJML's built-in defaults from adding unexpected space.

---

## Email Structure

```
mj-wrapper (padding: 0 24px 32px 24px)
│
├── BANNER
├── SECTION HEADER
├── HERO IMAGE
│
├── FEATURE CARD (repeatable, bg: #FFFFFF)
├── DIVIDER
├── FEATURE CARD
├── DIVIDER
├── FEATURE CARD (no divider after last)
│
├── OTHER UPDATES HEADER (bg: #EEF4FB)
├── OTHER UPDATE CARD (repeatable, bg: #EEF4FB)
├── DIVIDER
├── OTHER UPDATE CARD
│
└── PRIMARY CTA
```

The wrapper provides the lateral gutters (24px each side) and sits inside the 600px body. All content is 552px wide. The lateral padding + card inset (32px each side) gives readable content at 488px.

---

## Components

### Banner

Dark navy bar with centered logo. No rounded corners.

```xml
<mj-section background-color="#171717" padding="20px 0">
  <mj-column>
    <mj-image
      src="LOGO_URL"
      alt="Surfline"
      width="29px"
      height="40px"
      align="center"
      href="https://www.surfline.com/"
      target="_blank"
      padding="0" />
  </mj-column>
</mj-section>
```

**Logo display size:** 29×40px. Logo is always clickable, linking to `https://www.surfline.com/`.

---

### Section Header

Opens a primary content group (main features or Other Updates). Contains an uppercase label and an H1.

```xml
<mj-section padding="28px 32px 20px 32px" background-color="#FFFFFF" css-class="email-card">
  <mj-column>
    <mj-text
      css-class="muted-text"
      font-size="11px" line-height="16px" font-weight="600"
      color="#98A2AF" letter-spacing="0.08em"
      padding="0 0 10px 0">
      SECTION LABEL
    </mj-text>
    <mj-text
      css-class="heading-h1 body-text"
      font-size="28px" line-height="36px" font-weight="600"
      color="#171717" padding="0">
      Section headline text here.
    </mj-text>
  </mj-column>
</mj-section>
```

For the Other Updates section header, change `background-color` to `#EEF4FB`. All other values are identical.

---

### Hero Image

Appears immediately after the section header. Inset 32px each side, 16px top gap.

```xml
<mj-section background-color="#FFFFFF" css-class="email-card" padding="16px 32px 0 32px">
  <mj-column>
    <mj-image
      src="IMAGE_URL"
      alt="Description"
      width="488px"
      border-radius="12px"
      fluid-on-mobile="true"
      href="https://www.surfline.com/"
      target="_blank" />
  </mj-column>
</mj-section>
```

All images are clickable. `fluid-on-mobile="true"` ensures full-width on small screens.

---

### Feature Card (repeatable)

The core repeatable unit. Each card is two MJML sections: an image section and a content section. Both share the same `css-class` and `background-color`. The only difference between a primary feature card (`#FFFFFF`) and an Other Updates card (`#EEF4FB`) is background color.

**Image section:**
```xml
<mj-section padding="16px 32px 0 32px" background-color="#FFFFFF" css-class="email-card">
  <mj-column>
    <mj-image
      src="IMAGE_URL"
      alt="Feature screenshot"
      width="488px"
      border-radius="12px"
      fluid-on-mobile="true"
      href="https://www.surfline.com/"
      target="_blank" />
  </mj-column>
</mj-section>
```

For cards that follow a divider (i.e., not the first card in a group), use `padding="24px 32px 0 32px"` on the image section to maintain consistent rhythm.

**Content section:**
```xml
<mj-section padding="24px 32px 0 32px" background-color="#FFFFFF" css-class="email-card">
  <mj-column>
    <mj-text
      css-class="muted-text"
      font-size="11px" line-height="16px" font-weight="600"
      color="#98A2AF" letter-spacing="0.08em"
      padding="0 0 6px 0">
      FEATURE LABEL
    </mj-text>
    <mj-text
      css-class="heading-h2 body-text"
      font-size="22px" line-height="30px" font-weight="600"
      color="#171717" padding="0 0 6px 0">
      Card headline
    </mj-text>
    <mj-text
      css-class="body-text"
      font-size="16px" line-height="25px" color="#424242"
      padding="0 0 20px 0">
      Body copy. Single paragraph.
    </mj-text>
    <mj-image
      src="CHEVRON_URL"
      alt="Go to Feature"
      width="32px" height="32px"
      align="left"
      href="https://www.surfline.com/"
      target="_blank"
      padding="0 0 32px 0" />
  </mj-column>
</mj-section>
```

**Multi-paragraph cards:** Use separate `mj-text` blocks per paragraph. Use `padding="0 0 12px 0"` on all paragraphs except the last before the chevron, which uses `padding="0 0 20px 0"`.

**BETA badge:** Inline span within the label `mj-text`:
```html
<span style="display:inline-block;background:#DDE4F1;color:#424242;font-size:9px;font-weight:600;padding:2px 6px;border-radius:4px;letter-spacing:0.05em;vertical-align:middle;margin-left:4px;">BETA</span>
```

---

### Divider

Separates cards within the same section group. Always inset 32px.

```xml
<mj-section padding="0 32px" background-color="#FFFFFF" css-class="email-card">
  <mj-column>
    <mj-divider />
  </mj-column>
</mj-section>
```

For dividers in the Other Updates section, use `background-color="#EEF4FB"`.

---

### Chevron CTA Button

Not an `mj-button` — rendered as a 32×32px clickable image, left-aligned, below body copy.

```xml
<mj-image
  src="https://braze-images.com/appboy/communication/assets/image_assets/images/6a18d458256bf70083ce8b2b/original.png?1780012119"
  alt="Go to [Feature]"
  width="32px"
  height="32px"
  align="left"
  href="DESTINATION_URL"
  target="_blank"
  padding="0 0 32px 0" />
```

The 32px bottom padding creates the gap between the chevron and the divider (or section end) below it.

---

### Primary CTA Button

Full-width pill button. Appears once at the bottom of the email. Only element that uses a distinct destination URL separate from `surfline.com/`.

```xml
<mj-section padding="20px 32px 36px 32px" background-color="#EEF4FB" css-class="email-card">
  <mj-column>
    <mj-button
      href="https://www.surfline.com/lp/whatsnew/home"
      target="_blank"
      background-color="#3A70E0"
      color="#FFFFFF"
      font-family='Linear Sans, Arial, Helvetica, sans-serif'
      font-size="16px"
      font-weight="600"
      line-height="20px"
      inner-padding="14px 32px"
      border-radius="99px"
      align="center">
      View all updates
    </mj-button>
  </mj-column>
</mj-section>
```

---

### Marketing Footer Block

Standalone compiled footer, appended in Braze as a repeatable component (`marketing-footer-block.html` in `Outputs/`). Injected via the Liquid tag `{{content_blocks.${Marketing_Footer_v062026}}}` (see **Footer & Liquid Injection**). Structure, top to bottom: social icons row (Instagram, TikTok, YouTube, Facebook), legal / unsubscribe text, app store buttons, PARTNERS label, partner logos.

| Element | Value |
|---|---|
| Background | `#121212` |
| Text | `#ffffff` |
| Social icons | 30×30px, 40px gap, centered |
| Social icons padding | 48px top / 0 bottom |
| Legal section padding | 24px top / 60px bottom |
| App store badge | 166×51px, width + height fixed in both attributes and inline style (prevents squish) |
| PARTNERS label | 11px / 600 / line-height 16px / letter-spacing 0.08em |
| Address | 2261 Market St, Suite 10852, San Francisco, 94114, USA |

Partner logo sizes (ratios preserved): WSL 68×20, El Salvador 96×23, Sun Bum 23×33, Vans 56×19, JS 88×17.

The unsubscribe block is self-contained in this footer, unlike the What's New template where unsub is appended separately by Braze. Social icons are Braze-CDN hosted; app store badges and partner logos are Chamaileon-CDN hosted — confirm those remain reachable before a send.

---

## Spacing System

| Context | Value |
|---|---|
| Wrapper lateral gutters | `0 24px` |
| Card internal inset (left/right) | `32px` |
| Section header top padding | `28px` |
| Section header bottom padding | `20px` |
| Label to headline gap | `10px` (header), `6px` (card) |
| Headline to body gap | `6px` |
| Hero image top gap (below header) | `16px` |
| Between image and content section | `24px` |
| Between body paragraphs | `12px` |
| Last paragraph to chevron | `20px` |
| Chevron bottom (to divider/section end) | `32px` |
| Primary CTA top padding | `20px` |
| Primary CTA bottom padding | `36px` |

---

## Link Strategy

| Element | URL |
|---|---|
| Logo | `https://www.surfline.com/` |
| Hero image | `https://www.surfline.com/` |
| All feature card images | `https://www.surfline.com/` |
| All chevron buttons | `https://www.surfline.com/` (feature-specific URLs added per campaign) |
| Primary CTA button | Campaign-specific (e.g. `https://www.surfline.com/lp/whatsnew/home`) |

All links use `target="_blank"`. Inline text links (e.g. `mailto:`) use `style="color:#3A70E0;text-decoration:none;"`.

---

## Footer & Liquid Injection

Braze content-block Liquid tags (the marketing footer, and any others such as Highlight Cam Matches) must land **inside the outer wrapper `<td>`** that `mj-wrapper` compiles to — after the last content section's closing `</div>` and its MSO comment, before the outer wrapper table closes. Use `mj-raw` **inside** the `mj-wrapper` to inject them. `mj-raw` at body level ejects the tag outside the wrapper and breaks the layout.

Correct tail:

```html
        <!--[if mso | IE]></td></tr></table></td></tr><![endif]-->

        <!-- ── HIGHLIGHT CAM MATCHES ── -->
        {{content_blocks.${Highlight_Cam_Matches_2}}}

        <!-- ── MARKETING FOOTER ── -->
        {{content_blocks.${Marketing_Footer_v062026}}}
        <!--[if mso | IE]></table><![endif]-->
       </td>
      </tr>
     </tbody>
    </table>
   </div>
```

`UK-Ireland-Summer-Outlook.html` is the confirmed reference for correct placement.

| Mistake | Symptom |
|---|---|
| Tags placed after the article `</div>` (or `</body></html>`) | Footer renders at the wrong width; the black footer background is narrower than the email |
| Tags inside a content section's `<tbody>` | Footer inherits that section's background color and max-width |

**Liquid renders raw outside Braze.** Tags such as `{{${set_user_to_unsubscribed_url}}}` and `{{ "now" | date: "%Y" }}` show as literal text in any non-Braze preview. Strip or ignore them when eyeballing locally.

**Local-preview black bar is a non-issue.** The footer block is a full standalone HTML document (its own `<!DOCTYPE>` / `<html>` / `<body>`). Previewed locally with the Liquid tag left as literal text, the browser sees the outer email left unclosed after the footer's early `</body></html>`, producing a black bar below the footer. Braze strips the standalone wrapper on injection, so this does not occur in Braze.

---

## Known Issues & Decisions

**font-family attribute quoting:** Do not use escaped inner quotes (`'"Linear Sans"'`) in MJML attribute values. MJML injects them literally into `style=""` HTML attributes, breaking rendering. Use `font-family='Linear Sans, Arial, Helvetica, sans-serif'` (no inner quotes). Multi-word font names without quotes still resolve correctly in CSS.

**SVG images:** Email clients do not support SVG in `<img>` tags. All icon assets must be PNG or hosted as PNG via CDN. If converting from SVG locally, use `sharp` to rasterize at 2× the display size for retina screens.

**Base64 images:** Avoid embedding images as base64 data URIs in production. Many email security filters block or strip them. Use hosted CDN URLs. Base64 is acceptable only for local testing.

**rgba backgrounds on buttons:** Outlook does not support `rgba` background-color on buttons. Use a hex fallback.

**`border-radius` on sections:** MJML compiles section `border-radius` into VML for Outlook. Use sparingly — this template uses no rounded corners on sections, only on images (`border-radius="12px"`) and the pill button (`border-radius="99px"`).

**Unsubscribe block:** Intentionally omitted from this template. It is managed as a separate repeatable component and appended in Braze/the sending platform.

**Output formatting:** Always deliver prettified HTML. MJML's `--config.minify=true` is required for correct mobile column behavior but produces single-line output. Run the prettify script before handing off.

**Wrapper gutters / full-bleed variant:** The What's New template uses `mj-wrapper` lateral gutters (`padding="0 24px 32px 24px"`), giving 552px cards over the 600px body. Single-feature forecaster emails (UK/Ireland, Vicco) drop the lateral gutters (`padding="0"`) for a full-bleed card, keeping the inner `32px` section inset. Leftover hardcoded widths carried over from a gutter layout cause side gaps — recompute.

**Content width formula:** usable content width = `600 − 2×wrapper_gutter − 2×card_inset`. Gutter layout: 600 − 48 − 64 = 488px. Full-bleed: 600 − 0 − 64 = 536px. Set the computed px on the image `<td>` and `width:100%` on the `<img>` for fluid scaling. On mobile, zero the wrapper's lateral padding via a `@media (max-width:479px)` override to prevent side gaps.

**Asset hosting:** prefer Braze CDN for all production assets. Some legacy footer assets sit on Chamaileon CDN — confirm reachable before a send. Figma exports and other signed / temporary URLs are placeholders only; host on Braze CDN before production.
