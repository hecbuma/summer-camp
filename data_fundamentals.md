---
layout: default
title: "Data Fundamentals"
---

# Data Fundamentals

> Here's the thing that's probably been bugging you: you type a movie into a form, hit save, and it… goes somewhere. It comes back when you reload the page. But where *is* it? The database has been an invisible box in the middle of your app this whole time. This lane opens the box. By the end you'll be able to see your data, talk to it directly, and connect pieces of it together — which is most of what makes a real app's code make sense.

Work on your `watchlist` app from the last lane.

---

## The one sentence that makes databases click

You already know this idea. You built it in Foundations:

```ruby
movies = [
  { title: "Arrival", rating: 5 },
  { title: "Dune", rating: 4 }
]
```

An array of hashes. A list of things, each with labeled values.

**A database table is an array of hashes that survives when the program stops.**

That's genuinely it. Your `movies` table is a list, each movie is an entry, and each entry has labeled values — `title`, `notes`, `rating`. The only real difference from your Foundations code is that when you close the program and come back tomorrow, it's still there.

Some vocabulary, so the words stop being scary:

- A **table** is the list — `movies`.
- A **row** (or **record**) is one entry in it — one movie.
- A **column** is one of the labels every entry has — `title`, `rating`.
- The **schema** is the description of what columns exist. That's what migrations change.
- An **id** is a unique number Rails gives every row automatically, so you can always point at exactly one.

---

## Module 1 — Look your data in the eye

Time to actually see it. Rails has a tool that's about to become your favorite thing: the **console**. It's `irb` from Foundations, except your whole app is loaded into it.

In your `watchlist` folder:

```bash
bin/rails console
```

Now type this and press Enter:

```ruby
Movie.all
```

There they are. The movies you typed into that form, sitting in your terminal as real objects. Not a page, not a form — the actual data.

Try these one at a time and watch what comes back:

```ruby
Movie.count            # how many movies do I have?
Movie.first            # the first one
Movie.last             # the most recent one
Movie.first.title      # just the title of the first one
```

That last one is the important one. `Movie.first` gives you a movie **object** — like your `Dog` from Foundations — and `.title` asks it for its data.

**Now create a movie without using the form:**

```ruby
Movie.create(title: "Spirited Away", rating: 5)
```

Leave the console open, go to `http://localhost:3000/movies` in your browser, and refresh. It's there. You just added data to your app by typing a line of Ruby — no form, no clicking. That's what the form was doing all along; you just did it directly.

**Find things:**

```ruby
Movie.where(rating: 5)              # all my 5-star movies
Movie.where("rating >= 4")          # 4 stars and up
Movie.order(rating: :desc)          # best first
Movie.find_by(title: "Dune")        # the one called Dune
```

**Change something:**

```ruby
movie = Movie.find_by(title: "Dune")
movie.rating = 3
movie.save
```

Refresh the browser. Changed.

Type `exit` to leave the console when you're done.

> **This is the tool.** Any time you're not sure what your app is doing, open the console and ask it. Professional Rails developers live in here — it's the fastest way to check whether something worked, poke at real data, or test an idea.

**You can now:** open the Rails console, see your real data, and create, find, and change records with Ruby.

---

## Module 2 — Where the controller gets its data

Now connect what you just did to the app itself, because it's the same thing.

Open `app/controllers/movies_controller.rb` and look at the `index` method. You'll see something very close to:

```ruby
@movies = Movie.all
```

That's the *exact* line you just typed in the console. The controller asks the model for the data, puts it in `@movies`, and hands it to the view. Then in `app/views/movies/index.html.erb`, the view loops over `@movies` — the same kind of loop you wrote in Foundations when you looped over an array of hashes.

Look at the `show` method too:

```ruby
@movie = Movie.find(params[:id])
```

`params[:id]` is the id from the web address. When you visit `/movies/3`, `params[:id]` is `3`, and this line fetches that exact movie. That's how a page knows which movie to show you.

**Try it:** change `index` to sort by rating, best first:

```ruby
@movies = Movie.order(rating: :desc)
```

Refresh the movies page. Sorted. You just changed what your app shows by changing one line — the same line you were experimenting with in the console a minute ago.

**You can now:** read a controller and know exactly where its data comes from, because you can run the same query yourself.

---

## Module 3 — Connecting data together

Here's the leap. Real apps don't have one lonely table — they have several, connected. Neta has venues, and recommendations, and feedback, all linked. Let's do the small version.

Right now a movie has one rating. But you might watch a movie more than once, and think something different each time. So: **a movie has many viewings.** Each viewing belongs to one movie, and records when you watched it and what you thought.

Generate it:

```bash
bin/rails generate model Viewing movie:references watched_on:date reaction:text
bin/rails db:migrate
```

That `movie:references` part is the important bit — it tells Rails this table connects to the movies table. Open the migration it created and you'll see it added a `movie_id` column. **That's how the connection works:** each viewing stores the id of the movie it belongs to. Nothing magic — just a number pointing at a row.

Now tell the models about each other. Open `app/models/viewing.rb` — Rails already added `belongs_to :movie`. Then open `app/models/movie.rb` and add the other side:

```ruby
has_many :viewings
```

Now go to the console and watch what those two lines bought you:

```bash
bin/rails console
```

```ruby
movie = Movie.first
movie.viewings                                    # empty for now

movie.viewings.create(watched_on: Date.today, reaction: "Even better the second time")
movie.viewings.create(watched_on: Date.new(2025, 12, 1), reaction: "First watch, loved it")

movie.viewings           # both of them
movie.viewings.count     # 2
```

And from the other direction:

```ruby
viewing = Viewing.first
viewing.movie            # the movie it belongs to
viewing.movie.title
```

Read that last line slowly: from a viewing, you can reach its movie, and ask that movie for its title. **That's what `has_many` and `belongs_to` do — they let you walk between connected pieces of data.** When you open Neta and see a recommendation that knows its venue, this is exactly that.

**Show the viewings on the page.** Open `app/views/movies/show.html.erb` and add a section listing them:

```erb
<h2>Viewings</h2>
<ul>
  <% @movie.viewings.each do |viewing| %>
    <li><%= viewing.watched_on %> — <%= viewing.reaction %></li>
  <% end %>
</ul>
```

That's a loop over a collection — the thing you learned in Foundations — except the collection comes from the database and lives across two connected tables.

**You can now:** create a second table linked to the first, explain how `movie_id` makes the link, and walk between connected records in both directions.

---

## Where this goes next

The invisible box isn't invisible anymore. You can see your data, query it, change it, and connect it.

Your app works — but let's be honest about how it looks. Default Rails pages are plain. Next up is **Frontend Fundamentals**, where you make the watchlist something you'd actually be happy to show someone.

---


