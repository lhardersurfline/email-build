# Surfline Email Build

Source, templates, and reference for Surfline marketing emails: MJML compiled to
inline-table HTML, deployed via Braze. This repo replaces the local
`Claude vEmailBuild` project. Version history lives in git, so nothing is
overwritten or lost.

## Structure

- `Context/` — inputs: style guides, design tokens, Braze config, template readmes. **Read these first.**
- `Outputs/` — current templates and reference emails (the live set).
- `Drafts/` — work in progress.
- `Archive/` — legacy versions carried over from the old project. Optional going forward; git history now handles versioning.

## Working rules

1. Read everything in `Context/` before starting a build.
2. While iterating, show the HTML preview **and** the HTML code in chat, so it's easy to eyeball and paste into an external previewer.
3. When an output is done, confirm which folder it belongs in (`Context/`, `Drafts/`, or `Outputs/`), then commit with a clear message.
4. Versioning lives in git. Keep stable filenames (e.g. `WhatsNew_Template.html`) and let commits track changes. Add a date suffix only where Braze needs a discrete named file.
5. Never rewrite history (no force-push). Deletions are recoverable through git, so retiring a file by deleting it is fine; the old `move-to-Archive` step is optional now.

## Build pipeline

MJML source → compile → BeautifulSoup prettify → placeholder / footer injection → final HTML.
Standards, design tokens, and the footer Liquid tag live in `Context/`.

## Working with Claude

With the GitHub connector active, start a session by pointing Claude at this repo.
Claude reads `Context/` and the relevant current files, builds, previews in chat,
then commits to the correct folder. Use a branch + PR when you want review before
merge; commit to `main` for quick iterations.
