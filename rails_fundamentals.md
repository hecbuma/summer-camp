---
layout: default
title: "Rails Fundamentals"
---

# Rails Fundamentals

> You've finished Foundations: you know Ruby, you're at home in the terminal, and you understand what Rails *is* — the Ruby you already know, arranged to answer web requests. Now you make that real. In this lane you'll build a small, working Rails app from nothing in a single sitting, take it apart to see how it fits together, and then make your first real change to Neta. This is the bridge from "I get the idea" to "my hands have done it."

Same rhythm as before: small steps, run it, see it work. Go at your own pace.

---

## A note on the AI, again

You did all of Foundations by hand, on purpose, and now you can actually *read* code. That was the whole point. From here you can start letting AI tools help you look things up and explain things — but keep the same deal: it's there to help you understand, never to understand *for* you. If you ever catch yourself pasting in code you can't explain, that's the signal to slow down and read it. In this lane especially, the goal is that you can point at any file and say what it's for.

---

## Resources — where to look things up

You don't have to hold all of Rails in your head. Nobody does. Here's where the real answers live, and they're worth bookmarking:

- **[rubyonrails.org](https://rubyonrails.org/)** — the official home of Rails. Start here for the big picture.
- **[Rails Guides](https://guides.rubyonrails.org/)** — the official handbook, written to be read. The **[Getting Started guide](https://guides.rubyonrails.org/getting_started.html)** walks through building a blog from scratch and is the single best companion to this lane. When something here goes quickly, that guide goes slowly and explains why.
- **[Rails API docs](https://api.rubyonrails.org/)** — the reference for every method Rails gives you. Denser; you grow into it.

And two videos worth watching — the classic and its modern successor:

- **[How to build a blog in 15 minutes with Rails](https://www.youtube.com/watch?v=Gzj723LkRJY)** — DHH, the creator of Rails, demoing it in 2005. This video is a big part of why Rails took off. It's *old* — the Rails in it is ancient and you shouldn't copy the exact commands — but watch it for the feeling: one person building a working web app astonishingly fast. That feeling is the thing you're about to have yourself.
- **[Rails 8: The Demo](https://www.youtube.com/watch?v=X_Hw9P1iZfQ)** — DHH again, on the official Rails channel, twenty years later — the same spirit of the classic demo, but on Rails 8, the version you're actually using. Watch this one *after* the original: the idea is identical, the tooling has just moved on. The commands and screens will look much closer to what you're doing in this lane.

---

## Module 1 — Build a working Rails app

You're going to build a tiny app that lets you add, list, edit, and delete places — a stripped-down cousin of what Neta does. And Rails is going to write most of it for you, so you can see the whole shape at once.

First, make sure you have Rails itself:

```bash
gem install rails
```

Now create a fresh app and move into it:

```bash
rails new place_book
cd place_book
```

`rails new` just made you a complete web application — dozens of files, all wired together. Take a breath and don't worry about most of them yet.

Now the magic line. This tells Rails to build everything needed to manage "places," where each place has a **name** and a **note**:

```bash
bin/rails generate scaffold Place name:string note:text
```

That one command wrote a model, a controller, a set of pages, and the instructions to create a database table. Run those instructions to actually build the table:

```bash
bin/rails db:migrate
```

Start the app:

```bash
bin/rails server
```

Open your browser to **http://localhost:3000/places**. You can add a place, see it in a list, click into it, edit it, delete it. That's a real, working web application — create, read, update, delete, the four things almost every app does — and you built it in about two minutes.

Sit with that for a second. Then add a few places, edit one, delete one. When you're done, go back to the terminal and press `Ctrl + C` to stop the server.

> **Honest note:** scaffolding is training wheels. It generates everything at once so you can see the whole picture fast. Real apps like Neta usually build these pieces by hand, one deliberate decision at a time — which is why Neta's code looks different from what you just made. That's expected. Right now you *want* the whole picture fast; later you'll build it slowly.

---

## Module 2 — Take it apart

You built an app without seeing how. Now let's look inside, because every piece maps to something you already learned in Foundations. Open the `place_book` folder in your editor and find these.

**The routes** — `config/routes.rb`. You'll see one line: `resources :places`. That single line created all the web addresses your app answers: the list page, the "new place" page, the edit page, and so on. Routes are the front door — they decide which request goes where.

**The model** — `app/models/place.rb`. Look how small it is: `class Place < ApplicationRecord`. That's it. Remember your `Dog` class from Foundations, with data and behavior? This is the same idea — a `Place` is an object that holds data (a name, a note) and knows how to save itself to the database. Rails gives it all that ability from that one line.

**The migration** — `db/migrate/..._create_places.rb`. This is the instruction sheet that built your table. Read it: it says, make a table for places, with a `name` and a `note`. When you ran `db:migrate`, this is what ran.

**The controller** — `app/controllers/places_controller.rb`. This is a class full of **methods** — `index`, `show`, `new`, `create`, `edit`, `update`, `destroy`. Each method handles one kind of request. When you clicked "New Place," the `new` method ran. When you saved, `create` ran. You wrote methods in Foundations; these are methods doing the real job of a web app.

**The views** — the `app/views/places/` folder. These are the actual pages sent back to your browser — the list, the detail page, the form. This is the *response* in the request-and-response cycle you saw with your little web server.

Trace one full trip in your head: you visit `/places` → the **route** sends it to the controller's `index` **method** → that method asks the **model** for all the places → it hands them to the **view** → the view becomes the page you see. Route, controller, model, view. That's Rails. Every app in this style works this way, Neta included.

**You can now:** point at any of the five core pieces of a Rails app and say what it does, and trace a request from the address bar to the page.

---

## Module 3 — Add a feature yourself

So far Rails built the app *for* you and you read what it made. Now you change it — by hand. You're going to give each place a **rating** from 1 to 5. It's a small feature, but it touches almost every layer you just explored, and it's the first time you're adding something to a Rails app on your own. This is the real skill.

The trick for each step: **find how `name` or `note` already does it, and copy that pattern for `rating`.** You practiced reading this code in Module 2 — now you use it.

**1. Add a place to store the rating.** Right now the database has columns for `name` and `note`, but nothing for a rating. You add one with a migration — the same kind of instruction sheet the scaffold made, except this time you're writing it:

```bash
bin/rails generate migration AddRatingToPlaces rating:integer
bin/rails db:migrate
```

Open the file it created under `db/migrate/` and read it — it says "add a `rating` column, holding a whole number, to the places table." Running `db:migrate` made that change real.

**2. Let the form accept the rating.** Open `app/controllers/places_controller.rb` and find the `place_params` method near the bottom. It lists the fields the form is allowed to send — right now `:name` and `:note`. Add `:rating` to that list. (Rails makes you say explicitly which fields are allowed. It's a safety feature — a form can't sneak in fields you didn't approve.)

**3. Add a rating box to the form.** Open `app/views/places/_form.html.erb`. Find the little block that shows the `note` field. Copy its shape for a rating — a label and a number box:

```erb
<div>
  <%= form.label :rating %>
  <%= form.number_field :rating %>
</div>
```

**4. Show the rating.** Now find where a place's `note` gets displayed (look in `app/views/places/` — likely `_place.html.erb` or `show.html.erb`). Mirror it to show the rating too, something like:

```erb
<p>
  <strong>Rating:</strong>
  <%= place.rating %>
</p>
```

Match whatever pattern the file already uses for `note` — that's why you read the code first.

**5. Try it.** Start the server again (`bin/rails server`), go to `http://localhost:3000/places`, and edit one of your places. There's your rating box. Give it a 5, save, and see it on the page. You just added a feature to a web app — the database, the form, and the display, all wired together by you.

> **The Neta connection:** this is exactly the kind of thing Neta does, just fancier. Neta scores places too — but its scores are computed by an LLM reading reviews, not typed in by hand. You just built the toy version of Neta's whole reason for existing. When you open Neta next, that idea will already be familiar.

**Stretch (optional):** open `app/models/place.rb` and add a rule that a rating has to be between 1 and 5:

```ruby
validates :rating, inclusion: { in: 1..5 }, allow_nil: true
```

Now try saving a rating of 9 — Rails will stop you and show an error. You just wrote your first validation, the same way real apps protect their data.

**You can now:** add a new field to a Rails app end to end — migration, controller, form, and display — by reading the existing code and following its pattern.

---

## Module 4 — Meet the real codebase

Now you're ready to open Neta — not to understand all of it (nobody does at first), but to find your way around and make one small, real change. And here's the payoff: it has the exact same folders you just explored in your own app. `models`, `controllers`, `views`, `routes`. You've seen these before. Neta's are bigger and hand-written, but the shape is familiar.

**Scavenger hunt.** With your coach, open the Neta project in your editor. Your mission: find where a specific piece of text on the page comes from. Pick something you saw on the screen — a heading, a button label — and hunt for it in the code. Your editor can search the whole project (`Cmd/Ctrl + Shift + F`). When you find the file that contains that text, you've found the **view** that builds part of the page. Poke around the folders and notice how they line up with your `place_book` app.

**Your first real change.** Your coach will pick one tiny, safe thing for you to change — a bit of wording on the page, most likely. You'll:

1. Make a branch for your change.
2. Find the text (you just practiced this) and edit it.
3. Save, refresh the page, and see *your* words on the screen.
4. Commit it, push it, and open a pull request — the same way everyone on the team ships their work.

Your coach will walk you through the git steps the first time. When your change is merged, it's official: you edited a real, running application that other people use. That's the moment you've been building toward.

**You can now:** find your way around a real codebase, make a small change, see it live, and ship it like everyone else.

---

## Where this goes next

From never having written a line of code, you've reached the point of shipping a change to a real Rails app — with a working app of your own built along the way. That's a serious summer, and it's the exact base the rest of the team started from.

Next time around, you're ready to start picking up real backlog tickets from the main program, slowly and with review. For now, enjoy the fact that you can open a Rails app and actually know what you're looking at.

---
