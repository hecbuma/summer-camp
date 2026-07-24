---
layout: default
title: "Frontend Fundamentals"
---

# Frontend Fundamentals

> Your watchlist works. It also looks like a tax form. This lane fixes that — but the point isn't just prettiness. Views are where Ruby and HTML meet, and understanding *how* they meet is one of the last big pieces of how a Rails app actually works. By the end you'll have an app you'd be happy to show someone, and you'll understand how every page you've seen so far got built.

You know HTML tags already, so we start from there and go straight to the interesting part.

---

## Module 1 — ERB: Ruby inside HTML

Open `app/views/movies/index.html.erb` and look at the file extension: **`.erb`**. That stands for Embedded Ruby, and it's the answer to a question you might not have thought to ask — *how does a web page show data that changes?*

HTML on its own is static. It says what it says. But your movies page shows different movies depending on what's in your database. ERB is how: it's an HTML file with **Ruby running inside it**.

There are exactly two things to learn, and then you know ERB:

```erb
<%= something %>   <!-- run this Ruby AND print the result onto the page -->
<%  something %>   <!-- run this Ruby but print nothing -->
```

The only difference is the `=`. That's it. That's the whole syntax.

You use the second one for logic that shouldn't appear on the page — loops and conditions:

```erb
<% @movies.each do |movie| %>
  <p><%= movie.title %></p>
<% end %>
```

Read that carefully. The `each` loop is Ruby — the same loop you wrote in Foundations. The `<p>` is HTML. The loop runs once per movie, and each time it prints a paragraph with that movie's title. **This is the moment "Ruby" and "web page" stop being separate things.** The page is built by running Ruby.

**Try it.** In `index.html.erb`, add a line above the list that counts them:

```erb
<p><%= @movies.count %> movies on the list</p>
```

Refresh. The number is real, and it changes when you add a movie.

**Now try a condition:**

```erb
<% if @movies.any? %>
  <p>You have movies to watch.</p>
<% else %>
  <p>Your list is empty. Add something!</p>
<% end %>
```

Delete all your movies to see the second message, then add one back. Same `if` you learned in Foundations, now deciding what a web page says.

**You can now:** read and write ERB, and explain how a page shows data that changes.

---

## Module 2 — Layouts and partials

Two ideas that explain how Rails pages are assembled.

**The layout is the wrapper every page shares.** Open `app/views/layouts/application.html.erb`. This is the frame — the `<html>`, the `<head>`, the stylesheets. Find this line in the middle:

```erb
<%= yield %>
```

That's where each individual page gets dropped in. Your movies page doesn't include `<html>` tags because it doesn't need to — the layout provides them. One wrapper, every page.

**Try it:** add a simple header just above the `<%= yield %>`:

```erb
<header>
  <h1>My Watchlist</h1>
</header>
```

Refresh any page in the app. It's on all of them. That's the layout doing its job.

**A partial is a reusable chunk of a page.** You've already seen one: `_form.html.erb`. The leading underscore means "this is a partial." It exists once, and both the new-movie page and the edit-movie page use it — which is why editing the form in Rails Fundamentals changed both pages at once.

You render one like this:

```erb
<%= render "form", movie: @movie %>
```

The scaffold also made `_movie.html.erb` — the chunk that displays a single movie — and the index page renders it for each movie in the list. That's why your rating showed up in the list *and* the detail page from one edit.

**The rule of thumb:** if you're writing the same markup twice, make it a partial.

**You can now:** explain how a page is assembled from a layout plus a view plus partials.

---

## Module 3 — Make it look good

Now the fun part. Your app already has **Tailwind** installed (that's the `--css=tailwind` from when you created it). Tailwind means you style things by adding classes directly in your HTML, instead of writing CSS in a separate file.

It looks like this:

```erb
<h1 class="text-3xl font-bold text-gray-900">My Watchlist</h1>
```

Each class does one small thing: `text-3xl` is font size, `font-bold` is weight, `text-gray-900` is color. You stack them up. The **[Tailwind docs](https://tailwindcss.com/docs)** have a search box — that's how you find the class for anything you want.

Here's a starting point for your movie list. Open `app/views/movies/index.html.erb` and try wrapping your list in something like this:

```erb
<div class="max-w-2xl mx-auto p-6">
  <h1 class="text-3xl font-bold mb-6">My Watchlist</h1>

  <div class="space-y-3">
    <% @movies.each do |movie| %>
      <div class="border rounded-lg p-4 hover:shadow-md transition">
        <h2 class="text-xl font-semibold"><%= movie.title %></h2>
        <p class="text-gray-600"><%= movie.notes %></p>
        <p class="text-sm text-amber-600"><%= "★" * movie.rating.to_i %></p>
      </div>
    <% end %>
  </div>
</div>
```

That `"★" * movie.rating.to_i` is a nice trick — remember `"ha" * 3` from Foundations? Same thing. A 4-star movie prints four stars.

**Now make it yours.** Seriously — this is your app. Change the colors. Pick a font. Make the cards look how you want. Try:

- A different color scheme (`bg-slate-900 text-white` for dark mode)
- Rounded corners, shadows, spacing you like
- Making the title a link to the movie's page

**Your challenge:** style the movie detail page (`show.html.erb`) to match, including the viewings list you built in Data Fundamentals.

> **Look at Neta's mockups.** The [design reference](../mockups.html) for Neta is built with the same Tailwind classes you're using right now. Open one and read the HTML — you'll recognize the pattern. That's not a coincidence; it's the same tool.

**You can now:** style a Rails app with Tailwind, and make something you'd actually show someone.

---

## Where this goes next

You've got an app that works, holds real connected data, and looks good. You've learned Ruby, Rails, data, and frontend — but so far each one has been its own lane.

Real work uses all of them at once, plus Git, in a single motion. That's the last piece: **Putting It Together**.

---
