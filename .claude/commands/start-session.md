---
description: Start a coaching session. Reads progress, checks the schedule, proposes today's work.
argument-hint: "[minutes available, optional]"
---

The user is starting a practice session. Time available: $ARGUMENTS (default: 60 minutes on a weekday, 90 on a weekend).

1. Read `neetcode/progress.md` and `neetcode/neetcode-150-plan.md`.
2. Get today's date (run `date`). Compare against the dated schedule: say in one line whether they are ahead, on track, or behind, and by roughly how much.
3. In `progress.md`, set **Active session** to today's date and start time.
4. Recap in 2-3 lines: where they left off, the "Next up" item, and any weak spots relevant today.
5. Check the **Review queue**: if any reviews are due today or overdue, propose doing one first (cold re-solve).
6. Propose a concrete plan that fits the time available (e.g. 10 min warm-up quiz, 40 min new problem, 10 min wrap). Wait for the user to confirm or adjust, then begin.

Follow the teaching style in `CLAUDE.md` (hint ladder, no full solutions up front).
