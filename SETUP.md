# Setup — Surfline Email Build

How to use Claude to build Surfline marketing emails. Everyone does the same two
setup steps, then picks one of three paths depending on how they want Claude to
reach the templates — and whether they need to save work back.

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

Paths 1 and 2 run inside that project (or in a plain chat, with one extra step —
see Path 1). Path 3 runs on your computer instead and doesn't use the project.

---

## Pick your path

| | Path | Best for | Saves back to the repo? |
|---|---|---|---|
| **1** | Upload the files | Just need an email, minimal fuss | No |
| **2** | GitHub Integration | Building often, want it always current | No |
| **3** | Local clone + write access | Updating and adding shared templates | Yes |

---

## Path 1 — Upload the files into your chat

No accounts or connections. You take a copy of the shared folder and hand the
relevant files to Claude.

1. **Download the repo.** On the GitHub page, click the green **Code** button, choose **Download ZIP**, and unzip it.
2. **Attach the files in your chat:**
   - both files from `Context/` (`native-design-system-email-ref.md` and `surfline-email-template-context.md`)
   - the one template from `Outputs/` you want to start from (e.g. `WhatsNew_Template.html`, or a reference email like `Vicco-Incoming.html`)
   - optionally `_Instructions.md` from the repo root
3. **Start the build.**
   - **In the Email Build project:** just say *"let's build an email"* and attach the files. The project instructions already tell Claude what to do.
   - **In a plain chat (no project):** you must also tell Claude to read the instructions — e.g. *"read the attached `_Instructions.md` and `Context/` files, then let's build an email."* Without that, Claude has no workflow loaded.
4. **Answer the brief.** Claude asks for base, type, copy, images, links, and whether to share it as a template, then shows the preview and the code.
5. **Use it.** Copy the HTML into your previewer or into Braze.

When the templates change, download the ZIP again for the latest.

**Saving back:** not possible on this path. Claude only sees what's attached. If
your email should become a shared template, hand the finished `.html` to someone
on Path 3, or add it yourself through the GitHub web UI.

---

## Path 2 — GitHub Integration (no downloading or uploading)

Connect GitHub once and Claude pulls the templates from the repo link itself. No
ZIP, no attachments, always current.

1. **Add the GitHub Integration.** In Claude's settings → Connectors, connect **GitHub** and give it access to `lhardersurfline/email-build`.
2. **Enable the Claude in Chrome extension.** Required today — see the note below.
3. **Add the repo link** to your chat or to the Email Build project: `https://github.com/lhardersurfline/email-build`
4. **Ask for your email.** Claude reads `Context/`, `_Instructions.md`, and the `Outputs/` templates directly from the repo.

### Required: Claude in Chrome

The GitHub Integration connects and the repo link attaches correctly, but Claude
currently **can't read the repo through that link on its own — even though the
repo is public**. Enabling the **Claude in Chrome** extension lets Claude open
and read the link, and that's the workaround until the underlying issue is fixed.

- Install / enable Claude in Chrome, and stay signed in to your Claude account in that browser.
- Use Claude in Chrome for these chats rather than the desktop app.
- **Symptom if it's missing:** Claude says it can't access the repository, returns nothing for the link, or claims the files don't exist. That's this issue, not a broken link or a permissions problem. Enable the extension and retry, or fall back to Path 1 and upload the files.

**Saving back:** treat this path as read-only. Claude delivers the preview and
HTML in chat; getting a finished template into the repo goes through Path 3 or
the GitHub web UI.

---

## Path 3 — Local clone with write access

For anyone regularly updating or adding templates in the shared folder. Claude
works against a real clone on your computer and performs the git write actions —
commit, push, PR — for you.

1. **Install Claude Code.** See https://claude.ai/code for setup for your OS. (Claude Desktop with a Filesystem MCP server pointed at the clone works the same way.)
2. **Clone the repo.** Ask Claude to clone `lhardersurfline/email-build` to your computer, then open a session in that folder. The default clone path in our instructions is `C:\Users\Leif Harder\.claude\email-build`; if yours is elsewhere, tell Claude the path at the start of the session.
3. **Connect the GitHub MCP server** in Claude Code and point it at `lhardersurfline/email-build`. Claude can walk you through this. This is what lets Claude push and open PRs.
4. **Skip the shared project.** Claude Code isn't a claude.ai project, so there's no instructions field. At the start of a session, tell Claude to read `_Instructions.md` at the repo root — it has the full workflow.
5. **Ask for your email or template change.** Claude reads the current files, builds, and commits/pushes (or opens a PR) directly.

**Saving back:** full access. When your email is finished Claude asks whether to
make it a shared template. Say yes and it asks whether to add the change straight
into the live templates on `main`, or set it aside for review as a PR. Pick one
and Claude handles the rest — no commands to paste.

---

## Optional on any path — using your own images

If your images aren't hosted anywhere yet, connect the **Braze** connector
(settings → Connectors). Then, when Claude asks about images in the brief, attach
your image files and Claude uploads them to the Braze media library and drops the
returned CDN links straight into the email build.

This works on all three paths — it's independent of how Claude reads the repo.

Without it, host the images yourself and paste the links into the brief. Images
from Figma or other temporary links expire, so anything going to a real send
should live in Braze.

---

## Example prompts

Two common ways people kick off a build. Copy one, swap in your own details.

### 1. Reusing a template we've already set up

Claude will normally ask you for these inputs one at a time. If you already have
them all, drop them in up front and Claude will just confirm or ask about
anything missing instead of asking from scratch.

> Generate an email for this incoming, leveraging our outlook / incoming templates. Here's some of the inputs, clarify any missing items before building:
> Image URL: https://braze-images.com/appboy/communication/assets/image_assets/images/6aad659660c6eb00888f850f/original.png?1789748629
> CTA link: https://www.surfline.com/surf-news/autumn-equinox-delivers-for-western-regions-of-the-uk-and-ireland/3IlprTV0FbWqM5nsqxJA6A
> H1: Autumn Equinox Delivers for Western Regions of the UK and Ireland
> Body: Summer-like warmth along with settled conditions at last.
> CTA Text: See what's coming
> Overline: INCOMING

### 2. Building a new email from a Figma design

Share the Figma link, attach the images that need to go in the email, and ask
Claude to upload them to the Braze media library so the output uses real CDN
links instead of temporary Figma ones.

> I'd like you to build this email (→ [www.figma.com](https://www.figma.com)) for me. Please use the assets from the attached images. You can upload the images to the Braze media library in the Surfline workspace and use the source code provided for the image urls in your output. Once you've finished building the HTML, go ahead and build the email in Braze as an email template.

---

## Which one am I?

- Just need an email, minimal fuss → **Path 1**, upload the files.
- Building often and want it always current → **Path 2**, GitHub Integration (plus Claude in Chrome).
- Pushing updates and templates back to the repo regularly → **Path 3**, local clone.
- Bringing your own unhosted images → add the **Braze** connector to whichever path you're on.
