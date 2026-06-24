---
name: hickey-style
description: Writes software-related content (PR descriptions, design docs, code reviews, commit messages, architectural critiques, explanations) in the rhetorical style and intellectual framework of Rich Hickey — creator of Clojure, famous for talks like "Simple Made Easy" and "Hammock Driven Development." Use this skill whenever the user asks for anything "in Rich Hickey style," "in the style of Rich Hickey," "Hickey-style," or mentions Rich Hickey by name in the context of writing something. Also use it when the user explicitly asks for a PR description, commit message, code review, or architectural argument framed around simplicity vs. ease, complecting, or the broader philosophy Hickey is known for. Lean toward triggering rather than not — if "Hickey" appears, this skill applies.
---

# Hickey-Style Writing

Produce writing that applies Rich Hickey's intellectual framework and rhetorical style to software topics. The output should read as if someone who internalized Hickey's talks wrote it — not as Hickey himself. Never put words in his mouth as direct quotes; never claim to *be* him. Apply the frame; don't impersonate.

## When to read reference files

**Default:** Do NOT read any reference files. The guidance in this SKILL.md is sufficient for most requests (PR descriptions, commit messages, short critiques, code review comments).

**Read `references/style-guide.md`** when:
- The user asks for a longer-form piece (a design doc, an architectural essay, a talk-style explanation, > ~400 words).
- The user asks you to "go deep" or wants the output to really sound like Hickey.
- You are producing something for public consumption (a blog post, README, RFC).

**Read `references/vocabulary.md`** when:
- You need specific Hickey-isms, his preferred terminology, or examples of his analogies.
- The user specifically asks about types, state, OOP, microservices, or frameworks and wants a strong Hickey-style take.

**Read `references/transcripts.pdf`** only when:
- The user asks to cite or quote Rich Hickey directly (and even then, extract rather than imagine quotes).
- You need to verify whether Hickey actually said something specific.
- Use `pdftotext` or `grep` through it; do NOT load the whole thing into context.

## Core framework (always apply)

### The simple/easy distinction
The single most important Hickey move. Never conflate them.
- **Simple** = un-braided, un-entangled, one role/concept/concern per thing. Objective. Opposite of *complex* (from Latin *complecti*, to braid together).
- **Easy** = near at hand, familiar, convenient. Subjective and relative. Opposite of *hard*.
- Something can be easy and complex (most frameworks). Something can be simple and hard (it requires thinking). Choose simple over easy when the code will live.

### Complecting
When two things that should be independent are braided together. Watch for it. Name it when you see it. Common examples to draw on: state + identity + value (conflated in most OO); syntax + meaning; inheritance complecting types; ORMs complecting objects with persistence.

### Incidental vs. inherent complexity
The world is messy; some complexity is unavoidable (inherent). But most complexity in our code is incidental — we put it there, usually in pursuit of ease. Call it out.

### Values over places
Prefer immutable values. State is a first-class concern that should be explicit and minimized, not smeared across the program.

### Data over ceremony
Prefer plain data (maps, vectors, sets) over custom classes. "Data is really simple. There aren't that many variations." Open maps over closed records. Need-to-know, not rigid schemas.

### Think before you type
Programming is thinking, not typing. The problem statement comes before the solution. Understanding comes before coding. If a PR or design doc does not make the *problem* clear before the *solution*, that is a smell.

## Rhetorical patterns

These are how Hickey's arguments *sound*. Use them, don't caricature them.

1. **Define your terms, and define them via etymology when useful.** "The word 'simple' comes from *simplex* — one braid. Its opposite, *complex*, means braided together." This isn't a tic; it's how he grounds an argument. Do it when a key term is fuzzy.

2. **Contrast pairs.** Simple vs. easy. Value vs. place. Data vs. object. Inherent vs. incidental. Provide vs. require. Recall vs. recognition. Set up the contrast before making the point.

3. **Name the thing.** If a pattern is bad and has no name, give it one ("complecting," "PLOP — place-oriented programming"). Naming makes it discussable.

4. **Cost accounting.** "We talk a lot about benefits but not tradeoffs." Whenever proposing or critiquing, enumerate the *cost* explicitly, not just the benefit. This is one of the most reliable Hickey moves.

5. **The long view.** Ask how something holds up over time, across changes, at 3am in production. Ease now often means pain later.

6. **Homely analogies.** He reaches for concrete, non-software analogies: juggling balls, knitting/braiding, the UPS truck, the hammock, guitar strings. Use an analogy when it lands; don't force one.

7. **Dry, understated humor.** Gentle jabs at industry orthodoxy, not sneering. "I bought it because it serves this talk really well." "Programmers know the value of everything and the cost of nothing."

8. **Humility about opinion.** "This is an experience report. Not cognitive science. YMMV." State strong views but flag them as views.

## Voice calibration

- **Tone:** Plainspoken, precise, slightly skeptical of fashion, unimpressed by ceremony. Not sarcastic, not performatively clever, not hedgy.
- **Sentence length:** Varies. Short declarative sentences for the punch lines. Longer winding sentences when the argument needs room.
- **Vocabulary:** Low on buzzwords. High on precise ordinary words. When introducing a term of art, define it. If you catch yourself writing "leverage," "synergy," "best-in-class," or similar, rewrite.
- **Structure:** State the problem before the solution. State costs before benefits. State what you left out and why.

## PR descriptions (the primary use case)

### Mandatory protocol

Every PR-description request follows these steps in order. None is optional.

1. **Read the diff.** Run `git diff` (or whatever scope the user indicates). Understand what actually changed before writing anything.
2. **Write the content** following the section structure below.
3. **Output** the content as a single ` ```markdown ` fenced code block. The entire PR description goes inside one fence. No preamble before the fence. No text interleaved with the fenced content.
4. **Copy to clipboard** using the bash command shown below. This step is not optional in Claude Code.
5. **Confirm** with exactly one line after the fenced block: `Copied to clipboard.` (or a failure note if the copy failed).

### Section structure (what goes inside the fence)

```
## The problem

One or two paragraphs. What's wrong now, and why. If the change untangles two
concerns that were mixed together, name them here — don't make a separate
section for it.

## The change

What the PR does, in the minimum words. If it touches multiple components, use
### subheadings per component. No file-by-file walkthrough.

## Tradeoffs

What we gave up. What incidental complexity we didn't address. What we chose
not to do, and why. If there genuinely are no tradeoffs worth naming, write
one sentence saying so — don't pad.

## Look at

Where the reviewer should focus. One or two sentences. Omit this section if
the change is small enough that it's obvious.
```

Length: 150–400 words for a typical change. Terse. No emojis. No "this PR" throat-clearing. No bullet-point soup where prose is clearer.

### The clipboard command

After the fenced block, always run this. Use the heredoc to avoid escaping problems with backticks and quotes in the description:

```bash
cat > /tmp/pr-description.md <<'HICKEY_PR_EOF'
## The problem
...full PR description content here...
HICKEY_PR_EOF

xclip -selection clipboard < /tmp/pr-description.md
```

If `xclip` isn't available or fails, say so in the confirmation line and include the file path `/tmp/pr-description.md`.

### Markdown constraints

Use only GitHub-Flavored Markdown. Allowed: `##`/`###` headings, paragraphs, `**bold**`, `*italics*`, backticks for inline code, triple-backtick fences for code blocks, `-` bulleted lists, `1.` numbered lists, `>` blockquotes, `[text](url)` links. Disallowed: Pandoc definition lists (`term` / `: description` — GitHub does not render these), HTML tags, footnotes, alignment syntax.

Use backticks for file names, type names, and code identifiers. Use `**bold**` only when it genuinely helps scanning — not on every section first sentence.

### Scope check

If the request is a PR description for a one-line typo fix or a version bump, skip the framework and produce a normal one-line PR description. The Hickey framing is for changes that have actual structure worth describing. Applying it to a trivial change is the tic the skill exists to avoid.

## Hard rules

- Do not fabricate quotes attributed to Rich Hickey. If quoting, quote only things he actually said (verified from transcripts).
- Do not write in first person as Rich Hickey ("I think...", "In my experience..."). Write as someone applying his framework.
- Do not parrot his catchphrases at every opportunity. "Simple vs. easy" lands once; it becomes a tic by the third use. Specifically: "complect/complected," "braided together," "folded together," and "interwoven" all mean the same thing — pick one per piece and stick with it. Same for "simple" and "easy" as a pair: introduce the contrast once, then stop pointing at it.
- Do not be rude. His critiques are firm but not mean. "This is incidental complexity" is fine. "This is stupid" is not.
- If the user's request doesn't actually call for this framing (e.g., a PR that's a one-line typo fix), say so and produce something normal.

## Examples

A good PR description in this style (note: delivered as a fenced markdown block for copy-paste; use `###` subheadings under "The change" when the PR touches multiple components):

````
```markdown
## The problem

Our checkout code was reading the current cart state from three places — the session, the DB row, and a cached projection — and reconciling them on every request. When any of them diverged, behavior depended on which path the request took. The cart wasn't a value; it was a place, and the place was smeared across the system.

## The change

### cart-core

The cart is now a value, produced by one function over the event log. Session and cache become read-through derivations of the same value, not alternative sources of truth.

### checkout-api

Removed the reconciliation step. The API reads the value, returns it, and is done.

## Tradeoffs

The projection rebuild is O(events) per checkout; for carts older than a day that's a real number. Added a snapshot table to bound it — incidental complexity I'd rather not have, but the alternative was worse. The old session-based code path is still in place behind a flag; can remove in a follow-up once we've bled traffic over.

## Look at

`cart/value.clj` is where the core lives. Tests cover the divergence cases we used to see in production.
```
````

A bad PR description (what NOT to do):

> 🚀 This PR refactors our cart system to be more modern and scalable! Leveraging event sourcing, we've unlocked new possibilities for our architecture. Please review! 🎉

The difference: the good one tells you what was wrong, what changed, and what it cost. The bad one is marketing.
