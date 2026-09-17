# Surfline Email Build

Source, templates, and reference for Surfline marketing emails: MJML compiled to
inline-table HTML, deployed via Braze. This repo replaces the local
`Claude vEmailBuild` project. Version history lives in git, so nothing is
overwritten or lost.

**Repo:** https://github.com/lhardersurfline/email-build

## Structure

- `Context/` — inputs: design tokens, component specs, template reference. **Read these first.**
- `Outputs/` — current templates and reference emails (the live set).
- `Drafts/` — work in progress.

Prior dated versions from the pre-git project are retained in git history.

## Three ways to connect Claude to this repo

Everyone uses one of three paths. The path decides how Claude reads the
templates and whether it can save work back.

| | Path | Claude reads via | Saves back to repo? | Extra requirement |
|---|---|---|---|---|
| **1** | **Upload** | Files you attach in the chat | No | None |
| **2** | **GitHub Integration** | Repo link, no upload needed | No | Claude in Chrome extension (see below) |
| **3** | **Local clone** | Filesystem MCP on a cloned folder | Yes — full git write | Claude Code or Filesystem MCP setup |

Any path can optionally add the **Braze connector**, which lets Claude upload
your image files to the Braze media library and drop the returned CDN URLs
straight into the build.

Full step-by-step for each path is in [`SETUP.md`](./SETUP.md).

### Known issue on Path 2

The GitHub Integration connects fine and the repo link can be added to a chat or
project, but Claude currently **fails to read the repo through that link on its
own — even though the repo is public**. The working workaround is to enable the
**Claude in Chrome** extension, which lets Claude open and read the link. Until
that's fixed, treat Claude in Chrome as a requirement for Path 2, not an option.

## How we work

The full, authoritative workflow is in [`_Instructions.md`](./_Instructions.md). In short:

1. Read everything in `Context/` before starting a build.
2. Iterate with the HTML preview **and** the code shown in chat.
3. Confirm the destination folder before saving an output.
4. Keep stable filenames; git commits track versions. Add a date suffix only where Braze needs a discrete named file.
5. Never force-push or rewrite history; deletions are recoverable through git.

New teammate? See [`SETUP.md`](./SETUP.md) to get connected.

## Build pipeline

MJML source → compile → BeautifulSoup prettify → placeholder / footer injection → final HTML. Standards, design tokens, and the footer Liquid tag live in `Context/`. Generation uses the `email-html-mjml` skill.
