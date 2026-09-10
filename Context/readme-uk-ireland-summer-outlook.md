# README: UK & Ireland Summer Outlook Email — Build Notes
**File:** `UK-Ireland-Summer-Outlook-v23062026-v4.html`
**Date:** June 23, 2026
**Purpose:** One-off forecaster content email built on the What's New template backbone.

---

## Overview

This email was built to promote a UK & Ireland Summer Outlook article. It consolidates all content into a single block — no multi-feature layout — using the What's New template structure with MJML compiled to HTML.

---

## Final Structure (top to bottom)

| Section | Details |
|---|---|
| Banner | Dark (`#121212`) Surfline logo bar, centered |
| Section Header | Muted label + H1 headline |
| Hero Image | Full-width surfer photo, `border-radius:12px`, links to article |
| Body Copy + CTA | Paragraph text + pill button ("Check it out"), links to article |
| Marketing Footer | Braze Liquid tag: `{{content_blocks.${Marketing_Footer_v062026}}}` |

---

## Key Decisions

- **No What's New H1 header** — removed for this send; label + H1 live in the section header block instead
- **Single image** — surfer photo chosen over the beach/van photo
- **White background** (`#FFFFFF`) throughout content card
- **Section label:** `UK & IRELAND SUMMER OUTLOOK`
- **H1:** `El Niño's arrived, record-heat wave already, but what about the waves?`
- **Body copy ends at:** `...It's been a season of extremes already, and it's just begun.` (last sentence trimmed from source)
- **CTA:** Pill button, label "Check it out", links to article URL

---

## Spacing Notes

- Header top padding: `28px`
- Image top padding: `24px` (matched to body copy top padding for visual consistency)
- Body copy bottom padding: `20px`
- CTA bottom padding: `64px` (extra breathing room before footer)

---

## Build Issues & Fixes

### 1. Outer wrapper margin causing inset on all sides
**Problem:** `mj-wrapper padding="0 24px 32px 24px"` was shrinking the card to 552px, creating visible gaps on left/right edges on both desktop and mobile.
**Fix:** Removed lateral padding from wrapper (`padding="0"`). Inner `32px` horizontal padding on each `mj-section` was left intact as the intentional content inset.

### 2. Hero image not filling available width
**Problem:** Image `<td>` had a hardcoded `width:488px` — a leftover from the previous wrapper padding calculation (600 − 24 − 24 − 32 − 32 = 488).
**Fix:** Updated image width to `536px` (600 − 32 − 32 = 536) with `width:100%` on the `<img>` tag for fluid scaling.

---

## File Versions

| Version | Notes |
|---|---|
| `v23062026` | Initial build |
| `v23062026-v2` | Body copy trimmed; hero image top padding 16px→24px; CTA bottom padding 32px→64px |
| `v23062026-v3` | Removed outer wrapper lateral padding (full-width fix) |
| `v23062026-v4` | Fixed hero image hardcoded width to fluid 536px |
