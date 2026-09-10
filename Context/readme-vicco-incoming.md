# README: Vicco Incoming Forecaster Email — Build Notes
**File:** `Vicco-Incoming-v23062026-v4.html`
**Date:** June 23, 2026
**Purpose:** Forecaster incoming email for Victoria surf conditions, built on the UK/Ireland Summer Outlook template structure.

---

## Overview

Single-feature forecaster email promoting a weekend surf forecast article for Victoria's Surf Coast, Mornington Peninsula, and Philip Island.

---

## Final Structure (top to bottom)

| Section | Details |
|---|---|
| Banner | Dark (`#121212`) Surfline logo bar, centered |
| Section Header | `VICTORIA INCOMING` muted label + H1 headline |
| Hero Image | Swell map photo, `border-radius:12px`, links to article |
| Body Copy + CTA | Paragraph text + pill button ("Check the details"), links to article |
| Highlight Cam Matches | Braze Liquid tag: `{{content_blocks.${Highlight_Cam_Matches_2}}}` |
| Marketing Footer | Braze Liquid tag: `{{content_blocks.${Marketing_Footer_v062026}}}` |

---

## Liquid Tag Placement — Critical Notes

### Correct placement
Both Liquid tags must sit **inside the outer wrapper `<td>`**, after the last content section's closing `</div>` and its MSO comment, but before the outer wrapper table closes:

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
   <!--[if mso | IE]></td></tr></table><![endif]-->
  </div>
 </body>
</html>
```

This matches the structure of `UK-Ireland-Summer-Outlook-v23062026-v4.html`, which is the confirmed reference for correct footer placement.

### What goes wrong if placement is incorrect

| Mistake | Symptom |
|---|---|
| Liquid tags placed after `</div></body></html>` (outside the article div) | Footer renders at wrong width; marketing footer black background is narrower than the rest of the email |
| Liquid tags inside a content section's `<tbody>` | Footer inherits that section's background color and max-width constraints |

---

## Black bar in local preview — known non-issue

The marketing footer file (`marketing-footer-block-v08062026-v3.html`) is a full standalone HTML document with its own `<!DOCTYPE>`, `<html>`, `<head>`, `<body>`, and `</body></html>` closing tags.

When previewed locally in a browser with the Liquid tag rendered as literal text (not injected), the browser sees the outer email's structure left unclosed after the footer's `</body></html>` fires early. This causes a black bar to appear below the footer in local preview.

**This does not occur in Braze.** Braze strips the standalone document wrapper when injecting content blocks, serving only the inner HTML. The black bar is a local preview artifact only.

---

## Versioning

| Version | Notes |
|---|---|
| `v23062026` | Initial build — body background `#F5F7FA` |
| `v23062026-v2` | Body background changed to `#FFFFFF` |
| `v23062026-v3` | Liquid tags moved inside outer wrapper `<td>`; outer wrapper restructured to match UK/Ireland pattern |
| `v23062026-v4` | Fixed duplicate MSO closing comment in CTA section; confirmed correct structure |
