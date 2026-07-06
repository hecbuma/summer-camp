---
layout: default
title: "Lane 0 — Foundations"
---

# Lane 0 — Foundations

> A start-from-scratch track for your first summer in code. No experience needed, and no rush. The others on the team already knew a bit of programming coming in — you're building that base now, in the same tools they use. By the end of this you'll understand how programs work, be at home in the terminal, and make your first real change to a live codebase. That's a serious summer.

Work through it in order, at your own pace. There's no deadline and no "behind." Each module is small on purpose: you type a little, run it, see what happens, and move on. That loop — **write it, run it, see it** — is the whole job, and you start doing it in module zero.

---

## A few things before you start

**Turn the AI assistants off — for now.** Claude, Copilot, all of it. This is going to feel backwards, because everyone talks about how AI writes code for you. Here's the thing: it does, and that's exactly the problem right now. If the AI writes it, the AI understood it — not you. These first modules are about building the muscle in *your* head. Type everything yourself, even when it's slow, even when you make mistakes. Especially then. You'll turn the AI back on later, once you have something real for it to build on. Trust this part.

**Errors are normal. Errors are friendly.** You are going to see red error messages constantly. That is not you failing — that is the computer telling you exactly what confused it. Professionals see errors all day long. The skill isn't avoiding them, it's reading them. We'll practice that early so they stop being scary.

**Ask when you're stuck.** Try something first — genuinely try. But if you've been stuck for a while, that's what your coach is for. Getting unstuck quickly is part of learning, not cheating.

---

## Module 0 — Make it yours

Before any code, make the space yours. This is your workshop now.

1. Open your **terminal** and your **editor** (VS Code).

2. Pick a color theme you like. In VS Code, press `Cmd/Ctrl + K` then `Cmd/Ctrl + T` and arrow through the themes. Choose one that feels good — you'll be looking at it all summer, so this matters more than it sounds.

3. In the terminal, make a folder for your experiments and move into it:
   ```bash
   mkdir ruby-camp
   cd ruby-camp
   ```

4. In your editor, open that `ruby-camp` folder. Create a new file called `hello.rb` and type exactly this:
   ```ruby
   puts "Hello. My name is ___ and this is my first program."
   ```
   (Put your actual name in.)

5. Save it. Back in the terminal, run it:
   ```bash
   ruby hello.rb
   ```

You just made a computer do something. Now change the sentence, save, and run it again. Change it again. That back-and-forth — edit, save, run — is what you'll be doing for the rest of your life in this field. Get comfortable with how fast it is.

**You can now:** create a file, write a line of Ruby, and run it in your terminal.

---

## Module 1 — Two ways to run code

There are two ways to run Ruby, and you'll bounce between them constantly.

**Way one: a file you save and run.** That's what you just did with `hello.rb`. Good for anything you want to keep.

**Way two: `irb`, a place to try things instantly.** Type `irb` in your terminal and press Enter. Now you're in a live Ruby session — type something and it answers immediately:

```ruby
2 + 2
"hello".upcase
10 * 60
```

Each line, press Enter and watch it respond. `irb` is your scratchpad — perfect for "wait, what does this do?" moments. When you're done, type `exit` to leave.

Spend a few minutes just poking at things in `irb`. Try `"your name".length`. Try `100 / 7`. Try `"ha" * 3`. Break things on purpose and see what happens.

**You can now:** use `irb` to experiment, and know when to reach for a file instead.

---

## Module 2 — Building blocks: variables, words, numbers

A **variable** is a labeled box you put a value in. Make a new file, `intro.rb`:

```ruby
name = "Sam"
age = 15

puts "This is #{name}, and they are #{age} years old."
```

Run it. That `#{...}` inside the string is how you drop a variable into text — it's called interpolation, and you'll use it constantly.

Now make it interactive. Change the file to *ask* for the name:

```ruby
puts "What's your name?"
name = gets.chomp

puts "Nice to meet you, #{name}!"
```

`gets` reads what the person types. `.chomp` trims off the invisible "Enter" at the end. Run it and type your name when it waits for you.

**Now break it on purpose.** Delete the `.chomp` so it just says `name = gets`, and run it again. See how the greeting looks a little off, with the name on its own line? That's the leftover Enter. Put `.chomp` back. You just learned what it does by removing it — that's a real debugging skill.

**Read your first error.** Delete the closing quote on one of your strings and run it. You'll get a red message. Read it — it'll say something about an "unterminated string" and point at a line number. It's not yelling at you; it's telling you where it got confused. Fix the quote and move on.

**You can now:** store values in variables, put them inside text, ask the user for input, and read an error message instead of panicking.

---

## Module 3 — Decisions and repeats

Programs get interesting when they can decide and repeat.

**Deciding** uses `if`:

```ruby
puts "How old are you?"
age = gets.chomp.to_i

if age >= 18
  puts "You're an adult."
elsif age >= 13
  puts "You're a teenager."
else
  puts "You're a kid."
end
```

`.to_i` turns the typed text into a number so you can compare it. Run it with different ages.

**Repeating** uses loops. Now build something real — a number guessing game. Make `guess.rb`:

```ruby
secret = rand(1..10)
puts "I'm thinking of a number between 1 and 10. Guess it!"

guess = gets.chomp.to_i

if guess == secret
  puts "You got it!"
else
  puts "Nope — it was #{secret}."
end
```

Run it a few times. It only gives you one shot, though. Let's fix that with a loop that keeps going until you're right:

```ruby
secret = rand(1..10)
puts "I'm thinking of a number between 1 and 10."

guessed = false
until guessed
  puts "Your guess?"
  guess = gets.chomp.to_i

  if guess == secret
    puts "You got it!"
    guessed = true
  elsif guess < secret
    puts "Too low. Try again."
  else
    puts "Too high. Try again."
  end
end
```

Play it. You built a real game — it thinks of a number, takes your guesses, gives hints, and knows when you win. That's genuinely a program.

**Your challenge:** count the guesses and tell the player how many tries it took. (Hint: make a variable that starts at 0 and goes up by one each guess.)

**You can now:** make a program decide with `if`, repeat with a loop, and combine them into something that actually plays.

---

## Module 4 — Collections: lists and lookups

So far your variables hold one thing. Often you want a *bunch* of things.

**An array is an ordered list.** Make `collections.rb`:

```ruby
foods = ["tacos", "pizza", "ramen"]

puts foods[0]      # the first one — counting starts at 0
puts foods.length  # how many

foods << "sushi"   # add one to the end

foods.each do |food|
  puts "I like #{food}."
end
```

That `.each` loop runs once for each item — `food` is the current one each time around. Run it and watch it print a line per food.

**A hash stores labeled values** — like a little lookup table:

```ruby
person = {
  name: "Alex",
  age: 15,
  city: "Colima"
}

puts person[:name]
puts "#{person[:name]} lives in #{person[:city]}."
```

You look things up by their label (`:name`, `:city`) instead of by position. You'll see hashes *everywhere* in Rails.

**Your challenge:** make an array of a few hashes — each one a person with a name and a city — and loop over them printing "NAME is from CITY" for each. This combines everything: an array, hashes, and a loop. Take your time.

**You can now:** hold lists of things in arrays, labeled data in hashes, and loop over them.

---

## Module 5 — Methods and objects

This module is the quiet bridge to Rails, so it matters. Go slow.

**A method is a named set of steps** you can reuse. Make `methods.rb`:

```ruby
def greet(name)
  puts "Hello, #{name}! Welcome to camp."
end

greet("Sam")
greet("Alex")
greet("Jordan")
```

You wrote the greeting logic once and used it three times. That's the whole point of methods — bundle up steps, give them a name, reuse them.

**Now, objects.** Here's something you've been doing without noticing: when you wrote `"hello".upcase` or `foods.length`, you were calling methods *on things*. In Ruby, everything is an object, and objects have methods — things they can do. `"hello"` knows how to `.upcase` itself. `[1, 2, 3]` knows how to `.sum` itself. Try both in `irb`.

**You can make your own kinds of object.** A class is a blueprint:

```ruby
class Dog
  def initialize(name)
    @name = name
  end

  def bark
    puts "#{@name} says woof!"
  end
end

rex = Dog.new("Rex")
rex.bark
```

Read that carefully. A `Dog` has some **data** it carries (`@name`) and some **behavior** it can do (`bark`). You made a specific dog named Rex, and told it to bark. That combination — data plus behavior, bundled together — is what an object is.

**Why this is the bridge:** hold onto this idea, because next module it pays off. In Rails, a **model** is an object just like your Dog — it holds data (a restaurant's name, its score) and knows how to do things. A **controller** is a class full of methods, where each method handles one kind of web request. You already know what classes and methods are now. Rails is going to be *the Ruby you already know*, arranged for a job.

**You can now:** write your own methods, understand that everything in Ruby is an object with behavior, and build a simple class of your own.

---

## Module 6 — From Ruby to the web

Time to connect what you know to how websites actually work. This is the part that makes Rails click.

**A website is just a program answering requests.** When you open a page, your browser sends a *request* to a computer somewhere, and a program on that computer sends back a *response*. That's it. A website isn't a magic place — it's a program that receives requests and hands back answers. Let's prove it on your own machine.

**Serve a file with Ruby.** In your `ruby-camp` folder, make a file called `index.html` with this inside:

```html
<h1>My first web page</h1>
<p>This was served by a Ruby program running on my own computer.</p>
```

Now, in the terminal, start Ruby's tiny built-in web server:

```bash
ruby -run -e httpd . -p 8000
```

Open your browser to **http://localhost:8000**. There's your page.

Sit with what just happened: a Ruby program (the little server) received a request from your browser and handed back a file *you made*. The server isn't magic. It's Ruby, doing the request-and-response thing. When you're done looking, go back to the terminal and press `Ctrl + C` to stop it.

**Now the leap.** That server just handed back a file sitting on disk. But what if, instead of reading a file, a program *decided* what to send back — looked something up, did some math, built the answer on the spot? That's the whole idea of a **backend**: the response isn't a file, it's something the program *computes* for each request. Your guessing game computed responses to your guesses. A web backend does the same thing, but the "input" is a web request and the "output" is a web page.

**Rails is exactly this, organized.** Rails is Ruby, arranged to answer web requests, with a clear place for each job:

- **Routes** look at an incoming request and decide who should handle it.
- **Controllers** are classes full of methods — each method handles a request. (Classes and methods. You built those in module 5.)
- **Models** are objects that hold data and talk to the database. (Objects. Also module 5.)
- **Views** are the response that gets sent back to the browser.

Every single piece of Rails maps to something you've already done by hand. That's not an accident — that's why we did it in that order.

**See it in the real thing.** Ask your coach to help you start the Neta app, then open **http://localhost:3000**. Watch it load, and narrate it to yourself in your new vocabulary: *my browser sent a request; Rails looked at the route and picked a controller method; that method got some data from a model; it sent back a view — the page I'm looking at.* Same request-and-response loop as your little server. Just bigger.

**You can now:** explain what a web server does, that you've run one yourself, and how Rails is the Ruby you know arranged to answer web requests.

---

## Where this goes next

That's the Foundations lane. If you got here, you've gone from never having written a line of code to understanding how programs work, living in the terminal, and knowing what Rails actually *is* — the Ruby you know, arranged to answer web requests. That's a real base, and it's the exact one the rest of the team started with.

You're not going to jump straight into the big Neta codebase yet. Next you'll build a tiny Rails app of your own — from nothing, working, in one sitting — so Rails stops being a concept and becomes something your hands have done. That's the **Rails Fundamentals** lane. Head there when you're ready.

---
