# Setup — Surfline Email Build

How to use Claude to build Surfline marketing emails. Most people do two quick
setup steps first, then pick how Claude gets the latest templates. If you're
pushing updates and templates back to the repo regularly, skip ahead to
**Use Claude Code** below instead.

> **What's GitHub here?** Just an online shared folder that always holds the
> newest email templates and standards. You don't need to know anything about it
> to build an email. Where it matters below, it's explained in plain terms.

---

## First — two steps everyone does once

**1. Add the email skill.**
This is the Surfline HTML email skill (`email-html-mjml`). It teaches Claude how
to build our emails correctly. It's shared across Surfline, so you add it to your
own Claude one time.

- Open this link: https://claude.ai/customize/skills?selectedId=skill_0186su7DXTY39k3PWsxxKCHr
- The Surfline HTML email skill opens. Add / turn it on. That's it.

**2. Open the shared project and start a chat inside it.**
There is a shared **Surfline Email Build** project in Claude. Open it and start a
new chat there. Don't create your own project — the shared one already has
Claude's instructions loaded, so it knows the workflow from the first message.

---

## Then — choose how Claude gets the templates

### Simplest: upload the files into your chat
No accounts or connections.

1. **Get the files from the shared folder.** On the GitHub page, click the green **Code** button, choose **Download ZIP**, and unzip it.
2. **Attach two things in your chat:** the two files from the `Context` folder (`native-design-system-email-ref.md` and `surfline-email-template-context.md`), and the one template from the `Outputs` folder you want to start from (e.g. `WhatsNew_Template.html`, or a reference email like `Vicco-Incoming.html`).
3. **Describe your email.** Claude runs a short brief (base, type, copy, images, links, and whether to share it as a template), then shows the preview and the code.
4. **Use it.** Copy the HTML into your previewer or into Braze.

When the templates change, download the ZIP again for the latest.

> If the shared project already includes these files (check with your
> maintainer), skip the upload and go straight to describing your email.

### No more uploading: let Claude read the shared folder
Connect GitHub once and Claude always pulls the latest templates itself. No
downloads, always current, and no terminal or commands.

1. In Claude's settings, connect the **GitHub** connector and give it access to the Surfline email folder (`lhardersurfline/email-build`).
2. Start a chat in the shared project and ask for your email. Claude reads the current templates directly.

---

## Pushing updates and templates yourself? Use Claude Code

If you're regularly updating or adding templates in the shared folder, skip the
project-and-copy/paste workflow above and use **Claude Code** instead. It runs
on your computer, reads and writes the repo directly, and commits and pushes
for you — no pasting commands into a terminal.

1. **Install Claude Code.** See https://claude.ai/code for setup for your OS.
2. **Install the GitHub MCP server** in Claude Code and connect it to `lhardersurfline/email-build`. Claude can walk you through this.
3. **Create your local environment** — ask Claude Code to clone the repo to your computer, then open a session in that folder.
4. **Skip the shared project.** You don't need it here — Claude Code isn't a claude.ai project, so there's no instructions field to paste into. At the start of a session, tell Claude to read `_Instructions.md` at the repo root; it has the full workflow (build brief, saving both the `.mjml` and `.html`, commit flow, etc.).
5. **Ask for your email or template change.** Claude reads the current files, builds it, and commits/pushes (or opens a PR) directly — same as the GitHub connector, just running locally instead of in a claude.ai chat.

---

## Saving your email back for everyone

When your email is finished, Claude asks whether to make it a shared template.
Say yes and it saves the change. How that reaches the shared folder depends on
your setup:

- **If you connected GitHub (the connector), or you're using Claude Code:** Claude does it for you. It asks whether to add your change straight into the live templates, or set it aside for someone to review first (a PR). Pick one and Claude handles the rest. No commands to paste.

---

## Optional — using your own images

If your images aren't hosted anywhere yet, connect the **Braze** connector. Then,
when Claude asks about images in the brief, attach your files and Claude uploads
them to the Braze media library and drops the links into the email.

Without it, host the images yourself and paste the links into the brief. Images
from Figma or other temporary links expire, so anything going to a real send
should live in Braze.

---

## Which one am I?

- Just need an email, minimal fuss → **upload the files** (or skip if they're already in the shared project).
- Building often and want it always current → **connect GitHub**.
- Pushing updates and templates back to the repo regularly → **use Claude Code** with the GitHub MCP, or the **GitHub connector** in the shared project.
- Bringing your own images → add the **Braze** step.
