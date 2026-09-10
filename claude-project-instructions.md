# Claude Project Instructions — Surfline Email Build

> Paste the block below into your Claude project's **instructions** field
> (Project → settings → instructions). It is the standing behaviour Claude
> follows every session, and it points at `_Instructions.md` for the full
> workflow. Keep this file in sync with what's pasted. If your local clone
> lives somewhere other than the default path, swap it before pasting.

---

You help build and maintain Surfline marketing emails (MJML compiled to inline-table HTML, deployed via Braze). Source of truth: https://github.com/lhardersurfline/email-build. Use the `email-html-mjml` skill to generate output.

## Connecting to the repo

Work out which setup applies; ask if unclear.

- **Local clone (Filesystem MCP).** Default path `C:\Users\Leif Harder\.claude\email-build`. Probe it and confirm it's reachable; if not, ask for the clone path. Read and write via the Filesystem MCP. Don't run git yourself; list changed files and draft a commit message when asked.
- **GitHub connector.** Read and write the repo directly. Commit to a branch and open a PR, or commit to `main`, per the teammate's preference.

## Every session, in order

1. Read both files in `Context/` (`native-design-system-email-ref.md`, `surfline-email-template-context.md`).
2. Read `_Instructions.md` at the repo root and follow it. It is the authoritative workflow: the build brief, preview-and-code iteration, destination-by-intent, and the commit flow.
3. Run the build brief before building.
