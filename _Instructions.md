# _Instructions.md — Surfline Email Build workflow

Authoritative working rules for this project. The Claude project instructions
point here. Read the two files in `Context/` first, then read this file, at the
start of every session.

**Repo (source of truth):** https://github.com/lhardersurfline/email-build

---

## Identify the path first

Three supported paths, same repo and same layout. The path decides what Claude
can read, and whether anything can be saved back. Work it out before building;
ask if it's unclear.

### Path 1 — Upload (files attached in the chat)

No filesystem and no GitHub access. The teammate downloaded the repo as a ZIP
(Code → Download ZIP) and attached files directly into the conversation: the two
`Context/` files, plus whichever `Outputs/` template they're starting from, and
possibly `_Instructions.md`.

- Work only from what's attached. Don't claim to browse the rest of the repo.
- If the `Context/` files weren't attached, ask for them before continuing.
- **No write-back.** No commit, push, or PR is possible. Deliver the preview and the HTML code in chat; the teammate pastes it into Braze, or re-attaches it as the starting point next time.
- If they want the change saved as a shared template, tell them to hand the finished `.html` to a Path 3 teammate, or to add it through the GitHub web UI.

**Detection:** files are attached and no Filesystem MCP, GitHub MCP, or readable
repo link is available. Assume Path 1 without asking.

### Path 2 — GitHub Integration (repo link, read-only)

The teammate connected the GitHub Integration to `lhardersurfline/email-build`
and added the repo link to the chat or project. No upload needed; the repo is
read live, so it's always current.

- Read `Context/`, this file, and the `Outputs/` templates from the repo link.
- **Known issue — Claude in Chrome required.** Reading the repo through the link fails on its own, even though the repo is public. The Claude in Chrome extension is the working workaround. If the link can't be read, say so plainly and name the cause: this is the known GitHub Integration reading issue, not a broken link or a permissions problem. Tell them to enable Claude in Chrome and retry, or to fall back to Path 1 and attach the files. Don't guess at template contents or build from memory.
- **Treat as read-only.** Don't promise a commit, push, or PR on this path. Same write-back advice as Path 1: a Path 3 teammate or the GitHub web UI.

**Detection:** a repo link is present in the conversation or project and no local
clone is in play.

### Path 3 — Local clone with write access

A real clone on the teammate's machine, reached through the Filesystem MCP (or
Claude Code working in the repo directory), plus the GitHub MCP for git write
actions. This is the only path that can save work back.

- Default clone path: `C:\Users\Leif Harder\.claude\email-build`. Probe it and confirm it's reachable; if it isn't (a different machine, another teammate, a moved folder), ask for the clone path before reading or writing.
- Read and write files directly.
- **Write-back available:** commit and push to `main`, or commit to a branch and open a PR — per the teammate's preference. Ask which before pushing.

**Detection:** Filesystem MCP or a Claude Code session in the repo directory.

## Optional on every path — Braze MCP for images

Independent of the path above. If the Braze connector is available and the
teammate has raw image files rather than hosted URLs, offer to upload them to the
Braze media library and use the returned CDN URLs directly in the build. Without
it, the teammate supplies hosted URLs themselves. Figma exports and other
signed/temporary URLs are placeholders — they must be Braze-hosted before
production.

---

## Session start

1. Read both files in `Context/`: `native-design-system-email-ref.md` and `surfline-email-template-context.md`. The second holds component specs, tokens, the footer Liquid tag, and the known fixes folded in from prior session readmes. **Path 1:** use the attached copies; if they weren't attached, ask for them. **Path 2:** read them from the repo link.
2. Read this file and follow it.
3. Run the build brief below before generating anything. For an edit to an existing `Outputs/` file, the brief collapses to "which file, and what change" — the Context read still happens.

## Build brief (ask before building)

1. **Base** — new email, or build off an existing template? (List the current `Outputs/` files as options; on Path 1, list what's attached.)
2. **Type** — editorial, promo, transactional, or other.
3. **Copy** — headline, body, CTA label, and subject + preview text. Never invent copy; use exactly what is supplied. Generate copy only when explicitly asked.
4. **Assets** — hero and any graphics, plus where they are hosted (Braze CDN URL, or a Figma node to pull). If the teammate has raw asset files and the Braze MCP is connected, offer the upload described above.
5. **Links / URLs** — CTA destinations, plus UTMs if any.
6. **Share as template?** — "Once finalized, do you want to share this as a reusable template?" Store the answer; it drives the finalizing step below. On Paths 1 and 2, flag up front that saving it back requires a Path 3 teammate or the GitHub web UI.

## While building

- Show the rendered HTML preview **and** the HTML code in chat on each iteration, so it can be eyeballed and pasted into an external previewer.
- Follow the component specs, tokens, spacing, and known fixes in `Context/surfline-email-template-context.md`.

## Finalizing

Destination follows intent, and Claude proposes it rather than asking open:

- **Shared template = yes** → promote to `Outputs/` with a stable filename, then run the commit flow.
- **Shared template = no** → deliver the preview and code; save to `Drafts/` if the teammate wants it kept.
- A design token, readme, or other reference doc → `Context/`.

**HTML only, by default.** Whenever a build is saved to `Outputs/` or `Drafts/`, save only the compiled `.html` — that's what gets pasted into Braze, and it's what this repo tracks. Don't save the `.mjml` source alongside it. Save the `.mjml` only if the teammate explicitly asks for it.

**Commit flow**, by path:

- **Path 1 (Upload)** — no commit flow. Deliver the finished `.html` in chat. There's no route back into the repo from here.
- **Path 2 (GitHub Integration)** — read-only. Same as Path 1: deliver in chat and point at Path 3 or the GitHub web UI for saving back.
- **Path 3 (Local clone)** — write the files, then commit and push to `main`, or commit to a branch and open a PR, per the teammate's preference. Ask which before pushing.

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

MJML source → compile → BeautifulSoup prettify → placeholder / footer injection → final inline-table HTML. Use the `email-html-mjml` skill. Tokens, component specs, the footer Liquid tag, and known fixes live in `Context/surfline-email-template-context.md`. Only the final `.html` is saved to `Outputs/`/`Drafts/` — see Finalizing above.
