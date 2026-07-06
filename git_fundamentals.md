---
layout: default
title: "Git Fundamentals"
---

# Git Fundamentals

> Git is how you save your work, try things without fear, and share code with other people. It confuses almost everyone at first — that is completely normal, so don't worry if it feels strange. This guide gives you just the mental model and the handful of commands you'll actually use every day. You already ran this whole loop once in your Week 0 first pull request; this is the explanation of what was happening.

---

## The mental model

A few words, and then it clicks:

- A **repository** ("repo") is just a project folder that Git is watching for changes.
- A **commit** is a saved snapshot of your work — like a save point in a game. You can always come back to one.
- A **branch** is a parallel line of work. You make a branch to try something *without* touching the main version. When your change is good, it gets merged in.
- **`main`** is the primary branch — the official version everyone shares.
- **GitHub** is a copy of your repo that lives online, so you and your team can share commits. Your computer has one copy; GitHub has another. You **push** to send your work up, and **pull** to bring other people's work down.

Here's the picture: think of the project as a shared document. **Commits** are the versions you save as you go. A **branch** is your own draft copy, so you can experiment without messing up the shared one. **Push** and **pull** keep your copy and the online copy in sync.

---

## The daily loop

This is the rhythm you repeat for every change you make. It's always the same shape:

**1. Start from the latest main.**
```bash
git checkout main
git pull
```

**2. Make a branch for your change.**
```bash
git checkout -b my-change
```

**3. Do your work in the editor. Then see what changed.**
```bash
git status
```

**4. Stage the changes you want to save.**
```bash
git add .
```

**5. Commit them, with a message describing what you did.**
```bash
git commit -m "Add a friendly welcome message"
```

**6. Push your branch up to GitHub.**
```bash
git push -u origin my-change
```

**7. Open a pull request on GitHub.** Your coach reviews it and merges it.

**8. Once it's merged, go back to main and pull the latest.**
```bash
git checkout main
git pull
```

That's the whole thing: branch → change → add → commit → push → PR → merge → pull. Everything else in Git is details you'll pick up only when you actually need them.

---

## A few commands that save you

- **`git status`** — "where am I, and what's changed?" Run it constantly. It's the single most useful command in Git, and it often tells you exactly what to do next.
- **`git log --oneline`** — the list of commits, most recent first.
- **`git diff`** — shows exactly what you've changed since your last commit.
- **`git branch`** — lists your branches and shows which one you're currently on.

---

## You will mess up, and that's fine

Everyone gets tangled in Git sometimes — including people who've used it for fifteen years. Here's the reassuring part: because every commit is a saved snapshot, it's genuinely hard to *lose* work. When something looks wrong, your first move is always `git status` — it usually explains what's going on and even suggests what to do. And when you're truly stuck, ask your coach. Getting untangled is a normal part of the job, not a sign you're doing it wrong.

---

## Where this fits

You'll use this loop for real in **Rails Fundamentals**, when you make your first change to Neta and open a pull request. Come back to this page any time the commands start to blur together — that muscle memory takes a few rounds to build, and then it's yours for good.
