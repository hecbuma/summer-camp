---
layout: default
title: "Real Data"
---

# Real Data

> Your watchlist holds movies *you* typed in. Every title, every note, every rating — all by hand. Real apps don't work that way. They pull data from the outside world: other people's databases, live and constantly updating. This lane is that step, and it's the last one before you're fully caught up. You'll add real posters, pull actual movie data from the internet, and build the exact pattern Neta uses to talk to Google. This is the endgame.

Keep working on your `watchlist` app.

---

## Module 1 — A poster, by hand

Start with the visual win. Before any API, just prove your app can show an image.

**Add a column to hold a poster address:**

```bash
bin/rails generate migration AddPosterUrlToMovies poster_url:string
bin/rails db:migrate
```

**Permit it** in `movie_params` in `app/controllers/movies_controller.rb` (add `:poster_url`), and **add a field for it** in `_form.html.erb`, following the pattern of the fields already there:

```erb
<div>
  <%= form.label :poster_url %>
  <%= form.text_field :poster_url %>
</div>
```

**Show it.** In your movie list or show page:

```erb
<% if movie.poster_url.present? %>
  <%= image_tag movie.poster_url, class: "w-32 rounded-lg shadow" %>
<% end %>
```

`image_tag` is a Rails helper that writes an HTML `<img>` tag for you. The `if ... present?` guard matters — without it, movies with no poster would try to load a blank image and look broken.

**Try it:** find any movie poster image online, right-click, copy the image address, and paste it into one of your movies. Refresh.

Suddenly your app looks like a *product*. That's the power of images — and it's also the motivation for everything below, because typing poster URLs by hand for every movie would be miserable. Let's make the computer do it.

**You can now:** store and display an image in a Rails app.

---

## Module 2 — Talk to the internet

Somewhere out there is a server that knows about every movie ever made. You're going to ask it questions.

We'll use the **OMDb API** — a free service that serves IMDb data. (Fun fact: IMDb itself has no free public API, so OMDb is what the community built on top of it. It hands back IMDb *and* Rotten Tomatoes ratings, plus a poster URL.)

**Get your key.** Go to **[omdbapi.com](https://www.omdbapi.com/apikey.aspx)**, request a free key, and check your email to activate it. Free tier is 1,000 requests a day — far more than you'll need.

**Store it safely.** Your API key is a password. It must *never* end up in your code where it could be committed to GitHub. Rails has a secure vault for exactly this:

```bash
EDITOR="code --wait" bin/rails credentials:edit
```

That opens an encrypted file in VS Code. Add your key:

```yaml
omdb_api_key: your_key_here
```

Save and **close the tab** — Rails waits for the editor to close, then re-encrypts the file. Now the key is stored encrypted, and only someone with your app's master key can read it.

> **This is exactly what Neta does.** Remember the `RAILS_MASTER_KEY` your coach gave you in setup? It unlocks Neta's version of this same file, which holds its Google and AI keys. You just learned why that step existed.

**Make your first request.** Open the console (`bin/rails console`) and try:

```ruby
require "net/http"

url = URI("https://www.omdbapi.com/")
url.query = URI.encode_www_form(t: "Arrival", apikey: Rails.application.credentials.omdb_api_key)

response = Net::HTTP.get(url)
puts response
```

That wall of text is **JSON** — the language programs use to send data to each other. It's shaped like a Ruby hash: labels and values. Turn it into one:

```ruby
data = JSON.parse(response)

data["Title"]
data["Year"]
data["Poster"]
data["Plot"]
data["Ratings"]
```

You just pulled real movie data off the internet into your terminal. `data["Poster"]` is a real poster URL — the thing you were pasting by hand a minute ago.

**You can now:** call an external API, parse JSON, and store an API key securely.

---

## Module 3 — Wrap it in a service object

That console code works, but it doesn't belong scattered through your app. Real apps put "talking to an outside service" in its own class — a **service object**. Neta does exactly this with its `PlacesClient`.

Create `app/services/movie_client.rb`:

```ruby
require "net/http"

class MovieClient
  BASE_URL = "https://www.omdbapi.com/"

  def self.find(title)
    new.find(title)
  end

  def find(title)
    response = Net::HTTP.get_response(url_for(title))
    return nil unless response.is_a?(Net::HTTPSuccess)

    data = JSON.parse(response.body)
    return nil if data["Response"] == "False"

    data
  end

  private

  def url_for(title)
    url = URI(BASE_URL)
    url.query = URI.encode_www_form(t: title, apikey: api_key)
    url
  end

  def api_key
    Rails.application.credentials.omdb_api_key
  end
end
```

Read it carefully — you know every piece. It's a **class** with **methods** (Foundations module 5), it uses a **hash** (module 4), and it has an `if` guard (module 3).

Two things worth noticing, because they're what separates toy code from real code:

- **It can fail gracefully.** The internet is unreliable and titles get misspelled. Both `return nil` lines handle that — a bad request gives you `nil` instead of crashing your app.
- **It has one job.** It fetches movie data. It doesn't save anything, doesn't render anything. That's the whole idea of a service object.

**Try it** in the console (restart it first so it picks up the new file):

```ruby
MovieClient.find("The Matrix")
MovieClient.find("asdfghjkl")   # → nil, handled cleanly
```

**You can now:** write a service object that talks to an external API and fails gracefully.

---

## Module 4 — Import a movie for real

Now connect it to the app. Give your model the ability to fill itself in from the API.

In `app/models/movie.rb`:

```ruby
def import_details!
  data = MovieClient.find(title)
  return false if data.nil?

  self.poster_url = data["Poster"] unless data["Poster"] == "N/A"
  self.notes = data["Plot"] if notes.blank?
  save
end
```

Try it in the console:

```ruby
movie = Movie.create(title: "Spirited Away")
movie.import_details!
movie.poster_url
```

Refresh your browser. **The poster is there.** You typed a title; the computer went and found the picture.

**Add a button for it.** In `config/routes.rb`, give the movies resource an extra action:

```ruby
resources :movies do
  member do
    post :import
  end
end
```

In `app/controllers/movies_controller.rb`:

```ruby
def import
  @movie = Movie.find(params[:id])

  if @movie.import_details!
    redirect_to @movie, notice: "Details imported."
  else
    redirect_to @movie, alert: "Couldn't find that movie."
  end
end
```

And on the movie's show page:

```erb
<%= button_to "Import details", import_movie_path(@movie), class: "bg-blue-600 text-white px-4 py-2 rounded" %>
```

Add a movie with just a title, click the button, and watch it fill itself in.

> **Notice what you just built:** data comes from an outside service, and you *save it into your own database*. Next time you load the page, you don't call the API again — you already have it. That's called **caching**, and it's why apps feel fast and don't blow through their API limits. Neta does exactly this: it stores venue scores and skips the Google call if the data is fresh.

**You can now:** pull data from an external service into your own database, through a button in your app.

---

## Module 5 — The hype gap (the finale)

Here's where your watchlist and Neta become the same idea.

That OMDb response has a `Ratings` array with scores from different sources — IMDb, Rotten Tomatoes, Metacritic. **They often disagree.** A movie critics adore might bore audiences. A crowd-pleaser might get panned.

That disagreement is *information*.

Pull both scores in. Add columns for them:

```bash
bin/rails generate migration AddScoresToMovies imdb_score:float critic_score:float
bin/rails db:migrate
```

Then extend `import_details!` to dig them out of the `Ratings` array. Each entry looks like `{"Source" => "Rotten Tomatoes", "Value" => "94%"}`, so you find the one you want and strip the symbols off the value:

```ruby
ratings = data["Ratings"] || []

imdb = ratings.find { |r| r["Source"] == "Internet Movie Database" }
self.imdb_score = imdb["Value"].split("/").first.to_f * 10 if imdb

critics = ratings.find { |r| r["Source"] == "Rotten Tomatoes" }
self.critic_score = critics["Value"].to_f if critics
```

(`find` on an array with a block — that's Ruby you already know. `.to_f` turns `"94%"` into `94.0`.)

Now add the interesting part to `app/models/movie.rb`:

```ruby
def hype_gap
  return nil unless imdb_score && critic_score
  critic_score - imdb_score
end
```

A big positive number means critics loved it far more than audiences did. Negative means the crowd loved it and critics didn't.

Show it on the page, and sort your list by it. Then look at your own watchlist through that lens — you'll spot patterns about your own taste.

**Now read this slowly:** Neta's entire thesis is `hype_score − craft_score`. You just built the same idea with different inputs. Neta uses an AI reading restaurant reviews instead of two rating sites, but the shape — *pull data from outside, compute a gap between how something is perceived and how good it actually is, use it to decide* — is identical.

You didn't just learn Rails. You built a small version of the product you're about to work on.

**You can now:** combine data from an external source into a computed insight — the core pattern behind Neta.

---

## Where this goes next

Look at what your watchlist does now. It stores connected data, looks good, pulls live information from the internet, keeps a local cache, and computes something genuinely useful from it. That's not a toy anymore. That's an app.

And every pattern in it — service objects, external APIs, encrypted credentials, caching, a computed score — is a pattern you'll find in Neta.

You're ready. Head to **[the main program](neta_summer_program.html)** and start picking up real tickets.

---
