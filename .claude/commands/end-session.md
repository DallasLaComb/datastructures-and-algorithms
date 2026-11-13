---
description: End a coaching session. Logs what happened and sets up tomorrow's pickup.
argument-hint: "[optional notes from the user]"
---

The user is ending a practice session. Their notes: $ARGUMENTS

1. Get the current date/time (`date`) and read `neetcode/progress.md`.
2. Summarize the session from the conversation: what was covered, problems attempted/solved (and whether solved cold, with hints, or after reading the solution), time spent, quiz results.
3. **Write the history (append-only, never edit past entries).** In `neetcode/history/YYYY-MM.md` (create the month file if missing, following the format in `neetcode/history/README.md`):
   - Add a dated session heading (or complete today's if `/start-session` already opened one).
   - Log **every question asked this session** with the user's answer **verbatim**, the result, the correct answer/why, and the skill tag. Include coding problems (attempts, hints used, cold or not, complexity stated).
   - Add the session summary and `Ladder: <before> -> <after>` footer.
   - In `neetcode/ladder.md`: append a row to **Snapshots** (date, level, cold count, all-time quiz accuracy, "where you were -> where you are"), refresh **Where you are now**, and if a gate was passed, promote the level and append to **Promotion log**. Only promote when the gate in the Rungs table is truly met.
4. Update `neetcode/progress.md`:
   - Append a dated entry to **Session log** (what was done, duration, mistakes, wins).
   - Add newly solved problems to the **Problems solved** table with pattern/insight.
   - Add reviews to the **Review queue** (1 day, 1 week, 1 month from today, as dates). Remove reviews completed today.
   - Update **Known weak spots** (add new ones, remove ones that are demonstrably fixed).
   - Update **Current status**: phase, plan week, current topic, a specific **Next up**, and clear **Active session**.
5. If checklist items in `neetcode-150-plan.md` were demonstrated from scratch, check them off.
6. Give the user a short wrap-up (include their ladder level and whether it changed): one win, one thing to watch, and exactly what they'll do next session. Keep it under 6 lines.

Do not commit unless the user asks.
