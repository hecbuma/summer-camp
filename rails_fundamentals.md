---
layout: default
title: "Rails Fundamentals"
---

# Rails Fundamentals

> You've finished Foundations: you know Ruby, you're at home in the terminal, and you understand what Rails *is* — the Ruby you already know, arranged to answer web requests. Now you make that real. In this lane you'll build a **movie watchlist** — an app for tracking what you want to watch and what you thought of it — from nothing, in a single sitting. Then you'll take it apart to see how it fits together, and add a feature to it by hand.

You'll keep building on this same app through the next few lanes, so it's yours now. Same rhythm as before: small steps, run it, see it work.

---

## A note on the AI, again

You did all of Foundations by hand, on purpose, and now you can actually *read* code. That was the whole point. From here you can start letting AI tools help you look things up and explain things — but keep the same deal: it's there to help you understand, never to understand *for* you. If you ever catch yourself pasting in code you can't explain, that's the signal to slow down and read it.

---

## Resources — where to look things up

You don't have to hold all of Rails in your head. Nobody does. Here's where the real answers live:

- **[rubyonrails.org](https://rubyonrails.org/)** — the official home of Rails.
- **[Rails Guides](https://guides.rubyonrails.org/)** — the official handbook, written to be read. The **[Getting Started guide](https://guides.rubyonrails.org/getting_started.html)** builds an app from scratch and is the best companion to this lane. When something here goes quickly, that guide goes slowly and explains why.
- **[Rails API docs](https://api.rubyonrails.org/)** — the reference for every method Rails gives you. Denser; you grow into it.

And two videos — the classic and its modern successor:

- **[How to build a blog in 15 minutes with Rails](https://www.youtube.com/watch?v=Gzj723LkRJY)** — DHH, the creator of Rails, demoing it in 2005. A big part of why Rails took off. It's *old* — don't copy the commands — but watch it for the feeling: one person building a working web app astonishingly fast.
- **[Rails 8: The Demo](https://www.youtube.com/watch?v=X_Hw9P1iZfQ)** — DHH again, on the official Rails channel, twenty years later, on Rails 8 — the version you're actually using. Watch this one *after* the original. Same idea, modern tooling.

---

## Module 1 — Build your watchlist

You're going to build an app for tracking movies you want to watch. Rails is going to write most of it for you, so you can see the whole shape at once.

First, make sure you have Rails itself:

```bash
gem install rails
```

Now create the app. That `--css=tailwind` part sets up a styling tool you'll use later — the same one Neta uses, so get it in from the start:

```bash
rails new watchlist --css=tailwind
cd watchlist
```

`rails new` just made you a complete web application — dozens of files, all wired together. Take a breath and don't worry about most of them yet.

Now the magic line. This tells Rails to build everything needed to manage movies, where each movie has a **title** and some **notes**:

```bash
bin/rails generate scaffold Movie title:string notes:text
```

That one command wrote a model, a controller, a set of pages, and the instructions to create a database table. Run those instructions to actually build the table:

```bash
bin/rails db:migrate
```

Start the app:

```bash
bin/dev
```

Open your browser to **http://localhost:3000/movies**. Add a movie you want to watch. See it in the list, click into it, edit it, delete it. That's a real, working web application — create, read, update, delete, the four things almost every app does — and you built it in about two minutes.

Add a few movies you actually want to see. You'll be working with this data for a while, so make it yours. When you're done, go back to the terminal and press `Ctrl + C` to stop the server.

> **Honest note:** scaffolding is training wheels. It generates everything at once so you can see the whole picture fast. Real apps like Neta build these pieces by hand, one deliberate decision at a time — which is why Neta's code looks different from what you just made. That's expected. Right now you *want* the whole picture fast; later you'll build it slowly.

---

## Module 2 — Take it apart

You built an app without seeing how. Now look inside, because every piece maps to something you already learned in Foundations. Open the `watchlist` folder in your editor and find these.

**The routes** — `config/routes.rb`. You'll see one line: `resources :movies`. That single line created all the web addresses your app answers — the list page, the "new movie" page, the edit page. Routes are the front door: they decide which request goes where.

**The model** — `app/models/movie.rb`. Look how small it is: `class Movie < ApplicationRecord`. Remember your `Dog` class from Foundations, with data and behavior? Same idea — a `Movie` is an object that holds data (a title, some notes) and knows how to save itself to the database. Rails gives it all that ability from that one line.

**The migration** — `db/migrate/..._create_movies.rb`. This is the instruction sheet that built your table. Read it: make a table for movies, with a `title` and `notes`. When you ran `db:migrate`, this is what ran.

**The controller** — `app/controllers/movies_controller.rb`. This is a class full of **methods** — `index`, `show`, `new`, `create`, `edit`, `update`, `destroy`. Each one handles a kind of request. When you clicked "New Movie," the `new` method ran. When you saved, `create` ran. You wrote methods in Foundations; these are methods doing the real job of a web app.

**The views** — the `app/views/movies/` folder. These are the actual pages sent back to your browser. This is the *response* in the request-and-response cycle you saw with your little web server.

Trace one full trip in your head: you visit `/movies` → the **route** sends it to the controller's `index` **method** → that method asks the **model** for all the movies → it hands them to the **view** → the view becomes the page you see. Route, controller, model, view. That's Rails. Every app in this style works this way, Neta included.

**You can now:** point at any of the five core pieces of a Rails app and say what it does, and trace a request from the address bar to the page.

---

## Module 3 — Add a feature yourself

So far Rails built the app *for* you and you read what it made. Now you change it — by hand. You're going to give each movie a **rating** from 1 to 5. Small feature, but it touches almost every layer you just explored, and it's the first time you're adding something to a Rails app on your own. This is the real skill.

The trick for each step: **find how `title` or `notes` already does it, and copy that pattern for `rating`.** You practiced reading this code in Module 2 — now you use it.

**1. Add a place to store the rating.** The database has columns for `title` and `notes`, but nothing for a rating. You add one with a migration — the same kind of instruction sheet the scaffold made, except this time you're writing it:

```bash
bin/rails generate migration AddRatingToMovies rating:integer
bin/rails db:migrate
```

Open the file it created under `db/migrate/` and read it — it says "add a `rating` column, holding a whole number, to the movies table." Running `db:migrate` made that change real.

**2. Let the form accept the rating.** Open `app/controllers/movies_controller.rb` and find the `movie_params` method near the bottom. It lists the fields the form is allowed to send — right now `:title` and `:notes`. Add `:rating` to that list. (Rails makes you say explicitly which fields are allowed. It's a safety feature — a form can't sneak in fields you didn't approve.)

**3. Add a rating box to the form.** Open `app/views/movies/_form.html.erb`. Find the block that shows the `notes` field. Copy its shape for a rating — a label and a number box:

```erb
<div>
  <%= form.label :rating %>
  <%= form.number_field :rating %>
</div>
```

**4. Show the rating.** Find where a movie's `notes` gets displayed (look in `app/views/movies/` — likely `_movie.html.erb` or `show.html.erb`). Mirror it to show the rating too:

```erb
<p>
  <strong>Rating:</strong>
  <%= movie.rating %>
</p>
```

Match whatever pattern the file already uses for `notes` — that's why you read the code first.

**5. Try it.** Start the server again (`bin/dev`), go to `http://localhost:3000/movies`, and edit one of your movies. There's your rating box. Give it a 5, save, and see it on the page. You just added a feature to a web app — the database, the form, and the display, all wired together by you.

**Stretch (optional):** open `app/models/movie.rb` and add a rule that a rating has to be between 1 and 5:

```ruby
validates :rating, inclusion: { in: 1..5 }, allow_nil: true
```

Now try saving a rating of 9 — Rails will stop you and show an error. You just wrote your first validation, the same way real apps protect their data.

**You can now:** add a new field to a Rails app end to end — migration, controller, form, and display — by reading the existing code and following its pattern.

---

## Where this goes next

You have a working app that you built, understand, and have already extended. But there's still a black box in the middle of it: **the database**. You've been saving movies without ever really seeing where they go.

That's next. In **Data Fundamentals** you'll open the database up, look your data straight in the eye, and learn how real apps connect information together.

---
