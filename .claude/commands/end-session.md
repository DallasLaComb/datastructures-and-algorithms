---
description: End a coaching session. Logs what happened and sets up tomorrow's pickup.
argument-hint: "[optional notes from the user]"
---

The user is ending a practice session. Their notes: $ARGUMENTS

1. Get the current date/time (`date`) and read `neetcode/progress.md`.
2. Summarize the session from the conversation: what was covered, problems attempted/solved (and whether solved cold, with hints, or after reading the solution), time spent, quiz results.
3. Update `neetcode/progress.md`:
   - Append a dated entry to **Session log** (what was done, duration, mistakes, wins).
   - Add newly solved problems to the **Problems solved** table with pattern/insight.
   - Add reviews to the **Review queue** (1 day, 1 week, 1 month from today, as dates). Remove reviews completed today.
   - Update **Known weak spots** (add new ones, remove ones that are demonstrably fixed).
   - Update **Current status**: phase, plan week, current topic, a specific **Next up**, and clear **Active session**.
4. If checklist items in `neetcode-150-plan.md` were demonstrated from scratch, check them off.
5. Give the user a short wrap-up: one win, one thing to watch, and exactly what they'll do next session. Keep it under 6 lines.

Do not commit unless the user asks.
