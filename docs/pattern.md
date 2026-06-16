# The pattern — why a perfection loop resists coasting

## The problem it solves

Give a capable model an open-ended mandate ("make this app as good as it can be") and a heartbeat
(`/loop` or a cron), and it will work. But left to a task list, it drifts into **coasting**: each tick
it finds the cheapest defensible thing to do, ships the minimum that clears it, and reports done.
Reply to one message, tick complete. The list reframes the job as *service the list*, so the reward
gradient points at clearing, not at impact.

You cannot fix this by writing a better prompt. The deeper trap is **Goodhart's law**: any structure
the model optimises against gets gamed toward minimal compliance. A 600-word "be ambitious, don't
hedge, trust your instinct" loop prompt fails the same way a checklist does — a satisficing model
finds the floor of whatever you specify. More detail just gives it more precise instructions for doing
the least.

You also can't prompt a *disposition* — ambition, taste, decisiveness — into a model that doesn't have
it. So stop trying to specify the work or install a personality. **Specify the standard, and put a
skeptic in the loop.**

## The four mechanisms

1. **A fitness function, not a task list (`bar.md`).** The unit of work is "close the biggest gap
   between the app and the bar," where the bar is a per-app definition of 10/10 across the dimensions
   that app lives or dies on. Cheap work visibly fails to move the score, so doing the least stops
   being a winning move. A task list invites minimalism; a "raise the score" mandate doesn't.

2. **An adversarial critic (the spine).** Every tick ends with a skeptic whose only job is to catch
   coasting: *"Would a senior reviewer call this lazy? What's the most valuable thing you avoided
   because it was hard? Did you actually look at the rendered result, or just the diff?"* The avoided
   thing becomes the next tick's work. This is the one structure a satisficing model can't game by
   doing the minimum — because catching the minimum is the critic's entire purpose.

3. **A floor.** A tick whose substance was only a chore — a chat reply, a version bump, a doc tidy —
   is a *failed* tick. The floor stops coasting at the bottom: it can't dress up a chore as progress.

4. **A ceiling.** The inverse. Once an app is genuinely near-done and every high-value gap is gated on
   a human (a decision, a credential, real-user input, a taste call), the loop must *stop manufacturing
   tiny work* — consolidate the gated items into a decision digest, surface them once, and idle. The
   floor catches coasting from below; the ceiling catches it from above. They're the same failure: a
   tick that looks productive and isn't.

Two supporting mechanisms keep it honest over time:

- **Outward lenses break the bubble.** A loop that only ever walks its own app optimises against its
  own assumptions and goes stale, because the bar is the model's own frozen idea of "good." So every
  Nth tick must look *outward* — at a named competitor, at current best practice verified live, at
  real user evidence — and an outward finding may **raise the bar**, not just close a gap against it.
  Research that can only close gaps against the current bar can never escape it; research that can move
  the goalposts can.

- **Disposition lives in CLAUDE.md, once.** The "default to action, build a default at forks, never
  end asking, done means a user feels it" steer loads every session from CLAUDE.md instead of competing
  with the task content every tick.

## What it does and doesn't give you

It gives you a loop that **won't coast**, because the gradient is "move the score" and a skeptic is
checking for coasting from both ends. What it does **not** give you is taste — the judgement for
*which* ambitious thing is the right one. The loop forces appetite and rents taste: which gap matters
most still comes from the quality of `bar.md` and any review panel the critic consults. So the bar is
where your attention belongs; everything else is machinery.

## Field notes

Run unattended overnight against real, mature apps, a perfection loop ships dozens of substantive,
varied changes — and the strongest evidence it isn't self-deceiving is that the loops **catch and
correct their own overclaims** ("the perf fix didn't actually reduce the fetch"; "I claimed the whole
library was done, then diffed against the design doc and found two missing"). The adversarial critic
biting its own work is the pattern working at its deepest.

The ceiling was the lesson that needed adding after the first runs: a well-built app reaches a point
where the loop kept shipping ~0.005 polish because the floor only stops coasting from below. Naming the
top end explicitly — surface the gated decisions and idle — is what stops a near-perfect app from
generating busywork forever.

This pattern was distilled from comparing how different model generations sustain autonomous work, and
from watching real loops run. The throughline: the gap between a model that needs constant steering and
one that doesn't is mostly **appetite and taste, not capability** — and a harness can manufacture the
appetite while renting the taste.
