# Hickey-style deep reference

Load this file only when doing longer-form writing (design docs, essays, detailed architectural critiques). For PR descriptions and short outputs, the main SKILL.md is enough.

## Core theses (with more nuance than the main file)

### 1. Simple is not easy; easy is not simple

From *Simple Made Easy*: "simple" and "easy" are not synonyms. We treat them as if they were, and it costs us.

- **Simple** is an objective property: one concern, one role, one dimension per thing. You can point at code and ask "is this braided together with something else?" and get a yes/no answer that most people agree on.
- **Easy** is subjective: near your hands, near your understanding, near your current skill set. What's easy to you is not easy to the new hire.
- The confusion comes from good marketing. Libraries describe themselves as "dead simple" when they mean "easy to install and use in the demo." The complexity shows up later.
- The rule of thumb: **choose simple now, pay for it now, benefit from it later.** Most decisions that feel urgent are really decisions between incurring complexity cost now vs. later, not avoiding it altogether.

### 2. Complecting is the enemy

"Complect" is the verb form of "complex." It means to braid or fold together. Hickey rehabilitates this archaic word because we lack vocabulary for the thing that matters most in design.

Things that are commonly complected in code and shouldn't be:
- State, identity, and value (classic OO)
- Types and optionality (`Maybe String` complects "string" with "may or may not be present")
- Inheritance complects two types definitionally
- Syntax complects meaning with order
- ORMs complect data with persistence and with object lifecycle
- Frameworks complect your problem with their opinions about a hundred other problems
- Microservices often complect a service boundary with a deployment boundary with an ownership boundary

When reviewing code or a design, the highest-leverage question is: **"What is complected here that could be separate?"**

### 3. The problem of place

Variables (especially mutable ones) are *places*. A place is a slot in memory whose contents change over time. When you read the place, you get whatever happens to be there now.

This is a problem because:
- Two observers looking at the same place at different times see different things.
- You cannot reason about behavior by looking at the code alone; you have to reason about what might be in the place at each point.
- Testing is hard because you have to control the state of every place the code touches.

Values don't have this problem. A value is what it is; it doesn't change. `42` today is `42` tomorrow. `[1 2 3]` today is `[1 2 3]` forever.

Hickey's position: **programs should be about values; places should be the minority, named and explicit.**

### 4. Data over objects

"Data is really simple. There aren't that many variations in the essential nature of data: there are maps, there are sets, there are linear sequential things."

The argument against objects:
- Each class is a new, specific thing that you have to learn.
- Classes complect data with behavior, with identity, with lifecycle, with inheritance.
- You can't easily serialize, send over the wire, persist, or diff an object. You can do all of these trivially with data.
- Methods on a class lock the data behind an API that only that class's author anticipated. Data is open to new operations forever.

The positive case for data (maps, vectors, sets, keywords, strings, numbers):
- Universal. Any language, any system, any time.
- Open. A map with 10 keys today can have 11 tomorrow without breaking readers.
- Inspectable. Print it. Diff it. Log it.
- Need-to-know. Consumers pull out what they need and leave the rest alone.

### 5. Types as a limited tool

Hickey's position on static types is more nuanced than his critics grant. The short version:

- Types tell you about the shape of data, not about what a function *does*. `a -> a` is satisfied by infinitely many functions.
- Types encode decisions that are often context-dependent. "Is this field optional?" depends on who's asking and when — the same field is required on the way in and optional on the way out. Encoding optionality in the *type* of the field mixes these contexts.
- `Maybe T` is "evidence of a missing feature in the type system" (first-class union types) rather than a satisfying answer to optionality.
- Strong typing gives you some guarantees, but those guarantees are often about things that weren't going to go wrong anyway, while the things that *do* go wrong (wrong business logic, wrong schema, data you didn't expect) aren't caught.

Use this material carefully. It's contested territory and Hickey is criticized by static-typing advocates for some of his framings. If writing on this topic, represent it as *his view*, not as settled fact.

### 6. Hammock-driven development

Programming is thinking. Thinking doesn't happen at the keyboard. Key moves:

- **Write the problem down.** Not the solution. The *problem*. What are the constraints? What are the inputs? What are the things it has to do, the things it has to not do, the things that could go wrong?
- **Load the background mind.** Review what you've written until you can close your eyes and recall the pieces. This is when you move from input-reception mode to recall mode.
- **Step away from the computer.** The computer is a distraction machine. Go somewhere you can close your eyes (the hammock). No inputs. Just turning the problem over.
- **Waiting.** The background mind works on its own schedule. Sleep on it. Sometimes sleep on it for months. Work on more than one problem over time so you have something to do while any one of them is incubating.
- **The waking mind is a critic.** When you think you've had an idea, let the waking mind pick it apart. Great ideas survive that; bad ideas die there, and that's good.

For a PR description, the Hammock move is: before writing what the change does, write what the *problem* was. That single reframe often rewrites the whole PR.

### 7. Design is about leaving things out

"Design is about making choices." A big part of designing a system well is deciding what *not* to include, what *not* to solve, what *not* to couple. A framework that ships with an opinion on everything has pre-made a hundred choices for you, most of which you wouldn't have made that way. The cost is hidden because it's already paid by the time you start.

When reviewing a design: ask what's been left out and why. If the answer is "nothing, we included everything we might need," that's a warning sign.

## Rhetorical moves in detail

### The etymology move
Used sparingly, this is Hickey's signature opener for a key term.
- "The word *simple* comes from *simplex* — one fold, one braid. Its opposite, *complex*, means braided together."
- "The root of *easy* goes back to a Latin word meaning *to lie near* — the same root as *adjacent*."
- Only use this when the term is genuinely fuzzy and the etymology illuminates. Don't do it for every word.

### The contrast pair setup
State the two things, define the difference, then argue.
- "Programs deal with values and places. A value is something like 42, or the vector [1 2 3] — it is what it is. A place is a slot that holds a value, and that value can change. These are very different things, and conflating them gets us into trouble."

### The "what did we give up" move
After any proposal:
- "What did we give up by doing this? We gave up X. Is that worth Y? Maybe. Let's think about it."
- This is the antidote to hype. Apply it to your own proposals, not just others'.

### The homely analogy
Use when a technical point has a clean non-technical parallel.
- Complexity = knitting yarn
- State = the UPS truck that might have your TV on it
- Too many things at once = juggling; past 7±2 balls, you drop them
- Thinking without distractions = the hammock

Don't force one. A bad analogy is worse than none.

### The understated jab
- "I bought it because it serves this talk well." (on a speculative etymology)
- "Programmers know the value of everything and the cost of nothing."
- "It's not better. It's the same problem."
- Dry, specific, not mean-spirited.

## What to avoid

- Direct impersonation ("As Rich, I think..."). Write in the style, not as the person.
- Manufactured quotes. If a "Hickey quote" isn't in the transcripts, don't invent one.
- Catchphrase stacking. Using "simple vs. easy," "complecting," and "PLOP" all in one paragraph is parody, not style.
- Sneering at other languages or paradigms by default. His critique of OOP is specific and reasoned, not blanket.
- Hedging. "It could be argued that perhaps..." is the opposite of his voice. Make the claim; own it.
- Marketing tone. "Excited to share," "leveraging," "synergy," "best-in-class" — all wrong.
