---
layout: default
title: "Week 0 — Mac"
---

# Week 0 — Setup & Fundamentals (Mac)

> Before you write any real code, you need a working machine, a working git habit, and a rough map of what "backend" even means. That's this week. Grind through the setup, do the first-PR exercise at the end, and read the primer. Then you're ready for week one.

This guide is for **macOS**. If you're on Windows, use the Windows version instead — the commands are different.

---

## The mental model

Here's the good news: your Mac is already a Unix machine underneath. The **Terminal** app is a real, professional development environment — the same kind of environment Rails runs on in production. There's no extra layer to install, nothing to emulate. Everything you set up runs natively.

```
macOS (Terminal)        ← where ALL your dev work happens
  ├── Homebrew          ← the tool that installs everything else
  ├── Ruby + Rails
  ├── PostgreSQL (the database)
  └── git
VS Code                 ← your editor, opens the project directly
```

When this guide says "run this," it means **in the Terminal app**. Open it now: press `Cmd + Space`, type "Terminal," hit Enter. Keep it open — you'll live in it this week.

---

## Part 1 — Install Homebrew

Homebrew is the package manager for macOS — the tool you'll use to install almost everything else. Installing it also pulls in Apple's Command Line Tools, which include `git`.

In the Terminal:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

This takes a few minutes and may ask for your Mac password (you won't see characters as you type — that's normal).

When it finishes, it prints a couple of `Next steps` commands to add Homebrew to your PATH. **On Apple Silicon Macs** (M1/M2/M3/M4), run these:

```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

> On older **Intel** Macs, Homebrew installs to a different location and is usually already on your PATH — just follow whatever the installer printed at the end. When this guide later references `/opt/homebrew/...`, an Intel Mac uses `/usr/local/...` instead.

Check it worked:

```bash
brew --version
```

---

## Part 2 — Install the libraries Ruby needs

```bash
brew install openssl@3 libyaml gmp
```

These are the libraries Ruby compiles against. `git` is already installed from Part 1.

---

## Part 3 — Install Ruby and Node with mise

You won't install Ruby directly. You'll use a **version manager** called `mise` so you can match the exact Ruby version a project needs.

```bash
brew install mise
```

Activate it in your shell (macOS uses **zsh** by default, so this goes in `~/.zshrc`):

```bash
echo 'eval "$(mise activate zsh)"' >> ~/.zshrc
source ~/.zshrc
```

Now install Ruby and Node globally:

```bash
mise use --global ruby@3.4
mise use --global node@22
```

Check it worked:

```bash
ruby --version
node --version
```

> Later, when you clone Neta, it has a `.ruby-version` file. Running `mise install` inside the project folder installs the exact version the project expects. Always match the project's version — that's a real-world habit.

---

## Part 4 — Install PostgreSQL

PostgreSQL is the database Neta uses.

```bash
brew install postgresql@16
```

Add its tools to your PATH (this is what lets the `pg` gem and `psql` work):

```bash
echo 'export PATH="/opt/homebrew/opt/postgresql@16/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

Start it:

```bash
brew services start postgresql@16
```

That's it. Unlike some setups, `brew services` keeps PostgreSQL running and **restarts it automatically when you log in** — you don't have to start it by hand every session. Homebrew also creates a database user matching your Mac username, so Rails can connect locally without a password.

---

## Part 5 — Install VS Code

1. Download VS Code from https://code.visualstudio.com/ — it comes as a `.zip`. Open it, then drag **Visual Studio Code** into your **Applications** folder.

2. Open VS Code. Press `Cmd + Shift + P` to open the command palette, type **"Shell Command"**, and choose **"Install 'code' command in PATH."**

3. Now you can open any project from the Terminal. Try it later with:
   ```bash
   cd ~
   code .
   ```

Because macOS is already Unix, VS Code edits and runs your project directly — no extra extensions needed to get started.

---

## Part 6 — Set up git and GitHub

You already have `git` (it came with the Command Line Tools in Part 1). Configure who you are:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Use the same email as your GitHub account.

### Connect to GitHub with an SSH key

This lets you push code without typing a password every time.

1. Generate a key (press Enter at every prompt to accept defaults):
   ```bash
   ssh-keygen -t ed25519 -C "you@example.com"
   ```

2. Copy the public key straight to your clipboard:
   ```bash
   pbcopy < ~/.ssh/id_ed25519.pub
   ```

3. On GitHub: click your avatar → **Settings** → **SSH and GPG keys** → **New SSH key**. Paste (`Cmd + V`), give it a name like "MacBook," and save.

4. Test it:
   ```bash
   ssh -T git@github.com
   ```
   If it greets you by your username, you're connected.

---

## Part 7 — Get Neta running

Your coach will give you:
- The URL of **your fork** of Neta to clone
- The **`RAILS_MASTER_KEY`** and any API keys you need

Then:

```bash
cd ~
git clone git@github.com:YOUR-USERNAME/neta.git
cd neta
mise install          # installs the exact Ruby/Node the project needs
bundle install        # installs the project's gems
```

Set up the master key your coach gave you (this unlocks the app's secrets):

```bash
echo "PASTE_THE_KEY_HERE" > config/master.key
```

PostgreSQL is already running from Part 4, so create and seed the database:

```bash
bin/rails db:setup
```

Start the app:

```bash
bin/dev
```

Open your browser to **http://localhost:3000**. You should see Neta. If you do — you have a real Rails app running on your machine. That's the finish line for setup.

---

## Part 8 — Copy the project board and link your fork

All the work you'll do this summer lives as tickets on a GitHub Project board. Your coach has a master board with every ticket on it. You're going to make your **own copy** so your progress stays yours, separate from the other students'.

### Copy the master board

1. Open your coach's master GitHub Project (they'll give you the link).
2. In the **top-right corner**, click the **`...`** menu.
3. Click **Make a copy**.
4. In the dialog, check the box **"Draft issues will be copied if selected"** — this is what brings all the tickets along. If you skip it, you get an empty board.
5. Under **Owner**, select your **personal GitHub account**.
6. Give it a name (e.g. "Neta — [your name]") and click **Copy project**.

You now have your own board with every ticket on it, as draft issues.

### Turn a ticket into a real issue when you start it

The tickets arrive as **draft issues** — notes that live only on the board. When you're ready to actually work on one, you convert it into a real issue in *your fork*, so it's trackable and you can link your pull request to it.

1. Click the ticket to open it.
2. Click the **`...`** menu on the item and choose **Convert to issue**.
3. When it asks which repository, choose **your fork of Neta**.

That's it — the draft becomes a real issue in your repository. Do this one ticket at a time, as you pick up work. Don't convert everything at once; convert a ticket when you start it. That way your board always shows, at a glance, what's still a plan versus what you're actually building.

---

## The first-PR exercise

The whole program runs on pull requests, so let's do the entire git loop once, with zero risk, before any real work.

1. Make a new branch:
   ```bash
   git checkout -b add-my-name
   ```

2. Open the project in VS Code (`code .`), find the `CONTRIBUTORS.md` file (create it if it doesn't exist), and add a line with your name.

3. Stage, commit, and push:
   ```bash
   git add CONTRIBUTORS.md
   git commit -m "Add my name to contributors"
   git push -u origin add-my-name
   ```

4. Go to your fork on GitHub. It'll show a banner offering to **open a pull request**. Open it. Write a one-line description.

5. Tell your coach. They'll review it, maybe leave a comment, and merge it.

That's the loop you'll repeat all summer: branch → change → commit → push → PR → review → merge. Do it once now while the stakes are zero.

---

## Backend in five minutes

You know JavaScript and the browser. Here's the part you haven't seen yet, in rough strokes.

**Frontend vs backend.** The frontend is what runs in the browser — the HTML, CSS, and JavaScript you already know. The backend is a program running on a *server* that the browser talks to. Neta's backend is the Rails app. When you load a page, your browser sends a request to the server, the server does work (looks things up, runs logic), and sends back a response.

**The request/response cycle.** This is the heartbeat of every web app. Browser asks ("GET me the homepage"), server answers (here's the HTML). Browser asks ("POST this form"), server processes it and answers. Everything is a request and a response.

**What a database is.** Instead of storing information in files, web apps store it in a database — organized into tables of rows and columns, like a spreadsheet with rules. PostgreSQL is Neta's database. When Neta remembers a recommendation you got, it's saving a row in a table.

**What a framework does.** Rails is a framework — it's the structure and tooling that handles the repetitive parts of building a web app: receiving requests, talking to the database, sending back responses. You write the parts that make *your* app specific; Rails handles the plumbing.

**MVC, very briefly.** Rails organizes code into three roles. **Models** talk to the database (a `Venue`, a `Recommendation`). **Views** are what gets rendered and sent back (the HTML). **Controllers** sit in the middle — they receive the request, ask the models for data, and pick a view to respond with. You'll see all three this week when you trace how Neta builds a recommendation.

**Running a server locally.** `bin/dev` starts the Rails server on your own machine. `localhost:3000` means "this computer, port 3000" — a private copy of the app only you can see, for development. Nothing is on the internet yet.

Don't try to memorize this. It's a map so you recognize the territory when you walk into it next week.

---

## Quick troubleshooting

**`brew: command not found`.**
Homebrew isn't on your PATH. Run the `eval "$(/opt/homebrew/bin/brew shellenv)"` line from Part 1 (Intel Macs: `/usr/local/bin/brew`).

**`bundle install` fails on the `pg` gem.**
PostgreSQL's tools aren't on your PATH. Re-run the `export PATH` line in Part 4, then `source ~/.zshrc`, and try again.

**`code: command not found`.**
Open VS Code, press `Cmd + Shift + P`, and run "Install 'code' command in PATH."

**A command wants you to use `sudo` for gems, or says "permission denied."**
Don't `sudo` Ruby or gem commands. If that's happening, mise isn't active — open a new Terminal tab, or re-run the activate step in Part 3.

**The app can't connect to the database.**
Make sure Postgres is running: `brew services start postgresql@16`.

**Git or `xcode-select` errors about missing tools.**
Run `xcode-select --install` and accept the prompt.

---

## You're done with Week 0 when:

- [ ] Terminal works and Homebrew is installed (`brew --version`)
- [ ] `ruby --version` and `node --version` both work
- [ ] PostgreSQL is running via `brew services`
- [ ] VS Code opens from the Terminal with `code .`
- [ ] Your SSH key is on GitHub and `ssh -T git@github.com` greets you
- [ ] Neta runs at localhost:3000
- [ ] You copied the project board to your own account (with draft issues)
- [ ] You opened and merged your first pull request
- [ ] You read the backend primer

Hit all of those and you're ready for week one. Anything you couldn't get past after a real attempt — bring it to the first sync.
