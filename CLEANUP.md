# Cleanup plan: stable names + git history

Run this **after** the initial seed commit, so every dated version is captured in
history before the working tree is thinned.

## Approach
1. **Seed commit** — commit all four folders exactly as they are now (dated
   filenames). This puts every historical version into git history.
2. **Cleanup commit** — rename each current file to a stable name and remove the
   now-redundant dated copies. They stay recoverable in history from step 1.

## Outputs → stable names (current live set)
| Current file | Stable name |
|---|---|
| feature-content-block-snippet-v08062026-v2.html | feature-content-block-snippet.html |
| marketing-footer-block-v08062026-v3.html | marketing-footer-block.html |
| Premium-Expiry-v26062026-v2.html | Premium-Expiry.html |
| SoCal-Workweek-Surf-Report-v23062026-v5.html | SoCal-Workweek-Surf-Report.html |
| SouthPacific-Swell-Story-v26062026.html | SouthPacific-Swell-Story.html |
| Summer-LTA-Promo-v30062026-v11.html | Summer-LTA-Promo.html |
| surfline-logo-header-image-v15062026-v2.html | surfline-logo-header-image.html |
| UK-Ireland-Summer-Outlook-v23062026-v4.html | UK-Ireland-Summer-Outlook.html |
| Vicco-Incoming-v23062026-v4.html | Vicco-Incoming.html |
| WhatsNew_Template_v26062026.html | WhatsNew_Template.html |

## Context → stable names
| Current file | Stable name |
|---|---|
| native-design-system-email-ref-v632026.md | native-design-system-email-ref.md |
| readme-marketing-footer-v08062026.md | readme-marketing-footer.md |
| readme-uk-ireland-summer-outlook-v23062026.md | readme-uk-ireland-summer-outlook.md |
| readme-vicco-incoming-v23062026.md | readme-vicco-incoming.md |
| surfline-email-template-context-v632026.md | surfline-email-template-context.md |

## Confirm before running
- **Vicco-Incoming**: current = v4 (the file in `Outputs/`). v5 sits in `Archive/`,
  so v4 is treated as canonical. Correct?
- **WhatsNew**: two different things — `WhatsNew_Template` (the reusable template,
  above) vs `WhatsNew-June26` (a specific dated send in Drafts/Archive). The June
  send keeps its date; only the template gets a stable name. Correct?

## Archive & Drafts
- `Archive/`: captured in history by the seed commit. Afterward, either remove the
  dated copies from the working tree (recoverable via git) or leave them as a frozen
  record — your call.
- `Drafts/`: left as-is; WIP resolves naturally. The `surfline-2mo-offer` `.html` +
  `.mjml` pair stays together.
