# Session Readme — Marketing Footer Block
**Date:** 08 June 2026
**Output file:** `/Latest Outputs/marketing-footer-block-v08062026-v2.html`

---

## What this file is

A standalone compiled HTML email footer block for use in Surfline marketing emails. Intended to be appended in Braze as a repeatable footer component. It is not an MJML source file — it is compiled output, prettified and ready to paste.

---

## Block structure (top to bottom)

1. **Social icons row** — Instagram, TikTok, YouTube, Facebook. 30×30px icons, 40px gap between each, centered.
2. **Legal / unsubscribe text** — subscription notice, Braze unsubscribe liquid tag, copyright line with Liquid date, address, Terms of Use and Privacy Policy links.
3. **App store buttons** — App Store and Google Play badges, side by side, centered. 166×51px each with fixed dimensions to prevent squishing.
4. **Partners label** — "PARTNERS" in muted-text style (11px / 600 / letter-spacing 0.08em).
5. **Partner logos** — WSL, El Salvador, Sun Bum, Vans, JS. Inline-block, centered, 20px gap between logos.

---

## Design decisions & values

| Element | Value |
|---|---|
| Background | `#121212` |
| Text color | `#ffffff` |
| Font | Linear Sans, Arial, Helvetica, sans-serif |
| Social icon size | 30×30px |
| Social icons top padding | 48px |
| Social icons bottom padding | 0px |
| Legal section top padding | 24px |
| Legal section bottom padding | 60px |
| App store badge size | 166×51px (fixed width + height in both attributes and inline style) |
| Partners label | 11px / 600 / line-height 16px / letter-spacing 0.08em |
| Address | 2261 Market St, Suite 10852, San Francisco, 94114, USA |

### Partner logo sizes (post 20% reduction, ratios preserved)

| Logo | Width | Height |
|---|---|---|
| WSL | 68px | 20px |
| El Salvador | 96px | 23px |
| Sun Bum | 23px | 33px |
| Vans | 56px | 19px |
| JS | 88px | 17px |

---

## Changes made this session

Starting from `marketing-footer-block-v03062026.html`, the following edits were applied across iterations:

1. Social icons reduced from 35px to 30px
2. App store badge dimensions locked with explicit `width`, `height`, and `max-width` in inline style to prevent aspect ratio distortion on certain clients
3. Address updated from Irvine, CA to 2261 Market St, Suite 10852, San Francisco, 94114, USA
4. PARTNERS label reduced from 16px/700 to muted-text style (11px/600/letter-spacing 0.08em)
5. Social icons top padding increased to 48px (was 35px)
6. Legal section top padding reduced to 24px (was 60px, which was creating a large gap below icons)
7. All partner logos reduced by 20% with individual aspect ratios preserved per logo

---

## Known issues / notes

- The Liquid unsubscribe tag `{{${set_user_to_unsubscribed_url}}}` and date tag `{{ "now" | date: "%Y" }}` are Braze syntax and will render as raw text outside of Braze.
- App store badge images are hosted on Chamaileon CDN, not Braze CDN. Confirm these remain accessible before sending.
- Social icon images are hosted on Braze CDN.
- Partner logo images are hosted on Chamaileon CDN.
- Unsubscribe block is intentionally self-contained here (unlike the WhatsNew template where it is appended by Braze separately).

---

## Versioning

| File | Location | Notes |
|---|---|---|
| `marketing-footer-block-v03062026.html` | `/Archive` | Original draft |
| `marketing-footer-block-v08062026.html` | `/Archive` | First iteration 08 Jun |
| `marketing-footer-block-v08062026-v2.html` | `/Latest Outputs` | Current version |
