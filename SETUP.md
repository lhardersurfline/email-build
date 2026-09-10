# Setup — Surfline Email Build

Getting from zero to building Surfline emails with Claude.

Start at **Level 1**. It needs no accounts, connectors, or git. Move up only if
you want Claude to read the repo directly (Level 2) or push changes back to it
(Level 3).

**Repo:** https://github.com/lhardersurfline/email-build

---

## Level 1 — Build an email, no setup

For anyone who just wants Claude to produce an email from the repo's templates
and standards. No MCPs, no connectors, no git.

1. **Get the files.** On the repo page, click the green **Code** button → **Download ZIP**, then unzip. (Or `git clone` if you already use git.)
2. **Start a Claude Project.** In Claude, create a new Project so the context stays in place across chats. A single chat works too, but you'd re-upload each time.
3. **Add the instructions.** Open `claude-project-instructions.md`, copy the block inside, and paste it into the Project's instructions field.
4. **Upload what Claude needs:**
   - Every file in `Context/` (design tokens, Braze config, template readmes).
   - The one file in `Outputs/` you want to base your email on — a template like `WhatsNew_Template.html`, or a reference email like `Vicco-Incoming.html`.
5. **Ask for your email.** Describe what you want. Claude reads the context, asks any clarifying questions, then returns the HTML preview and the code in the chat.
6. **Use it.** Copy the HTML into your previewer or into Braze.

That's the whole loop. In this mode Claude works only from what you upload; it
doesn't touch the repo. When the templates change upstream, download the ZIP
again.

> The `Context/` files carry the full design system, so Claude can build from
> them alone. The `email-html-mjml` skill (see Level 3) smooths the MJML compile
> step but isn't required to get an email out.

---

## Level 2 — Let Claude read the repo directly

Skips the manual uploads. Pick whichever fits how you work. Either way, paste
`claude-project-instructions.md` into your Project instructions first, same as
Level 1 step 3.

### Option A — Local clone + Filesystem (Claude Desktop)
Best if you keep a copy of the repo on your machine.
1. Clone it: `git clone https://github.com/lhardersurfline/email-build.git`
2. In Claude Desktop, enable the **Filesystem** MCP and allow the folder you cloned into.
3. Start a session and tell Claude the clone path. It reads `Context/` and `Outputs/` directly.

### Option B — GitHub connector
Best if you'd rather not keep a local copy.
1. In Claude, connect the **GitHub** connector and grant it access to `lhardersurfline/email-build`.
2. Start a session and point Claude at the repo. It reads the files remotely.

---

## Level 3 — Push changes back to the repo

Once Claude can read the repo (Level 2), it can also help you change it. How the
change gets committed depends on your setup.

### Local clone + Filesystem
Claude writes the new or updated file into your clone. You commit and push:
```
git add -A
git commit -m "Describe the change"
git push
```
Prefer review? Commit on a branch and open a pull request instead of pushing to `main`.

### GitHub connector
Claude commits through the connector directly. Tell it your preference: a
**branch + pull request** for review, or straight to **`main`** for quick iterations.

### The build skill
The full MJML compile pipeline (MJML → compile → prettify) runs through the
`email-html-mjml` skill plus code execution. It isn't stored in the repo — ask
the maintainer to share it. Without it, Claude still builds from the `Context/`
standards and you can compile and preview in an external MJML tool.

---

## Which level am I?

- Just need an email now → **Level 1**.
- Building often and tired of uploading → **Level 2**.
- Improving the templates or reference files for everyone → **Level 3**.
