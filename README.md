# Perfection

**A self-improving work loop that drives an app toward a 10/10 product — instead of grinding a task list.**

Most autonomous loops coast. Left to a checklist, a capable model finds the cheapest defensible thing
to do each tick, ships the minimum that clears it, and reports done. Perfection replaces the
checklist with a **fitness function** and an **adversarial critic** whose only job is to catch
coasting — plus a floor and a ceiling that keep every tick honest. It's a Claude Code plugin: one
skill that sets the loop up in any app and tells you how to run it.

You can't prompt a model into ambition and taste. You *can* engineer an environment where coasting
visibly fails. That's all this is.

## How it works

Into your app's `.jez/` folder, the loop installs four files:

- **`bar.md`** — what 10/10 means for *this* app: honest current scores, the dimensions it lives or
  dies on, and a set of real competitors to measure against. The fitness function.
- **`loop.md`** — the tick: pick a lens → name the single biggest gap → ship it completely → run an
  adversarial critic → record → refine.
- **`state.md`** — the loop's working memory (lens, score, backlog, gated decisions).
- **`reflections/`** — an append-only log of what every tick did.

Each tick closes the biggest gap between the app and the bar. A skeptic checks that the work wasn't
lazy and surfaces what it dodged. A **floor** rejects chore-only ticks (a version bump is not a tick);
a **ceiling** stops it manufacturing busywork once the real work is gated on a human. Every fourth tick
looks **outward** — at a competitor or current best practice — so it can't optimise against its own
stale assumptions, and an outward finding can *raise the bar*.

Full methodology: [`docs/pattern.md`](docs/pattern.md).

## Install

This is a Claude Code plugin. Add the marketplace and install:

```
/plugin marketplace add jezweb/perfection
/plugin install perfection
```

## Use

In a Claude Code session inside the app you want to improve:

> Add a perfection loop to this app.

The skill reads your app, writes a genuine per-app `bar.md`, installs the loop files, and adds a short
disposition block to your `CLAUDE.md`. Then prove it, watched:

```
Run a tick of .jez/loop.md — don't post to chat, just do it and show me.
```

Watch a few ticks (ticks 2 and 3 are where coasting shows). When it's earning its keep, let it run
self-paced or on a cron:

```
/loop Run a tick of .jez/loop.md for <app>. Post a one-line summary to <your chat> and roll into the next tick.
```

If it ever coasts, the fix is almost always the `bar.md` (too soft) or the critic (not adversarial
enough) — not the prompt. The prompt is four lines on purpose.

## What it gives you (and what it doesn't)

It gives you a loop that **won't coast** — the gradient is "move the score," and a skeptic checks for
coasting from both ends. It does **not** give you taste: *which* ambitious thing is the right one still
comes from the quality of your `bar.md`. Spend your attention there; the rest is machinery.

## Pairs with

[Walkabout](https://github.com/jezweb/walkabout) — if an app has a Walkabout, the loop uses it to walk
the real pages each tick, and keeps the tour current as the app changes.

## Licence

MIT © 2026 Jezweb Pty Ltd
