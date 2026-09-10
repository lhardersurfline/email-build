# Surfline Email Build

Source, templates, and reference for Surfline marketing emails: MJML compiled to
inline-table HTML, deployed via Braze. This repo replaces the local
`Claude vEmailBuild` project. Version history lives in git, so nothing is
overwritten or lost.

**Repo:** https://github.com/lhardersurfline/email-build

## Structure

- `Context/` — inputs: design tokens, Braze config, template readmes. **Read these first.**
- `Outputs/` — current templates and reference emails (the live set).
- `Drafts/` — work in progress.

Prior dated versions from the pre-git project are retained in git history.

## How we work

The full, authoritative workflow is in [`_Instructions.md`](./_Instructions.md). In short:

1. Read everything in `Context/` before starting a build.
2. Iterate with the HTML preview **and** the code shown in chat.
3. Confirm the destination folder before saving an output.
4. Keep stable filenames; git commits track versions. Add a date suffix only where Braze needs a discrete named file.
5. Never force-push or rewrite history; deletions are recoverable through git.

## Working with Claude

Two setups are supported, both pointing at this repo:

- **Local clone + Filesystem MCP** — Claude reads/writes a local clone; you commit and push.
- **GitHub connector** — Claude reads/commits the repo directly; branch + PR for review, or `main` for quick iterations.

Claude's standing project instructions live in [`claude-project-instructions.md`](./claude-project-instructions.md) — paste that block into your Claude project settings.

New teammate? See [`SETUP.md`](./SETUP.md) to get connected. *(Coming next.)*

## Build pipeline

MJML source → compile → BeautifulSoup prettify → placeholder / footer injection → final HTML. Standards, design tokens, and the footer Liquid tag live in `Context/`. Generation uses the `email-html-mjml` skill.
