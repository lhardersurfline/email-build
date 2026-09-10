# _Instructions.md — Surfline Email Build workflow

Authoritative working rules for this project. The Claude project instructions
point here. Read the two files in `Context/` first, then read this file, at the
start of every session.

**Repo (source of truth):** https://github.com/lhardersurfline/email-build

---

## How files are accessed

Two supported setups, same repo and same layout:

1. **Local clone + Filesystem MCP** — files live in a local clone. The default path is `C:\Users\Leif Harder\.claude\email-build`. Claude probes that path and confirms it is reachable; if it is not (a different machine, another teammate, or a moved folder), Claude asks for the clone path before reading or writing. Claude reads and writes via the Filesystem MCP. The teammate runs git; Claude lists changed files and drafts a commit message.
2. **GitHub MCP connector** — Claude reads and writes the repo directly, committing to a branch (then PR) or to `main`, per the teammate's preference.

If it is unclear which setup is in use, ask.

## Session start

1. Read both files in `Context/`: `native-design-system-email-ref.md` and `surfline-email-template-context.md`. The second holds component specs, tokens, the footer Liquid tag, and the known fixes folded in from prior session readmes.
2. Read this file and follow it.
3. Run the build brief below before generating anything. For an edit to an existing `Outputs/` file, the brief collapses to "which file, and what change" — the Context read still happens.

## Build brief (ask before building)

1. **Base** — new email, or build off an existing template? (List the current `Outputs/` files as options.)
2. **Type** — editorial, promo, transactional, or other.
3. **Copy** — headline, body, CTA label, and subject + preview text. Never invent copy; use exactly what is supplied. Generate copy only when explicitly asked.
4. **Assets** — hero and any graphics, plus where they are hosted (Braze CDN URL, or a Figma node to pull). If the teammate has raw asset files and the Braze MCP is connected, offer to upload them to the Braze media library and use the returned CDN URL. Figma exports and other signed/temporary URLs are placeholders — they must be Braze-hosted before production.
5. **Links / URLs** — CTA destinations, plus UTMs if any.
6. **Share as template?** — "Once finalized, do you want to share this as a reusable template?" Store the answer; it drives the finalizing step below.

## While building

- Show the rendered HTML preview **and** the HTML code in chat on each iteration, so it can be eyeballed and pasted into an external previewer.
- Follow the component specs, tokens, spacing, and known fixes in `Context/surfline-email-template-context.md`.

## Finalizing

Destination follows intent, and Claude proposes it rather than asking open:

- **Shared template = yes** → promote to `Outputs/` with a stable filename, then run the commit flow.
- **Shared template = no** → deliver the preview and code; save to `Drafts/` if the teammate wants it kept.
- A design token, readme, or other reference doc → `Context/`.

**Commit flow**, by setup:

- **Local clone** — Claude lists the changed files and drafts a commit message. The teammate commits and pushes.
- **GitHub connector** — Claude commits to a branch and opens a PR, or commits to `main`, per preference.

## Folder architecture

- `Context/` — inputs: design system reference and the template/component context. Read first.
- `Outputs/` — the live set: current templates and reference emails.
- `Drafts/` — work in progress; promote to `Outputs/` when finalized.

## Versioning

Versioning lives in git, so nothing is lost by replacing a file.

- Keep stable filenames (e.g. `WhatsNew_Template.html`). Commit history tracks changes; write clear commit messages.
- Add a date suffix only where Braze needs a discrete named file for a specific send, not for iterations of a reusable template.
- Never force-push or rewrite published history. Deletions are recoverable through git, so retiring a file by deleting it is fine.

## Build pipeline

MJML source → compile → BeautifulSoup prettify → placeholder / footer injection → final inline-table HTML. Use the `email-html-mjml` skill. Tokens, component specs, the footer Liquid tag, and known fixes live in `Context/surfline-email-template-context.md`.
