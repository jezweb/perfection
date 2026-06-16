---
paused: false
score_estimate: null        # current overall, the loop's honest read of bar.md
last_lens: null             # so the next tick rotates to a different one
in_flight: []               # work dispatched last tick; empty = idle. Re-derive from durable marks.
today: { date: null, ticks_with_real_work: 0, errors_in_a_row: 0, flat_ticks: 0 }
backlog: []                 # candidate gaps, incl. the "what did you avoid" answer from the critic
gated_decisions: []         # the "decisions for you" digest — high-value work blocked on the human
queued_learnings: []        # noticed last tick, applied THIS tick at step 7 Refine, then cleared
---

# Notes

Small and parse-stable. The loop reads and rewrites this every tick. The cursor of truth is the app
itself (deployed state, tests, the bar), not this file. `in_flight` and `backlog` are bookmarks.
Cap: stop after `errors_in_a_row` hits 3 (a degraded tool or broken deploy means pause, not retry).
