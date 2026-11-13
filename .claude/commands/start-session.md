---
description: Start a coaching session. Reads progress, checks the schedule, proposes today's work.
argument-hint: "[minutes available, optional]"
---

The user is starting a practice session. Time available: $ARGUMENTS (default: 60 minutes on a weekday, 90 on a weekend).

1. Read `neetcode/progress.md`, `neetcode/neetcode-150-plan.md`, `neetcode/ladder.md`, and the most recent file in `neetcode/history/`. Open today's session heading in the history file (per `neetcode/history/README.md`) as questions get asked, so nothing is lost if the session ends abruptly.
   Show the ladder view in 2-3 lines: "Last session (date) you were at <level/where you were>. Now: <current level, cold count, quiz accuracy>." Optionally reference a past question they got wrong to see if it's fixed.
2. Get today's date (run `date`). Compare against the dated schedule: say in one line whether they are ahead, on track, or behind, and by roughly how much.
3. In `progress.md`, set **Active session** to today's date and start time.
4. Recap in 2-3 lines: where they left off, the "Next up" item, and any weak spots relevant today.
5. Check the **Review queue**: if any reviews are due today or overdue, propose doing one first (cold re-solve).
6. Propose a concrete plan that fits the time available (e.g. 10 min warm-up quiz, 40 min new problem, 10 min wrap). Wait for the user to confirm or adjust, then begin.

Follow the teaching style in `CLAUDE.md` (hint ladder, no full solutions up front).
