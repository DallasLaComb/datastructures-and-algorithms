# Progress (coach state file)

Maintained by Claude via `/start-session` and `/end-session`. Plan lives in `neetcode-150-plan.md`.

## Goal
Answer all 150 NeetCode problems cold in Java, with correct time/space analysis and clear out-loud explanations, to ace any technical interview. Progress: 0/150 solved cold.

## Current status
- **Phase:** 0 (Prerequisites)
- **Plan week:** 1 (Mon 2026-11-16 target start; actual start: 2026-11-12, pre-week)
- **Currently on:** Phase 0 section A (Complexity analysis)
- **Next up:** Retry the `i /= 3` loop question (complexity + iteration count for n = 81), then 10 Big-O snippets
- **Active session:** none

## Known weak spots
- Big-O of loops where the counter changes by a constant factor (log) vs constant amount (linear). Misjudged the `i *= 2` loop on 2026-11-12.
- Recursion complexity (naive fib = O(2^n)), unsure on 2026-11-12.
- Collection costs: thought `ArrayList.get(i)` was O(n); it's O(1). `remove(0)` is O(n).

## Phase 0 checklist progress
Items are checked in `neetcode-150-plan.md`. Sections: A Complexity [ ], B Java [ ], C Collections [ ], D Structures [ ], E Recursion [ ], F Tooling [ ], Exit gate [ ].

## Problems solved
| # | Problem | Topic | First solved | Cold? | Pattern / insight | Next review due |
|---|---------|-------|--------------|-------|-------------------|-----------------|

## Review queue
(Problems due for a cold re-solve, with due dates. Claude adds 1-day / 1-week / 1-month reviews after each problem.)

## Session log
### 2026-11-12 (Thu), setup session
- Created plan and diagnostic quiz. Quiz: Integer `==` correct; Big-O doubling loop wrong; fib complexity unsure; ArrayList cost wrong.
- Walked through why `i *= 2` is O(log n) (distance to n halves each step).
- Open item: answer the `i /= 3` follow-up.
