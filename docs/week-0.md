# Week 0 — Setup & Fundamentals

> Before you write any real code, you need a working machine, a working git habit, and a rough map of what "backend" even means. That's this week. Grind through the setup, do the first-PR exercise at the end, and read the primer. Then you're ready for week one.

This guide is written for **Windows 11**. You'll set up a Linux environment *inside* Windows and do all your real work there — the same environment professional Rails developers use.

---

## The mental model

Your laptop runs Windows. But Ruby, Rails, the database, and git are all going to live inside **Ubuntu Linux**, running through something called WSL2. Windows is just the host.

Think of it like this:

```
Windows 11               ← the machine you log into
  └── WSL2 (Ubuntu)      ← where ALL your dev work happens
        ├── Ruby + Rails
        ├── PostgreSQL (the database)
        └── git
  └── VS Code            ← runs on Windows, edits files inside Ubuntu
```

You'll write code in VS Code (on the Windows side), but it reads and runs everything inside Ubuntu. Once it's set up, you won't think about the boundary much.

---

## Part 1 — Install WSL2 and Ubuntu

1. Open **PowerShell as Administrator** (right-click the Start button → "Terminal (Admin)" or "Windows PowerShell (Admin)").

2. Run:
   ```powershell
   wsl --install
   ```
   This installs WSL2 and Ubuntu in one step.

3. **Restart your computer** when it tells you to.

4. After restart, Ubuntu opens automatically and asks you to create a **username and password**. This is your Linux user — it's separate from your Windows login. Pick something simple, remember the password (you'll type it for `sudo` commands).

> If Ubuntu doesn't open on its own, find "Ubuntu" in the Start menu and launch it.

From now on, when this guide says "run this," it means **inside the Ubuntu terminal**, not PowerShell — unless it explicitly says PowerShell.

---

## Part 2 — Update Ubuntu and install build tools

In the Ubuntu terminal:

```bash
sudo apt update && sudo apt upgrade -y
```

Then install the libraries Ruby and the Rails gems need to compile:

```bash
sudo apt install -y build-essential git curl \
  libssl-dev libreadline-dev zlib1g-dev libyaml-dev \
  libffi-dev libgdbm-dev libpq-dev pkg-config
```

`libpq-dev` is the one that lets Rails talk to PostgreSQL. If you skip it, installing the app later will fail with a `pg` gem error — now you know why.

---

## Part 3 — Install Ruby and Node with mise

You won't install Ruby directly. You'll use a **version manager** called `mise` so you can match the exact Ruby version a project needs.

```bash
curl https://mise.run | sh
```

Then activate it in your shell (this line tells your terminal to use mise every time it opens):

```bash
echo 'eval "$(~/.local/bin/mise activate bash)"' >> ~/.bashrc
source ~/.bashrc
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
sudo apt install -y postgresql postgresql-contrib
```

Start it:

```bash
sudo service postgresql start
```

**Important:** WSL does not start the database automatically. Every time you open a new terminal session to work, you'll run `sudo service postgresql start` first. If your app suddenly can't connect to the database, this is almost always why.

Now create a database user that matches your Ubuntu username, so Rails can connect without a password locally:

```bash
sudo -u postgres createuser -s $USER
```

That's it — Postgres is ready.

---

## Part 5 — Install VS Code and connect it to WSL

1. On the **Windows side**, download and install VS Code: https://code.visualstudio.com/

2. Open VS Code, go to Extensions (the squares icon on the left), search for **WSL**, and install the extension called "WSL" by Microsoft.

3. Back in your **Ubuntu terminal**, navigate to where your code will live and open VS Code from there:
   ```bash
   cd ~
   code .
   ```
   The first time, it installs a small server inside WSL. After that, VS Code opens connected to Ubuntu. You'll see a green box in the bottom-left corner that says something like `WSL: Ubuntu` — that means it's working.

> **The golden rule:** keep all your project files inside the Linux home folder (`~/`). Never put them in `/mnt/c/...` (the Windows drive). Files on the Windows side are *much* slower to work with. If your app feels sluggish, this is the first thing to check.

---

## Part 6 — Set up git and GitHub

You already have `git` installed (it came with the build tools). Now configure who you are:

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

2. Print the public key and copy it:
   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```

3. On GitHub: click your avatar → **Settings** → **SSH and GPG keys** → **New SSH key**. Paste what you copied, give it a name like "ThinkBook WSL," and save.

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

Make sure Postgres is running, then create and seed the database:

```bash
sudo service postgresql start
bin/rails db:setup
```

Start the app:

```bash
bin/dev
```

Open your browser to **http://localhost:3000**. You should see Neta. If you do — you have a real Rails app running on your machine. That's the finish line for setup.

---

## Part 8 — Copy the project board and link your fork

All the work you'll do this summer lives as tickets on a GitHub Project board. Your coach has a master board with every ticket on it. You're going to make your **own copy** so your progress stays yours, separate from the other student's.

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

> Why this matters: your board and your issues are isolated from the other student's. You move at your own pace, and your coach can look at either board independently to see where each of you is.

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

**The app can't connect to the database.**
Postgres isn't running. Run `sudo service postgresql start`.

**Everything feels slow.**
Your project is probably on the Windows drive (`/mnt/c/...`). Move it into your Linux home (`~/`).

**`bundle install` fails on the `pg` gem.**
`libpq-dev` is missing. Run the install command in Part 2 again.

**A command says "permission denied" or wants you to use `sudo` for gems.**
Don't `sudo` Ruby or gem commands. If that's happening, your Ruby version manager isn't active — close and reopen the terminal, or re-run the mise activation step in Part 3.

**Git push asks for a password.**
Your SSH key isn't set up or wasn't added to GitHub. Redo Part 6.

---

## You're done with Week 0 when:

- [ ] Ubuntu runs and you can open a terminal in it
- [ ] `ruby --version` and `node --version` both work
- [ ] PostgreSQL starts and you created your user
- [ ] VS Code opens connected to WSL (green corner badge)
- [ ] Your SSH key is on GitHub and `ssh -T git@github.com` greets you
- [ ] Neta runs at localhost:3000
- [ ] You copied the project board to your own account (with draft issues)
- [ ] You opened and merged your first pull request
- [ ] You read the backend primer

Hit all of those and you're ready for week one. Anything you couldn't get past after a real attempt — bring it to the first sync.