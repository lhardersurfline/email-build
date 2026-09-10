# _Instructions.md — Surfline Email Build workflow

Authoritative working rules for this project. The Claude project instructions
point here; read this at the start of every session, after reading `Context/`.

**Repo (source of truth):** https://github.com/lhardersurfline/email-build

---

## How files are accessed

Two supported setups, same repo and same layout:

1. **Local clone + Filesystem MCP** — files live in a local clone of the repo. Claude reads and writes via the Filesystem MCP. The teammate runs git (commit, push, pull); Claude does not run git, but can list changed files and draft commit messages on request.
2. **GitHub MCP connector** — Claude reads and writes the repo directly through the connector, committing to a branch (then PR) or to `main`.

If it's unclear which setup is in use, ask. For a local clone, if the clone path isn't known, ask for it.

## Session steps

1. Read every file in `Context/` before building — design tokens, Braze config, and template readmes.
2. Ask clarifying questions before starting work.
3. While iterating on an output, show the HTML preview and the HTML code in chat, so it can be eyeballed and pasted into an external previewer.
4. When a request is done, confirm which folder the output belongs in (`Context/`, `Drafts/`, or `Outputs/`) before writing.

## Folder architecture

- `Context/` — inputs: style guides, design tokens, Braze config, template/session readmes.
- `Outputs/` — the live set: current templates and reference emails.
- `Drafts/` — work in progress; promote to `Outputs/` when finalized.

## Versioning

Versioning lives in git, so nothing is lost by replacing a file.

- Keep stable filenames (e.g. `WhatsNew_Template.html`, `native-design-system-email-ref.md`). Commit history tracks changes; write clear commit messages.
- Add a date suffix only where Braze needs a discrete named file for a specific send (e.g. a dated campaign), not for iterations of a reusable template.
- Never force-push or rewrite published history. Deletions are recoverable through git, so retiring a file by deleting it is fine.
- Prior dated versions from the pre-git project are retained in git history (seed commit "First copy of Leif's local to the repo").

## Build pipeline

MJML source → compile → BeautifulSoup prettify → placeholder / footer injection → final inline-table HTML. Use the `email-html-mjml` skill. Design tokens, component specs, and the footer Liquid tag live in `Context/`.
