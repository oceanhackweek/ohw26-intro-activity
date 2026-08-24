# Setup — read before the tutorial

Everything you type in this session runs on CryoCloud. This page gets you
logged in and able to push to GitHub from the hub. If any step fails during the
session, let us know and a facilitator will help get you unstuck.

## Before you arrive

1. Have a GitHub account.
2. Accept the invitation to the **OceanHackWeek GitHub organization** and to the
   `ohw26-participants` team (check the email you used to sign up for GitHub).
3. **Make your org membership Public.** CryoCloud reads your team membership to
   log you in, and GitHub defaults membership to *Private*. Go to
   `https://github.com/orgs/oceanhackweek/people/<your-handle>` and confirm you
   are **not** set to private. If you're private, login to the hub will fail.

## Logging in to CryoCloud

1. Go to `https://hub.cryointhecloud.com` and sign in with GitHub. The first
   time, authorize **nasa-cryo-prod** (2i2c) when asked — you should see
   `oceanhackweek` with a checkmark on that screen.
2. On the Server Options screen:
   * Environment: choose the OceanHackWeek Python Image.
   * Resource Allocation: ~7 GB RAM is a good general choice. (This tutorial barely 
     uses any memory, so a smaller size works too, but you'll use this same server 
     for the data tutorials later in the week.)
   * Click Start.
3. Wait through the startup spinner (a new node can take a few minutes to come
   up, especially when everyone logs in at once), and you'll land in JupyterLab.
4. Open a Terminal: in the Launcher, under Other, Terminal.

## One-time git setup (in the hub terminal)

```bash
git config --global user.name "Your Name"
git config --global user.email "the-email-you-used-for-github@example.com"
git config --global core.pager cat      # print git output straight to the terminal (no pager to get stuck in)
git config --global pull.rebase false   # on 'git pull', merge remote changes instead of rebasing (avoids a pull error)
```

## Authenticating git

The hub can't reuse your laptop's credentials, so you authenticate here with GitHub's CLI. Run:

```bash
gh auth login
```

Answer the prompts with the arrow keys:

1. **Where do you use GitHub?** → `GitHub.com`
2. **Preferred protocol for Git operations?** → `HTTPS`
3. **Authenticate Git with your GitHub credentials?** → `Yes`
4. **How would you like to authenticate?** → `Login with a web browser`

It shows a **one-time code** (like `XXXX-XXXX`). Copy it, then on **your own
laptop's browser** open `https://github.com/login/device`, paste the code, and
authorize. (Authorizing lists `oceanhackweek` — that's expected.)

> **Heads up — you'll see a red message like**
> `Failed opening a web browser at https://github.com/login/device ... executable
> file not found`. **That is normal.** The hub simply has no browser to open for
> you, which is why you open the URL on your own laptop instead. Authentication
> still completes — look for `✓ Authentication complete` and
> `✓ Logged in as <your-handle>`.

That's it, git can now push from the hub. (Your credentials are stored on this
server, another reason to shut the server down when you're done.)

## Confirm you're authenticated

Check that git is ready to talk to GitHub:

```bash
gh auth status
```

You should see something like `✓ Logged in to github.com account <your-handle>`.
If you do, you're set. We'll clone the activity repo together as the first step
of the tutorial.

## When you're done for the day

Shut your server down so it isn't billing idle compute: **File → Hub Control
Panel → Stop My Server** (or go to `https://hub.crqyointhecloud.com/hub/home`).
Stopping the server does **not** delete anything in your home directory.