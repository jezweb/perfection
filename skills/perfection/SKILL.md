---
name: perfection
description: Add a self-improving "perfection loop" to an app — a background work loop that drives it toward a 10/10 product instead of grinding a task list. Sets up a per-app bar (fitness function), a tick procedure with an adversarial critic, inward + outward lenses, and a floor and ceiling that keep every tick honest. Use when the user wants an app to keep improving itself, asks to set up an autonomous build/improvement loop, or says "drive this toward perfection". Triggers: perfection loop, self-improving loop, drive toward perfection, autonomous improvement loop, make this app as good as it can be, keep improving the app.
---

# Perfection — make an app improve itself toward 10/10

Perfection is a background work loop whose unit of work is **"close the single biggest gap
between this app and what excellent looks like"** — judged by an adversarial critic, not "do the next
item on a list." It exists to beat the one failure every autonomous loop drifts into: **coasting**,
finding the cheapest defensible thing to do and calling it progress.

You can't prompt a model into ambition and taste. You *can* engineer an environment where coasting
visibly fails. That's all this is: a fitness function (`bar.md`), a critic whose only job is to catch
laziness, and a floor and ceiling that bracket every tick. The methodology and why it works are in
[`docs/pattern.md`](../../docs/pattern.md). This skill is how you set one up and run it.

## What you're installing

Into the target app's `.jez/` folder (create it if absent):

| File | Role |
|---|---|
| `bar.md` | what 10/10 means for **this specific** app — honest current scores, the dimensions it lives or dies on, and a Reference set of real competitors. The fitness function. A vague bar is the #1 cause of coasting, so this is the file that matters most. |
| `loop.md` | the tick: orient through a rotating lens → name the one biggest gap → ship it completely → adversarial critic → record → refine. Copied verbatim. |
| `state.md` | cursor, lens, score, backlog, gated decisions, queued learnings. Rewritten each tick. Copied verbatim, then filled as it runs. |
| `reflections/` | append-only log of what each tick did — the audit trail. |

Plus a short **disposition block** pasted into the app's `CLAUDE.md` (loads every session), and a
short prompt the user pastes into `/loop` or a cron to drive the ticks.

## Installing it (do this when asked to add a perfection loop)

1. **Understand the app deeply first.** Read its README, CLAUDE.md, any spec/audit/changelog docs,
   package.json, the main entry points, and find its deployed URL or built-artifact. You cannot write
   a genuine bar without knowing what the app is for and where it's weak. Skipping this produces a
   vague bar, which produces a coasting loop.
2. **Copy `templates/loop.md` and `templates/state.md`** into the app's `.jez/` **verbatim**. Create
   `.jez/reflections/` with a `.gitkeep`.
3. **Write a genuine, app-specific `bar.md`** from `templates/bar.md`'s structure. This is the real
   work:
   - The app line and a "feeling at 10/10" paragraph specific to *this* product, not generic.
   - 5–8 dimensions the app actually lives or dies on, each scored 1–10 **honestly** with a one-line
     justification grounded in what you saw. Score low where it's deserved — an honest low score is
     what gives the loop somewhere to climb. A bar of all 8s is a bar that lets it coast.
   - Current overall score + date + the single biggest known gap.
   - A **Reference set**: the real best-in-class competitors / reference products for what this app
     does, with the one thing each does better, plus a couple of "verify live" questions. Research
     these — they're what the outward lens hunts against.
   - Real non-goals (what the app should *not* become).
4. **Paste the disposition block** from `templates/disposition.md` into the app's `CLAUDE.md` (check
   it isn't already there). It's the standing "default to action, don't hedge, done means a user feels
   it" steer that belongs in CLAUDE.md once, not retyped every tick.
5. **Hand the user the run prompt** (below). Don't start a cron yourself — the user watches the first
   few ticks first.

## Running it

**Prove it first, watched.** Have the user paste this into a session in the app's folder:

```
Run a tick of .jez/loop.md — don't post to chat, just do it and show me.
```

One tick, then it stops. They say `next tick` to continue. **Watch ticks 2 and 3, not tick 1** — that's
where coasting shows. Working: each tick closes a real gap through a different lens, ships end-to-end
with a live check, and the critic names something it dodged. Coasting: it bumps a version / tidies a
doc and calls that a tick, rubber-stamps its own critic, or asks what to prioritise.

**Then let it run.** Self-paced in-session:

```
/loop Run a tick of .jez/loop.md for <app>. Post a one-line summary to <dev chat or notes channel> and roll into the next tick.
```

Or unattended on a cron (the `/schedule` skill) once it's earned trust — 30–60 min is a sensible
interval for a build loop.

## When it coasts, fix the bar or the critic — not the prompt

The prompt is four lines on purpose. If the loop starts doing cheap work, the cause is almost always:
- **a soft `bar.md`** (dimensions too vague or all scored high) → tighten it, add the dimension it's
  avoiding, lower an inflated score; or
- **a toothless critic** (step 5 rubber-stamping) → the critic must be adversarial about laziness
  specifically.

## Two failure modes the loop is built to catch — make sure they're working

- **Bubble (it optimises against its own assumptions).** Every 4th tick must use an **outward lens**:
  study a named competitor from the Reference set, research current best practice live, or read real
  user evidence. Outward findings may **raise the bar**, not just close gaps against it. Watch the
  "Last looked outward" line in `bar.md` flip from "not yet" to a date — that's the bubble breaking.
- **Ceiling (it manufactures cheap work once the real work is gated on the human).** When several
  ticks find every high-value gap blocked on a decision/credential/real-user-input/taste call, the
  loop must **stop polishing**, consolidate those into a "decisions for you" digest in `state.md`, post
  it once, and **idle** — not ship 0.005 completions to look busy.

## Notes

- **The live thing isn't always a URL.** For a desktop app, CLI, or library the loop must inspect the
  *built/running artifact*, not the source. If it can't get eyes on the running thing, it must say so
  and not claim a visual/feel gain it couldn't verify (the critic's step-5 question c enforces this).
- **This is not a backlog grinder.** For working through a fixed cohort (enrich N records, sweep a
  catalogue) where doing the least per tick is correct, use a plain backlog loop. This loop's whole
  design fights minimalism, which is wrong for grinding and right for chasing quality.
- **It rents taste, it doesn't own it.** The loop forces appetite (it won't coast); *which* ambitious
  thing is the right one still comes from the quality of `bar.md` and any review panel it consults.
  Spend your attention on the bar.
