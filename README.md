# OceanHackWeek 2026 — Intro to Git & GitHub

An introduction to git and GitHub. Everyone works in this repository at the same time.
We're going to make a few merge conflicts happen on purpose and practice resolving them, so you already know what to do when they show up later in the week.

> Adapted from the [OHW22 NE intro activity](https://github.com/oceanhackweek/ohw22-NE-intro-activity)
> by Catherine Mitchell, itself heavily borrowed from Alex Kerney's
> [OHW21 exercise](https://github.com/oceanhackweek/ohw21-intro-activity).

**Before we start, you should have:**
- Logged in to CryoCloud (`hub.cryointhecloud.com`) and opened a **Terminal**
  (Launcher → Other → Terminal)
- Working git authentication — see **[SETUP.md](SETUP.md)**.

## Resources

Background reading from the OHW site. These are general references. For how to
log in to our hub (CryoCloud) and set up git auth, use [SETUP.md](SETUP.md),
which is current for OHW26.

- [Git](https://oceanhackweek.github.io/resources/prep/git.html) — installing git
  and the basic workflow. (It walks through a fork workflow; today we use a
  shared-repo + branch approach.)
- [GitHub](https://oceanhackweek.github.io/resources/prep/github.html) — why
  GitHub, organizations, and teams.
- [JupyterHub](https://oceanhackweek.github.io/resources/prep/jupyterhub.html) —
  what Jupyter and JupyterHub are. Note: the login steps there describe a
  previous year's hub, not CryoCloud — see SETUP.md for the current login.

---

## The four places your code lives

Before any commands, here's the whole mental model. Every command today just
moves your work between these four places:

```
   working dir  --add-->  staging  --commit-->  local repo  --push-->  GitHub
   (files you            (marked             (saved on           (shared with
    edit)                 to save)            the hub)            everyone)

                    <----------------- pull -----------------
```

`git status` tells you which box your stuff is in right now. When you're lost,
run `git status`.

---

## Round 0 — open your intro issue

On this repo's **Issues** tab, click **New issue**. Title it
`Introduce YOUR NAME`. Paste this checklist into the body and submit:

```markdown
- [ ] Open this issue
- [ ] Clone the repo to CryoCloud
- [ ] Make a branch
- [ ] Add my introduction file
- [ ] Commit it
- [ ] Push my branch
- [ ] Open a pull request
- [ ] Review a neighbour's pull request (say hi!)
- [ ] Merge my pull request
- [ ] Round 2: sign the roll call and survive the conflict
- [ ] Close this issue
```

> Watch the issue for a moment after you open it — a welcome comment should
> appear. That's a **GitHub Action** running automatically. More on that at the
> end.

Tick boxes as you go by editing the issue (or just click the checkboxes).

---

## Round 1 — your own file (this should go smoothly)

Because everyone edits a *different* file this round, nothing collides.

**1. Clone the repo** (once). On the green **Code** button, copy the HTTPS URL, then:

```bash
git clone https://github.com/oceanhackweek/ohw26-intro-activity.git
cd ohw26-intro-activity
```

**2. Make a branch** so your work is separate from everyone else's:

```bash
git switch -c intro-YOURHANDLE
```

**3. Create your intro file** in the `introductions/` folder, named for your
GitHub handle, e.g. `introductions/sorochak.md`. Two ways, pick whichever you like:

- **File browser (what we'll demo):** in the left panel, double-click into the
  `introductions` folder *first*, then right-click → New File to make your file there. 
  Double-click it to open an editor tab, type, and save with **Cmd/Ctrl-S**. 
  Making it *inside* the `introductions` folder matters — that's where git expects it.

- **Terminal:** `nano introductions/YOURHANDLE.md` (save/exit: Ctrl-O, Enter,
  Ctrl-X).

Write a couple of lines: your name, where you're from, your institution, and
your favourite dessert.

**4. See what git noticed:**

```bash
git status
```

It lists your new file as untracked (it's in the *working dir* box).

**5. Stage and commit** (move it to *staging*, then to your *local repo*):

```bash
git add introductions/YOURHANDLE.md
git status          # now it's staged
git commit -m "Add intro for YOURHANDLE (#<your issue number>)"
```

Writing `#12` links the commit and PR back to your issue (using your real issue number).

**6. Push your branch to GitHub:**

```bash
git push -u origin intro-YOURHANDLE
```

**7. Open a pull request.** GitHub will show a banner offering to open a PR from
your branch — click it, check the message, and **Create pull request**.

**8. Review a neighbour's PR.** Go to the **Pull requests** tab, open someone
else's, click **Files changed**, and leave a friendly comment. Choose
**Approve**. (Approve / Request changes / Comment are how you signal whether a
PR is ready.)

**9. Merge.** Once you've got an approval, merge your own PR. 🎉

You just did the entire core loop: branch → edit → add → commit → push → PR →
review → merge.

---

## Round 2 — the shared file (this WILL conflict — that's the point)

Now everyone edits the **same lines of the same file** at the same time. Some of
you will merge cleanly; the rest will hit a merge conflict. We'll resolve them
together, and draw the branch/merge picture on the whiteboard as it happens.

**1. Get your local copy up to date.** Main has moved since you cloned (everyone's
Round 1 intros merged in), so pull those changes down first:

```bash
git switch main
git pull origin main
```

**2. Branch again:**

```bash
git switch -c rollcall-YOURHANDLE
```

**3. Open [`roll_call.md`](roll_call.md).** Find the placeholder line directly
under `--- sign below ---`:

`| your-handle | where you're from | one word |`

**Replace that whole line** with your own info, e.g. `| @asorochak | Ontario |
barnacle |`. Everyone edits the *same line* on purpose — that's what forces the
merge conflicts.

**4. Commit and push, same as before:**

```bash
git add roll_call.md
git commit -m "Sign the roll call — YOURHANDLE"
git push -u origin rollcall-YOURHANDLE
```

**5. Open a PR and merge it.** If your PR merges cleanly, great — you were early.
If GitHub says **"This branch has conflicts that must be resolved,"** read the
next section.

---

## Resolving a merge conflict

A conflict just means two people changed the same lines and git needs a human to
decide. Nothing is broken. Do this in your terminal:

```bash
git switch main
git pull origin main        # get everyone's merged changes
git switch rollcall-YOURHANDLE
git merge main              # git now flags the conflict
```

Open `roll_call.md`. You'll see conflict markers:

```
<<<<<<< HEAD
your line
=======
someone else's line
>>>>>>> main
```

Keep **both** rows (delete the `<<<<<<<`, `=======`, `>>>>>>>` marker lines and
arrange the rows how you want). Then:

```bash
git add roll_call.md
git commit                  # finishes the merge
git push
```

Refresh your PR — the conflict is gone and you can merge. That's it. You just did
the thing everyone's scared of.

---

## A quick word on `.gitignore`

Run `git status` after you've had a notebook open. See `.ipynb_checkpoints/`?
That's junk the hub creates that should never go into git. That's what
[`.gitignore`](.gitignore) is for — files listed there are invisible to git.
Take a look at ours.

---

## Bonus: the welcome bot was a GitHub Action

When you opened your intro issue, a comment appeared automatically. That's
GitHub Actions — automation that runs in response to repo events. Ours lives in
[`.github/workflows/welcome.yml`](.github/workflows/welcome.yml) and runs every
time an issue is opened. Actions are how projects run tests, check formatting,
and publish things automatically. Worth exploring for your project repos.

---

## Command cheat sheet

| Command | What it does |
|---|---|
| `git status` | Which box is my stuff in? Run this constantly. |
| `git clone <url>` | Copy a GitHub repo down to the hub |
| `git switch -c <name>` | Make and move to a new branch |
| `git switch <name>` | Move to an existing branch |
| `git add <file>` | Stage a file for the next commit |
| `git commit -m "msg"` | Save staged changes to your local repo |
| `git push -u origin <branch>` | Send your branch up to GitHub |
| `git pull origin main` | Bring GitHub's changes down to you |
| `git merge main` | Merge main into your branch (may conflict) |
| `git log --oneline --graph --all` | See the branch/merge tree |
