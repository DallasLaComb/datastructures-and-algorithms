# NeetCode 150 Plan (Java)

## Goal

Be able to answer all 150 NeetCode problems, solved cold in Java with correct time/space analysis, so I can ace any technical interview.

Definition of done: every problem solved cold (no hints, no solution peeking), and I can explain the approach out loud, including tradeoffs and complexity.

---

Profile: brand new to LeetCode, working 9-5, ~1 hr/weekday + ~2.5 hrs weekend (~7.5 hrs/week), no deadline, goal = solve all 150 and pass interviews.

Note: the neetcode.io/roadmap page is JS-rendered and couldn't be fetched, so topic order/counts below follow the standard NeetCode 150 layout. Verify against the site as you go.

---

## Diagnostic quiz results

| # | Topic | Result | Fix |
|---|-------|--------|-----|
| 1 | Big-O of `for (i=1; i<n; i*=2)` | Wrong (said ~O(n/2)) | Answer is **O(log n)**. `i` doubles: 1,2,4,8... reaches n after log2(n) steps. "n/2" would be `i += 2`. Rule: *multiply/divide the counter = log; add/subtract = linear.* |
| 2 | Java `Integer` `==` (1000 vs 1000) | Correct | Keep using `.equals()` for boxed types. Cache is only -128..127. |
| 3 | Naive `fib` complexity | Not sure | **O(2^n)**: each call spawns two calls, tree depth n. Memoization collapses it to O(n). This is the seed of all DP. |
| 4 | Which `ArrayList` op is O(n) | Wrong (said `get(i)`) | `get(i)` is **O(1)** (backed by an array). `remove(0)` / `add(0, x)` is **O(n)** (shifts everything). |

Takeaway: Java basics are okay; **Big-O, recursion, and collection-cost intuition are the gaps.** Phase 0 targets exactly those.

---

## Phase 0: Prerequisite checklist (~3 weeks, before problem #1)

Check an item only when you can do it from a blank file without looking anything up.

### A. Complexity analysis
- [ ] Explain O(1), O(log n), O(n), O(n log n), O(n^2), O(2^n), O(n!) with one example each
- [ ] Rule: loops in sequence add, nested loops multiply, halving/doubling = log
- [ ] Analyze time AND space (recursion stack counts as space)
- [ ] Know amortized O(1): `ArrayList.add`, `HashMap.put`
- [ ] Rule of thumb: n <= 10^5 needs O(n log n) or better; n <= 20 allows O(2^n)
- [ ] Redo quiz Q1 and Q3 correctly and explain why

### B. Java language essentials
- [ ] Primitives vs boxed types; `int` overflow; `long` literals (`1L`); `Integer.MAX_VALUE/MIN_VALUE`
- [ ] `==` vs `.equals()` (strings and boxed types)
- [ ] `String` is immutable; use `StringBuilder` for building (`append`, `reverse`, `insert`, `setLength`)
- [ ] `String` / `char[]` ops: `charAt`, `toCharArray`, `substring`, `split`, `String.valueOf`, `char - 'a'` indexing
- [ ] Arrays: `new int[n]`, 2D arrays, `Arrays.fill`, `Arrays.sort`, `Arrays.toString`, `Arrays.asList`, `.length` vs `.size()`
- [ ] Sorting with comparators: `Arrays.sort(arr, (a,b) -> a[0]-b[0])` (use `Integer.compare` to avoid overflow)
- [ ] Generics and lambdas basics; enhanced `for`; ternary; `Math.max/min/abs`
- [ ] Writing a small class with fields + constructor (you'll write `ListNode`, `TreeNode`, `Node`)
- [ ] Integer division, `%` with negatives, `Math.floorMod`

### C. Java Collections (know the API AND the cost)
- [ ] `ArrayList`: get O(1), add end O(1)*, add/remove front or middle O(n)
- [ ] `HashMap` / `HashSet`: `put/get/containsKey/getOrDefault/merge/computeIfAbsent`, iterate `entrySet()`
- [ ] `ArrayDeque` as stack (`push/pop/peek`) and queue (`offer/poll/peek`). **Avoid `Stack` and `LinkedList` for this.**
- [ ] `PriorityQueue`: min-heap default, max-heap via `Collections.reverseOrder()`, custom comparator, O(log n) add/poll
- [ ] `TreeMap` / `TreeSet`: sorted, `floorKey/ceilingKey`, O(log n)
- [ ] `LinkedList` vs `ArrayList` tradeoffs; `Collections.sort/reverse/swap`
- [ ] Your existing `datastructures/` notes cover Java syntax; use them as your cheat sheet

### D. Core data structure concepts (by hand, no library)
- [ ] Array and dynamic array resizing
- [ ] Singly linked list: insert, delete, reverse (write it from scratch)
- [ ] Stack and queue: LIFO vs FIFO, implement each with an array
- [ ] Hash table: hashing, collisions, why lookup is O(1) average
- [ ] Binary tree and BST: node structure, inorder/preorder/postorder
- [ ] Heap: what a min-heap guarantees, why it's O(log n)
- [ ] Graph: adjacency list vs matrix; how to build one from an edge list

### E. Recursion and traversal (the single most important prerequisite)
- [ ] Write recursive `factorial`, `sum of array`, `reverse string`, `power(x, n)`
- [ ] Identify base case + recursive case + what shrinks
- [ ] Draw the call stack / recursion tree for `fib(4)` by hand
- [ ] Add memoization to `fib` with a `HashMap` or array, state new complexity
- [ ] Recursive DFS on a binary tree (preorder/inorder/postorder)
- [ ] Iterative BFS on a tree using a queue
- [ ] Generate all subsets of `[1,2,3]` recursively (backtracking preview)

### F. Practice tooling and habits
- [ ] LeetCode account; Java selected as language; comfortable with the editor
- [ ] Know how to test your own cases (write 3: normal, empty/single, edge)
- [ ] Git repo structure for solutions (suggested: `neetcode/<topic>/<ProblemName>.java`)
- [ ] Tracking sheet (problem, date, attempts, "solved cold?", notes)

### Phase 0 exit gate
Solve these 5 cold in Java (target <= 20 min each, no solution peeking). If you stall, review the matching checklist section and retry.
1. Two Sum (hash map)
2. Valid Anagram (counts array/map)
3. Reverse Linked List (pointers)
4. Maximum Depth of Binary Tree (recursion)
5. Valid Parentheses (stack)

Suggested Phase 0 schedule (3 weeks):
- **Week 1:** Sections A + B. Re-quiz yourself on Big-O with 10 snippets (ask me to generate them).
- **Week 2:** Sections C + D. Hand-write a linked list, stack, and queue.
- **Week 3:** Sections E + F, then the exit gate problems.

---

## How to practice (this matters more than the schedule)

For every problem:
1. **Read + restate** it in your own words; write 2-3 test cases first (5 min).
2. **Attempt for 20-25 min** max. Think brute force first, then ask "what's repeated work?"
3. **Stuck? Watch/read the solution**, then close it and **re-implement from memory**.
4. **Write down**: pattern name, time/space, the one insight that unlocked it.
5. **Spaced review:** re-solve cold after 1 day, 1 week, 1 month. A problem only counts as "done" once you solve it cold without hints.

Looking at solutions is expected and fine. Skipping step 3's re-implementation is the mistake.

### Weekly rhythm (~7.5 hrs)
| Day | Time | Focus |
|-----|------|-------|
| Mon | 1 hr | New problem #1 (full method above) |
| Tue | 1 hr | New problem #2 |
| Wed | 1 hr | Re-solve a past problem cold (spaced review) |
| Thu | 1 hr | New problem #3 |
| Fri | 1 hr | Light: write up notes, or watch a topic explainer. Protect your rest. |
| Sat | 1.5 hr | New problem #4, plus attempt a harder one |
| Sun | 1 hr | Weekly review: redo that week's hardest problem, update tracker |

Target: ~4-5 new problems/week plus 2-3 reviews. Medium problems may take two sittings early on. That's normal.

---

## Phase 1: The NeetCode 150 (about 37 weeks after Phase 0)

Order follows the roadmap dependencies. Don't skip ahead: later topics assume earlier ones.

Week 1 starts Mon 2026-11-16. Dates are a target, not a deadline: if you fall behind, shift everything down rather than skipping.

| Weeks | Dates (Mon-Sun) | Topic | # | Core idea to internalize |
|-------|-----------------|-------|---|--------------------------|
| 1-3 | Nov 16 - Dec 6, 2026 | **Phase 0 prerequisites** | | See checklist above |
| 4-5 | Dec 7 - Dec 20 | Arrays & Hashing | 9 | Hash map/set for O(1) lookup, frequency counting |
| 6 | Dec 21 - Dec 27 | Two Pointers | 5 | Converging pointers on sorted data |
| 7 | Dec 28 - Jan 3 | Sliding Window | 6 | Expand/shrink window, maintain invariant |
| 8-9 | Jan 4 - Jan 17 | Stack | 7 | LIFO matching, monotonic stack |
| 10-11 | Jan 18 - Jan 31 | Binary Search | 7 | Halve search space; search on the answer |
| 12-13 | Feb 1 - Feb 14 | Linked List | 11 | Fast/slow pointers, reversal, dummy node |
| 14-16 | Feb 15 - Mar 7 | Trees | 15 | DFS recursion, BFS levels, BST property |
| 17 | Mar 8 - Mar 14 | **Checkpoint 1**: cold re-solve 15 earlier problems | | Fix weak spots before the hard stuff |
| 18 | Mar 15 - Mar 21 | Heap / Priority Queue | 7 | Top-K, streaming median |
| 19-20 | Mar 22 - Apr 4 | Backtracking | 9 | Choose / explore / un-choose |
| 21 | Apr 5 - Apr 11 | Tries | 3 | Prefix tree node structure |
| 22-24 | Apr 12 - May 2 | Graphs | 13 | BFS/DFS on grids, topological sort, union-find |
| 25-26 | May 3 - May 16 | Advanced Graphs | 6 | Dijkstra, MST, Bellman-Ford |
| 27-29 | May 17 - Jun 6 | 1-D Dynamic Programming | 12 | Define state, recurrence, memo then tabulate |
| 30-32 | Jun 7 - Jun 27 | 2-D Dynamic Programming | 11 | Grid/string DP tables |
| 33-34 | Jun 28 - Jul 11 | Greedy | 8 | Local choice proves global optimum |
| 35 | Jul 12 - Jul 18 | Intervals | 6 | Sort by start, merge/sweep |
| 36-37 | Jul 19 - Aug 1 | Math & Geometry | 8 | Matrix rotation, number tricks |
| 38 | Aug 2 - Aug 8 | Bit Manipulation | 7 | XOR, shifts, masks |
| 39-40 | Aug 9 - Aug 22, 2027 | **Final review**: random cold re-solves, timed 45-min sessions | | Interview simulation |

Holiday weeks (Thanksgiving, Christmas/New Year) will probably be lighter. Log them as light weeks in `progress.md` and push the schedule instead of cramming.

Total: 150 problems, roughly 9 months at your pace. If you find a groove, compress it; DP and Graphs are where most people need extra weeks, so don't rush those.

### Milestone checks
- [ ] After Phase 0: 5 gate problems solved cold
- [ ] After Trees (wk 16): can explain DFS vs BFS and write both from memory
- [ ] After Graphs (wk 24): can name the right pattern within 2 min of reading a problem
- [ ] After DP (wk 32): can derive a recurrence from scratch on an unseen Medium
- [ ] Final: solve unseen Mediums in <= 30 min; then begin timed mock interviews (talk out loud)

### Interview-readiness add-ons (start around week 20)
- [ ] Practice narrating your approach out loud while solving
- [ ] Do 1 mock interview/month (Pramp, a friend, or have me play interviewer)
- [ ] After finishing the 150: Blind 75 gaps, then company-tagged Mediums, plus system design/behavioral prep as the interview date nears

---

## Tracker template

| # | Problem | Topic | Date | Solved cold? | Pattern | Time/Space | Review dates |
|---|---------|-------|------|--------------|---------|------------|--------------|
