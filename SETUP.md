# Setup — Surfline Email Build

How to use Claude to build Surfline marketing emails. Everyone does two quick
setup steps first, then picks how Claude gets the latest templates.

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

### Prefer working from files on your computer? (maintainers)
Keeps a linked copy of the shared folder on your machine.

1. Install **Git** once, then download a linked copy of the shared folder to your computer. Claude can give you the exact line to run.
2. In **Claude Desktop**, turn on the **Filesystem** connector and allow the folder you downloaded into.
3. Start a chat in the shared project. Claude reads the templates straight from that folder.

---

## Saving your email back for everyone

When your email is finished, Claude asks whether to make it a shared template.
Say yes and it saves the change. How that reaches the shared folder depends on
your setup:

- **If you connected GitHub (the connector):** Claude does it for you. It asks whether to add your change straight into the live templates, or set it aside for someone to review first. Pick one and Claude handles the rest. No commands.
- **If you work from a copy on your computer:** Claude writes the file, then gives you a few lines of text to publish it. You don't need to understand them. Open your terminal (on Windows that's **Git Bash**; on Mac, **Terminal**) in the project folder, paste the lines Claude gave you, and press Enter. That sends your update to the shared folder so teammates get it.

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
- Maintaining the templates for everyone → **work from a copy on your computer**, or use the **GitHub connector** to save changes back.
- Bringing your own images → add the **Braze** step.
