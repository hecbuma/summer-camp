---
layout: default
title: "Putting It Together"
---

# Putting It Together

> You've learned Ruby, Rails, data, frontend, and Git — but each one in its own lane. Real work isn't like that. A single feature touches all of them at once, wrapped in the Git workflow, reviewed by another person. This lane is that: you'll build one complete feature on your watchlist the exact way a professional would, and then you'll walk into the real Neta codebase. This is the last stop before the main program.

---

## Module 1 — Ship a feature like a professional

You're going to add **"watched" tracking** to your watchlist: mark a movie as watched, and filter the list to show only what you still need to see. It's a genuinely useful feature, and it touches every layer.

But the feature isn't really the point. **The point is doing the whole loop.** Follow these steps in order, exactly as written — this is the rhythm of every change you'll ever ship.

### 1. Start clean and branch

```bash
git checkout main
git pull
git checkout -b watched-tracking
```

You're now working in your own line. Whatever you do here can't hurt `main`.

### 2. Change the data layer

A movie needs to remember whether you've watched it:

```bash
bin/rails generate migration AddWatchedToMovies watched:boolean
bin/rails db:migrate
```

Open the migration and read it before moving on. A `boolean` holds true or false.

### 3. Check it in the console

Before touching any pages, verify the data layer works:

```bash
bin/rails console
```

```ruby
movie = Movie.first
movie.watched = true
movie.save
Movie.where(watched: true).count
```

This is the habit worth building: **confirm the foundation before building on it.** If the data is wrong, no amount of view code will fix it.

### 4. Let the form accept it

In `app/controllers/movies_controller.rb`, add `:watched` to the permitted list in `movie_params`.

### 5. Add it to the form

In `app/views/movies/_form.html.erb`, following the pattern of the fields already there:

```erb
<div>
  <%= form.label :watched %>
  <%= form.check_box :watched %>
</div>
```

### 6. Show it, styled

In your movie list, show watched movies differently — a badge, faded text, a checkmark, whatever you like. Something like:

```erb
<% if movie.watched? %>
  <span class="text-xs bg-green-100 text-green-800 px-2 py-1 rounded">Watched</span>
<% end %>
```

Notice `movie.watched?` — Rails gives you that question-mark method for free on boolean columns.

### 7. Add the filter

In `app/controllers/movies_controller.rb`, make `index` respond to a filter:

```ruby
def index
  @movies = if params[:filter] == "unwatched"
    Movie.where(watched: false)
  else
    Movie.all
  end
end
```

Then add links to switch between them in `index.html.erb`:

```erb
<div class="flex gap-3 mb-4">
  <%= link_to "All", movies_path, class: "underline" %>
  <%= link_to "Still to watch", movies_path(filter: "unwatched"), class: "underline" %>
</div>
```

Click between them. Notice the web address changes — that's `params` in action, the same `params` that picks which movie to show.

### 8. Commit your work

```bash
git status
git add .
git commit -m "Add watched tracking and unwatched filter"
```

### 9. Push and open a pull request

```bash
git push -u origin watched-tracking
```

Then open the pull request on GitHub. **Write a real description** — what you built, and anything you're unsure about. This matters more than people expect: a good PR description is how you communicate with your team.

### 10. Get reviewed, then merge

Your coach will review it and probably leave comments. **Comments are normal and good** — even senior engineers get them on every PR. Make any requested changes, commit and push again (the PR updates automatically), and when it's approved, merge it.

Then:

```bash
git checkout main
git pull
```

You're back on main, with your feature in it.

**That's the loop.** Branch, build across every layer, verify, commit, push, PR, review, merge, return to main. You just did what professional developers do every single day — on an app you fully understand.

**You can now:** ship a complete feature through the whole professional workflow, touching data, controller, views, styling, and Git.

---

## Module 2 — Meet the real codebase

Now Neta. And here's the thing: it's not going to feel as foreign as you'd expect, because it's your watchlist with more layers.

| Your watchlist | Neta |
|---|---|
| `Movie` — a thing you might watch | `Venue` — a place you might eat |
| `rating` you typed in | `craft_score` an AI computed from reviews |
| `Viewing` — belongs to a movie | `Recommendation` — belongs to a venue |
| Your `movies_controller` | Their controllers, same job |
| Your ERB views with Tailwind | Their ERB views with Tailwind |

Same shapes. More of them, and hand-built rather than scaffolded.

**Scavenger hunt.** With your coach, open Neta in your editor. Find where a specific piece of text on the page comes from — pick a heading or button label you saw, and search the whole project (`Cmd/Ctrl + Shift + F`). When you find it, you've found the **view**. Then find that view's **controller**, and the **model** it gets data from. You've traced a request through a real app.

**Open its console.** Ask your coach to help you start Neta and run `bin/rails console`, then try:

```ruby
Venue.count
Venue.first
```

Same commands, real app, real data. This is why the console matters — it works everywhere.

**Your first real change.** Your coach will pick one small, safe thing — a bit of wording, most likely. Then you run the loop you just practiced: branch, find, edit, check, commit, push, PR. Your coach reviews and merges.

When it lands, it's official: you changed a real, running application that other people use.

**You can now:** find your way around a professional codebase, and ship a change to it.

---

## Where this goes next

Look at what happened this summer. You went from never having written a line of code to shipping a change to a live Rails app — with a movie watchlist you built, styled, and extended along the way. You know Ruby, you know how Rails works, you can see and shape data, you can build a page, and you can use Git the way a team does.

That's the base the rest of the team started from.

There's one more lane if you want it, and it's the best one: **Real Data**, where your watchlist stops holding only what you typed and starts pulling real movie posters and data off the internet — using the exact pattern Neta uses. Then you're ready for real tickets on the main program.

Take a second to appreciate how far that is from module zero.

---