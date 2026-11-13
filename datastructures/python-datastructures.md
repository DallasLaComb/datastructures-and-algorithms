# Python Data Structures Cheat Sheet (LeetCode Reference)

```python
from collections import deque, defaultdict, Counter, OrderedDict
from heapq import heapify, heappush, heappop, heappushpop, heapreplace, nlargest, nsmallest
from bisect import bisect_left, bisect_right, insort
from itertools import accumulate, combinations, permutations, product, groupby, chain, zip_longest
from functools import lru_cache, cache, reduce, cmp_to_key
from typing import List, Optional, Dict, Set, Tuple     # LeetCode type hints
import math
```

Syntax and API reference only. Mirrors `java-datastructures.md` section by section.

## Contents
1. [List as array](#1-list-as-array)
2. [String](#2-string)
3. [List](#3-list)
4. [Linked list (deque)](#4-linked-list-deque)
5. [Stack / deque](#5-stack--deque)
6. [Queue](#6-queue)
7. [Heap (heapq)](#7-heap-heapq)
8. [dict](#8-dict)
9. [Ordered dict](#9-ordered-dict)
10. [Sorted map (bisect / sortedcontainers)](#10-sorted-map-bisect--sortedcontainers)
11. [set / frozenset](#11-set--frozenset)
12. [Sorted set](#12-sorted-set)
13. [Iterators & generators](#13-iterators--generators)
14. [Built-in functions (Collections equivalent)](#14-built-in-functions-collections-equivalent)
15. [list / array helpers (Arrays equivalent)](#15-list--array-helpers-arrays-equivalent)
16. [math / int / str char helpers](#16-math--int--str-char-helpers)
17. [ListNode, TreeNode, dataclasses](#17-listnode-treenode-dataclasses)
18. [Comparators & sorting](#18-comparators--sorting)
19. [Bit sets / enums](#19-bit-sets--enums)
20. [Common conversions](#20-common-conversions)

---

## 1. List as array

Python has no fixed-size primitive arrays; a `list` is the array. `O(1)` access.

```python
a = [0] * 5                              # [0,0,0,0,0]
b = [1, 2, 3]
grid = [[0] * 4 for _ in range(3)]       # 3 rows, 4 cols
bad = [[0] * 4] * 3                      # WRONG: all rows are the SAME list
jag = [[] for _ in range(3)]             # jagged / adjacency list
lit = [[1, 2], [3, 4]]
s = [None] * 3
seen = [False] * n
cs = list("hello")                       # ['h','e','l','l','o']

a[0] = 10                                # set
x = a[0]                                 # get
x = a[-1]                                # last element
length = len(a)                          # function, not a method
rows, cols = len(grid), len(grid[0])
```

Typed arrays (rare): `import array; array.array('i', [1, 2, 3])`. NumPy is usually not available on LeetCode.

---

## 2. String

`str` is immutable. Build with a list and `"".join(...)` instead of repeated `+=`.

### str
```python
s = "hello"

len(s)                       # 5
s[1]                         # 'e'
s[-1]                        # 'o'
s[1:]                        # "ello"
s[1:3]                       # "el"  (end exclusive)
s[::-1]                      # "olleh" reversed
s[::2]                       # every 2nd char
s.find('l')                  # 2   (-1 if missing)
s.find("lo")                 # 3
s.find('l', 3)               # 3   (start from index)
s.index('l')                 # like find, but raises ValueError if missing
s.rfind('l')                 # 3
s.count('l')                 # 2   (non-overlapping)
"ell" in s                   # True
s.startswith("he")           # True  (accepts a tuple of prefixes)
s.endswith("lo")             # True
s == "hello"                 # True   (== compares content; `is` does not)
s.lower() == "HELLO".lower() # case-insensitive compare (or .casefold())
s < "help"                   # lexicographic compare
not s                        # True if empty
s.isspace()                  # only whitespace (False if empty)
s.upper(); s.lower(); s.title(); s.capitalize(); s.swapcase()
s.strip()                    # strip both ends (unicode ws); lstrip / rstrip
s.strip("xy")                # strip given chars
s.replace('l', 'L')          # all occurrences
s.replace("ll", "LL", 1)     # at most 1
re.sub(r"[aeiou]", "", s)    # regex (import re)
s.split(" ")                 # list[str]; "a  b".split(" ") -> ['a','','b']
s.split()                    # split on any whitespace run, drops empties
s.split(",", 1)              # at most 1 split
s.partition("=")             # (before, sep, after)
list(s)                      # list of chars
s * 3                        # "hellohellohello"
s + "!"                      # concat
hash(s)
"-".join(["a", "b"])         # "a-b"   (join takes any iterable of str)
",".join(map(str, nums))     # join ints
"%d-%s-%.2f" % (1, "x", 3.14159)
"{}-{}-{:.2f}".format(1, "x", 3.14159)
f"{1}-{'x'}-{3.14159:.2f}"   # f-string
f"{x:05d}  {x:b}  {x:x}  {x:>8}  {x:<8}"   # pad / binary / hex / align
str(123)                     # "123"
```

Char arithmetic (no char type; chars are length-1 strings):
```python
ord('a')                     # 97   (char -> int)
chr(97)                      # 'a'  (int -> char)
ord('c') - ord('a')          # 2
chr(ord('a') + 2)            # 'c'
```

### Building strings (StringBuilder equivalent)
```python
parts = []
parts.append("x"); parts.append(str(1)); parts.append('c')
parts.insert(0, "start")                # O(n)
parts[i]                                # get
parts[i] = 'z'                          # set
parts.pop(i)                            # delete at index
del parts[start:end]                    # delete range, end exclusive
parts[start:end] = ["str"]              # replace range
parts.reverse()                         # in place
len(parts)
result = "".join(parts)

chars = list("abc"); chars[1] = 'z'; "".join(chars)    # mutate-as-list pattern
```

---

## 3. List

Resizable array. `get/set` O(1), `append` amortized O(1), `insert/pop` in middle O(n).

```python
lst = []
lst2 = list(other)                       # copy (also other[:] or other.copy())
lst3 = [0] * 100                         # preallocated
imm = (1, 2, 3)                          # tuple: IMMUTABLE, hashable
nested = []
filled = [0] * n
sq = [i * i for i in range(n)]           # list comprehension
ev = [x for x in lst if x % 2 == 0]      # with filter
flat = [x for row in grid for x in row]  # flatten
```

### Set / Get
```python
lst.append(5)                 # append
lst.insert(0, 9)              # insert at index (shifts right)
lst[0] = 7                    # replace
lst[0]                        # read (IndexError if out of range)
len(lst)
```

### Other methods
```python
lst.extend(other)                     # append all (also lst += other)
lst[i:i] = other                      # insert all at index i
lst.pop()                             # remove & return LAST
lst.pop(0)                            # remove by INDEX (O(n)); use deque for queues
lst.remove(5)                         # remove first occurrence by VALUE (ValueError if missing)
del lst[0]                            # delete by index
del lst[1:3]                          # delete a slice
lst[:] = [x for x in lst if x % 2]    # removeIf (in place)
5 in lst                              # contains, O(n)
lst.index(5)                          # ValueError if missing
lst.count(5)
not lst                               # isEmpty
lst.clear()
lst[1:3]                              # COPY of [1,3)   (slices copy; no views)
lst.sort()                            # in place, stable
lst.sort(reverse=True)
lst.sort(key=lambda x: -x)
sorted(lst)                           # new list
lst.reverse()                         # in place
lst[::-1]                             # reversed copy
reversed(lst)                         # iterator
for i, x in enumerate(lst): ...
for a, b in zip(l1, l2): ...
lst == other                          # value equality
lst[-1]; lst[0]                       # first / last
```

Gotchas:
- `[[0] * m] * n` aliases rows; use a comprehension.
- Don't add/remove while iterating the same list; iterate a copy (`for x in lst[:]`) or rebuild.
- `a = b` aliases; `a = b[:]` / `list(b)` is a shallow copy; `copy.deepcopy(b)` for nested.
- `lst.sort()` returns `None`; `lst = lst.sort()` is a bug.
- Mutable default args: `def f(x, acc=[])` shares `acc` across calls.
- `lst.pop(0)` and `lst.insert(0, x)` are O(n).
- Unpacking: `a, b = b, a` swaps; `first, *rest = lst`; `*init, last = lst`.

---

## 4. Linked list (deque)

Python has no built-in linked list. `collections.deque` is a doubly-ended queue: O(1) at both ends, O(n) index in the middle. For node-based lists see [section 17](#17-listnode-treenode-dataclasses).

```python
dq = deque()
dq = deque([1, 2, 3])
dq = deque(maxlen=3)                      # bounded; oldest drops off the other end

dq.append(1)                              # tail
dq.appendleft(0)                          # head
dq.extend([4, 5]); dq.extendleft([9, 8])  # extendleft reverses the order
dq[0]; dq[-1]                             # peek (IndexError if empty); middle index O(n)
dq[0] = 9
dq.pop()                                  # remove tail (IndexError if empty)
dq.popleft()                              # remove head (IndexError if empty)
dq.insert(1, 5)                           # index insert
dq.remove(5)                              # by value, first occurrence
dq.index(5); dq.count(5)
5 in dq; len(dq); not dq; dq.clear()
dq.rotate(1)                              # right by 1; rotate(-1) is left
dq.reverse()
dq.copy()
reversed(dq)
```

---

## 5. Stack / deque

**Use a plain `list` as a stack.** `append`/`pop` at the end are O(1).

### list as a stack (LIFO)
```python
stack = []

stack.append(1)         # push
stack[-1]               # peek top (IndexError if empty)
stack.pop()             # pop top (IndexError if empty)
not stack               # isEmpty
len(stack)
1 in stack
stack.clear()
```
Iterating a list-stack goes bottom -> top (the reverse of Java's `ArrayDeque` stack).

### deque as a queue (FIFO) / double-ended
```python
dq = deque()
dq.appendleft(1)        # addFirst
dq.append(2)            # addLast
dq.popleft()            # removeFirst
dq.pop()                # removeLast
dq[0]                   # peekFirst   (check `if dq` first; no null-returning peek)
dq[-1]                  # peekLast
len(dq); bool(dq); x in dq; dq.clear()
iter(dq); reversed(dq)
dq.remove(x)
```
No `None`-on-empty variants: guard with `if dq:`, or `dq[0] if dq else None`.

### Monotonic stack idiom
```python
st = []
for i, x in enumerate(nums):
    while st and nums[st[-1]] < x:
        j = st.pop()
    st.append(i)
```

---

## 6. Queue

```python
q = deque()                 # preferred: O(1) popleft

q.append(1)                 # enqueue
q.popleft()                 # dequeue; IndexError if empty
q[0]                        # front; IndexError if empty
len(q)
not q                       # isEmpty
1 in q
q.clear()
```

Thread-safe / blocking variants (rarely needed on LeetCode):
```python
from queue import Queue, LifoQueue, PriorityQueue
sq = Queue(); sq.put(1); sq.get(); sq.qsize(); sq.empty()
```
Do not use `list.pop(0)` for BFS; it is O(n).

---

## 7. Heap (heapq)

`heapq` operates on a plain list. **Min-heap only.** `heappush/heappop` O(log n), `h[0]` peek O(1), `heapify` O(n). Iteration order is NOT sorted.

```python
h = []
heapify(lst)                          # in place, O(n)

heappush(h, x)                        # insert
h[0]                                  # peek min (IndexError if empty)
heappop(h)                            # remove & return min (IndexError if empty)
heappushpop(h, x)                     # push then pop (faster than separate)
heapreplace(h, x)                     # pop then push
len(h); not h
nlargest(k, lst); nsmallest(k, lst)
nlargest(k, lst, key=lambda x: x[1])
```

Max-heap: negate the values.
```python
maxh = []
heappush(maxh, -x)
top = -maxh[0]
top = -heappop(maxh)
```

Tuples compare left to right, so use them for priorities:
```python
pq = []
heappush(pq, (dist, node))            # by first element
heappush(pq, (-count, word))          # max count, then min word alphabetically
heappush(pq, (dist, i, node))         # tie-break with a counter if node isn't comparable
d, node = heappop(pq)
```

Remove an arbitrary element: no efficient op. Use lazy deletion (skip stale entries on pop) or `h.remove(x); heapify(h)` O(n).

Custom ordering for objects: define `__lt__`.
```python
class Task:
    def __init__(self, p, name): self.p, self.name = p, name
    def __lt__(self, other): return self.p < other.p
```

---

## 8. dict

Key -> value, insertion-ordered (3.7+), O(1) average. Keys must be hashable (`int`, `str`, `tuple`, `frozenset`; NOT `list`, `set`, `dict`).

```python
d = {}
d = {"a": 1, "b": 2}
d = dict(a=1, b=2)
copy = dict(d)                          # or d.copy(), {**d}
d = dict(zip(keys, vals))
d = {k: v for k, v in pairs}            # dict comprehension
d = dict.fromkeys(keys, 0)              # all values 0 (don't use a mutable value)
multi = defaultdict(list)               # missing key -> []
freq = defaultdict(int)                 # missing key -> 0
freq = Counter(iterable)                # counts
merged = d1 | d2                        # 3.9+ (right wins)
```

### Set / Get
```python
d["a"] = 1                              # set
d["a"]                                  # get; KeyError if missing
d.get("a")                              # None if missing
d.get("a", 0)                           # with fallback
len(d)
not d                                   # isEmpty
```

### Built-in tools
```python
"a" in d                                # containsKey, O(1)
1 in d.values()                         # containsValue, O(n)
d.pop("a")                              # remove; KeyError if missing
d.pop("a", None)                        # remove; default if missing
del d["a"]
d.popitem()                             # remove & return LAST inserted (k, v)
d.clear()

d.setdefault("a", 1)                    # insert if missing; returns the value
d.update(other)                         # putAll (also d |= other)
d.setdefault(k, []).append(x)           # computeIfAbsent idiom

d[k] = d.get(k, 0) + 1                  # merge / count idiom
freq[k] += 1                            # with defaultdict(int) or Counter
d = {k: v * 2 for k, v in d.items()}    # replaceAll

d.keys()                                # view
d.values()                              # view
d.items()                               # view of (k, v)
d == other
```

### Iteration
```python
for k in d: ...                         # keys
for k in d.keys(): ...
for v in d.values(): ...
for k, v in d.items(): ...
for i, (k, v) in enumerate(d.items()): ...
```
Do not add/remove keys while iterating (`RuntimeError`); iterate `list(d.items())`. Changing a value of an existing key is fine.

### Counter
```python
c = Counter("aabbbc")                   # Counter({'b':3,'a':2,'c':1})
c["z"]                                  # 0 for missing (no KeyError, doesn't insert)
c.most_common(2)                        # [('b',3),('a',2)]
c.update("abc"); c.subtract("abc")
c1 + c2; c1 - c2; c1 & c2; c1 | c2      # +, -(drops <=0), min, max
c.total()                               # 3.10+
c.elements()                            # iterator, repeats each key count times
c1 == c2                                # anagram check
```

### Sorting entries
```python
es = sorted(d.items(), key=lambda kv: kv[1], reverse=True)    # by value desc
es = sorted(d.items(), key=lambda kv: (-kv[1], kv[0]))        # value desc, key asc
es = sorted(d.items())                                        # by key
by_val = dict(sorted(d.items(), key=lambda kv: kv[1]))
max(d, key=d.get)                                             # key with largest value
```

Gotchas:
- `d[k]` raises `KeyError` if missing; use `.get`, `in`, or `defaultdict`.
- A `defaultdict` inserts the key on a read (`d[k]` creates it); `k in d` doesn't.
- Lists are unhashable: use `tuple(lst)` as a key. Tuples, strings, frozensets are value-based.
- Don't mutate a key object after insertion.
- `1 == 1.0 == True` hash the same, so they collide as keys.

---

## 9. Ordered dict

Plain `dict` already keeps insertion order. `OrderedDict` adds reordering ops (useful for LRU).

```python
od = OrderedDict()
od["a"] = 1
od.move_to_end("a")                     # to the end (most recent)
od.move_to_end("a", last=False)         # to the front
od.popitem(last=True)                   # pop newest (default)
od.popitem(last=False)                  # pop OLDEST (FIFO)
next(iter(od))                          # oldest key
next(reversed(od))                      # newest key
```

LRU cache pattern:
```python
class LRU:
    def __init__(self, cap): self.cap, self.od = cap, OrderedDict()
    def get(self, k):
        if k not in self.od: return -1
        self.od.move_to_end(k)
        return self.od[k]
    def put(self, k, v):
        self.od[k] = v
        self.od.move_to_end(k)
        if len(self.od) > self.cap:
            self.od.popitem(last=False)
```

Memoization (access-order caching for functions):
```python
@lru_cache(maxsize=None)                # or @cache (3.9+)
def f(i, j): ...
f.cache_clear()
```
Arguments must be hashable (use tuples, not lists).

Plain dict first/last (3.7+): `next(iter(d))`, `next(reversed(d))` (3.8+), `d.popitem()` pops the last.

---

## 10. Sorted map (bisect / sortedcontainers)

No built-in `TreeMap`. Two options: `bisect` on sorted lists (stdlib) or `sortedcontainers` (third-party, not always available).

### bisect on a sorted list
```python
keys = [1, 3, 5, 7]

bisect_left(keys, x)        # first index with keys[i] >= x   (insertion point, left)
bisect_right(keys, x)       # first index with keys[i] >  x   (alias: bisect)
insort(keys, x)             # insert keeping order, O(n)
bisect_left(keys, x, lo, hi)
bisect_left(keys, x, key=f) # 3.10+: key function applied to elements

i = bisect_left(keys, x)
floor   = keys[bisect_right(keys, x) - 1] if bisect_right(keys, x) > 0 else None   # greatest <= x
ceiling = keys[bisect_left(keys, x)]      if bisect_left(keys, x) < len(keys) else None   # smallest >= x
lower   = keys[bisect_left(keys, x) - 1]  if bisect_left(keys, x) > 0 else None    # greatest <  x
higher  = keys[bisect_right(keys, x)]     if bisect_right(keys, x) < len(keys) else None   # smallest >  x

keys[0]; keys[-1]           # first / last key
keys.pop(0); keys.pop()     # pollFirst O(n) / pollLast O(1)
keys[bisect_left(keys, a):bisect_left(keys, b)]    # subMap [a, b)
```
`insort` and `pop(0)` are O(n) (memmove, fast in practice for n up to ~1e5).

### sortedcontainers (if available)
```python
from sortedcontainers import SortedList, SortedDict, SortedSet

sd = SortedDict()                       # sorted by key, O(log n)
sd[3] = "c"
sd.peekitem(0)                          # first (k, v)
sd.peekitem(-1)                         # last (k, v)
sd.popitem(0); sd.popitem(-1)           # pollFirst / pollLast
sd.bisect_left(k); sd.bisect_right(k)   # index of key positions
list(sd.irange(minimum=a, maximum=b))   # keys in [a, b]
sd.keys()[0]; sd.keys()[-1]
SortedDict(lambda k: -k)                # custom key (descending)
```

---

## 11. set / frozenset

Unique hashable elements. O(1) average. Unordered (no guaranteed order).

```python
s = set()                                # NOT {}  ({} is an empty dict)
s = {1, 2, 3}
from_list = set(lst)
from_str = set("hello")
comp = {x * x for x in lst}              # set comprehension
fs = frozenset([1, 2, 3])                # IMMUTABLE, hashable (usable as dict key / set element)
set_of_lists = set()                     # lists are unhashable -> store tuples
set_of_lists.add(tuple(lst))

s.add(5)                # no return value (check `5 in s` first if you need to know)
5 in s                  # contains
s.remove(5)             # KeyError if missing
s.discard(5)            # no error if missing
s.pop()                 # remove & return an ARBITRARY element
len(s)
not s                   # isEmpty
s.clear()

s | other   ; s.union(other)                  # union
s & other   ; s.intersection(other)           # intersection
s - other   ; s.difference(other)             # difference
s ^ other   ; s.symmetric_difference(other)
s <= other  ; s.issubset(other)               # subset
s >= other  ; s.issuperset(other)
s.isdisjoint(other)

s |= other ; s.update(other)                  # in-place union
s &= other ; s.intersection_update(other)     # in-place intersection (retainAll)
s -= other ; s.difference_update(other)       # in-place difference (removeAll)

s = {x for x in s if x >= 0}                  # removeIf
for x in s: ...
sorted(s)                                     # -> sorted list
```

Insertion-ordered unique: `list(dict.fromkeys(lst))` or use a `dict` with dummy values.

---

## 12. Sorted set

No built-in `TreeSet`. Use a sorted list with `bisect`, or `SortedList` / `SortedSet`.

```python
# sorted list + bisect
ts = []
i = bisect_left(ts, x)
if i == len(ts) or ts[i] != x: ts.insert(i, x)       # add unique
i = bisect_left(ts, x)
if i < len(ts) and ts[i] == x: ts.pop(i)             # remove
i < len(ts) and ts[i] == x                           # contains

ts[0]; ts[-1]                                        # first / last
ts.pop(0); ts.pop()                                  # pollFirst / pollLast

# floor / ceiling / lower / higher: see section 10 (same bisect math)
ts[bisect_left(ts, a):bisect_right(ts, b)]           # elements in [a, b]
```

```python
from sortedcontainers import SortedList, SortedSet
sl = SortedList([3, 1, 2])              # allows duplicates; O(log n) add/remove
sl.add(5); sl.remove(5); sl.discard(5); sl.pop(); sl.pop(0)
sl[0]; sl[-1]                           # first / last
sl.bisect_left(x); sl.bisect_right(x)
sl.index(x); sl.count(x)
list(sl.irange(a, b))                   # values in [a, b]
SortedList(key=lambda x: -x)            # custom order
SortedSet([3, 1, 2])                    # unique version
```
`sortedcontainers` is NOT stdlib; LeetCode does provide it, many other judges don't.

---

## 13. Iterators & generators

```python
it = iter(lst)
next(it)                    # raises StopIteration at the end
next(it, None)              # default instead of raising
for x in it: ...            # consumes the rest

for i, x in enumerate(lst, start=1): ...
for a, b in zip(l1, l2): ...                 # stops at the shortest
for a, b in zip_longest(l1, l2, fillvalue=0): ...
for x in reversed(lst): ...
for i in range(n - 1, -1, -1): ...           # backwards by index
for a, b in zip(lst, lst[1:]): ...           # adjacent pairs

# remove while iterating: build a new list or iterate a copy
lst = [x for x in lst if x % 2]
for x in lst[:]:
    if x % 2 == 0: lst.remove(x)

# remove from dict while iterating
for k in list(d):
    if cond(k): del d[k]

# generator
def gen(n):
    for i in range(n):
        yield i
g = (x * x for x in range(n))                # generator expression (lazy)
sum(x for x in lst)                          # no brackets needed

# peek / walrus
while (line := next(it, None)) is not None: ...
```

---

## 14. Built-in functions (Collections equivalent)

```python
lst.sort()                                   # in place, stable
sorted(lst, key=..., reverse=True)           # new list
lst.reverse()
random.shuffle(lst)                          # import random
lst[i], lst[j] = lst[j], lst[i]              # swap
max(lst); min(lst)
max(lst, key=f); min(lst, key=f)
max(lst, default=0)                          # avoid ValueError on empty
max(a, b, c)
lst.count(x)                                 # frequency
bisect_left(sorted_lst, key)                 # binary search
sorted(set(lst))                             # unique sorted
tuple(lst)                                   # immutable view-ish copy
[]; {}; set()                                # empty collections
[x] ; (x,)                                   # singleton (note the comma)
[x] * n                                      # n copies
lst[:] = [x] * len(lst)                      # fill in place
lst.extend([a, b, c])                        # addAll
lst[k:] + lst[:k]                            # rotate left by k
lst[-k:] + lst[:-k]                          # rotate right by k
set(a).isdisjoint(b)                         # disjoint
sum(lst); sum(lst, start)
any(x > 0 for x in lst); all(x > 0 for x in lst)
len(lst); sorted(lst)[len(lst) // 2]
list(map(f, lst)); list(filter(f, lst))
reduce(lambda a, b: a * b, lst, 1)
```

---

## 15. list / array helpers (Arrays equivalent)

```python
print(arr)                                   # [1, 2, 3]
print(*arr)                                  # 1 2 3
for row in grid: print(row)                  # 2D
arr.sort()                                   # in place
arr[from_:to] = sorted(arr[from_:to])        # sort a range
lst.sort(key=cmp_to_key(compare))            # comparator function (see section 18)
intervals.sort(key=lambda x: x[0])
intervals.sort(key=lambda x: (x[0], -x[1]))  # multi-key
arr.sort(reverse=True)
arr = [-1] * n                               # fill
arr[a:b] = [val] * (b - a)                   # fill range
grid = [[-1] * m for _ in range(n)]          # fill 2D
arr[:new_len] + [0] * max(0, new_len - len(arr))   # copyOf (truncate / pad)
arr[from_:to]                                # copyOfRange [from, to)
arr[:]; arr.copy(); list(arr)                # shallow copy (2D: rows still shared!)
copy.deepcopy(grid)                          # deep copy (import copy)
[row[:] for row in grid]                     # deep copy of a 2D grid
a == b                                       # list equality (works for nested)
hash(tuple(arr))
bisect_left(sorted_arr, key)                 # see section 10
list(range(n)); list(range(a, b)); list(range(a, b, step))
sum(arr); max(arr); min(arr); sum(arr) / len(arr)
list(accumulate(arr))                        # prefix sums
list(accumulate(arr, initial=0))             # prefix sums with leading 0 (len n+1)
list(accumulate(arr, max))                   # running max
arr = [i * i for i in range(n)]              # setAll
next((i for i, (x, y) in enumerate(zip(a, b)) if x != y), -1)    # mismatch
(a > b) - (a < b)                            # compare -> -1, 0, 1
list(zip(*grid))                             # transpose
[list(r) for r in zip(*grid[::-1])]          # rotate 90 deg clockwise
```

Sort `list` descending: `sorted(a, reverse=True)` or `a.sort(reverse=True)`.

Itertools:
```python
list(combinations(lst, 2))                   # unordered pairs
list(permutations(lst))                      # all orderings
list(permutations(lst, 2))
list(product([0, 1], repeat=3))              # cartesian product
list(chain(a, b)); list(chain.from_iterable(nested))
for key, grp in groupby(sorted_lst): ...     # runs of equal items (input must be sorted by key)
list(accumulate(arr))
```

---

## 16. math / int / str char helpers

```python
# Limits
float('inf'); float('-inf')                  # idiomatic infinity
math.inf; -math.inf
sys.maxsize                                  # 2**63-1 (import sys)
# Python ints are ARBITRARY PRECISION: no overflow.
# Emulate 32-bit: INT_MAX = 2**31 - 1; INT_MIN = -2**31

# Math
max(a, b); min(a, b); abs(x)
a ** b; pow(a, b); pow(a, b, mod)            # pow with mod is fast modular exponentiation
math.sqrt(x)                                 # float
math.isqrt(x)                                # exact integer sqrt
math.floor(x); math.ceil(x)                  # return int
round(x)                                     # banker's rounding: round(2.5) == 2
a // b                                       # floor division (floors toward -inf)
a % b                                        # result has the sign of the DIVISOR (never negative for b > 0)
divmod(a, b)                                 # (a // b, a % b)
int(a / b)                                   # truncate toward zero (Java-style division)
math.fmod(a, b)                              # C-style remainder (sign of dividend)
math.gcd(a, b); math.lcm(a, b)               # lcm 3.9+
math.factorial(n); math.comb(n, k); math.perm(n, k)
math.log(x); math.log2(x); math.log10(x); math.hypot(a, b)
math.cbrt(x)                                 # 3.11+
math.isclose(a, b)
math.prod(lst)                               # product

# int
int("123"); int("-5"); int(" 7 ")            # parse (ValueError on bad input)
str(5)
bin(5)                       # '0b101'
bin(5)[2:]                   # '101'
format(5, 'b')               # '101'   (also '08b' zero-padded)
hex(255); format(255, 'x')   # '0xff' / 'ff'
oct(8)
int("101", 2)                # parse in base 2
int("ff", 16)
x.bit_count()                # popcount (3.10+); or bin(x).count('1')
x.bit_length()               # highest set bit position + 1
x & -x                       # lowest set bit
x & (x - 1)                  # clear lowest set bit
(x >> i) & 1                 # test bit i
x | (1 << i); x & ~(1 << i); x ^ (1 << i)    # set / clear / toggle bit i
(a > b) - (a < b)            # compare / signum
int(str(x)[::-1])            # reverse digits
sum(map(int, str(x)))        # digit sum

# str char helpers
c.isdigit(); c.isalpha(); c.isalnum()
c.isupper(); c.islower(); c.isspace()
c.upper(); c.lower()
int('7')                     # digit char -> int
ord(c) - ord('0')            # digit char -> int
chr(d + ord('0'))            # int -> digit char
str(d)
ord(c) - ord('a')            # alphabet index 0..25
chr(i + ord('a'))
string.ascii_lowercase       # 'abc...z'  (import string)
string.ascii_uppercase; string.digits

# Bit operators
x & y;  x | y;  x ^ y;  ~x
x << 1; x >> 1                # >> is arithmetic (sign-preserving); no >>> operator
(x & 0xFFFFFFFF)              # emulate unsigned 32-bit
```

---

## 17. ListNode, TreeNode, dataclasses

LeetCode supplies `ListNode` and `TreeNode`; shown here for syntax.

### ListNode
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

n = ListNode(5)
n.next = ListNode(6)
n.val; n.next
dummy = ListNode(0, head)             # dummy head idiom
```

### TreeNode
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```

### Pair / named tuple / dataclass
```python
p = (1, 2)                            # tuple: immutable, hashable, ordered compare
a, b = p
p[0]; p[1]

from collections import namedtuple
Pair = namedtuple("Pair", ["a", "b"]) # auto __eq__/__hash__/__repr__
p = Pair(1, 2); p.a; p.b

from dataclasses import dataclass
@dataclass(frozen=True)               # frozen -> hashable, immutable
class Pt:
    x: int
    y: int
@dataclass(order=True)                # generates comparison methods (field order)
class Task:
    priority: int
    name: str = ""

class Node:
    __slots__ = ("val", "next")       # less memory, faster attribute access
```

---

## 18. Comparators & sorting

Python sorts with a **key function**, not a comparator. `cmp_to_key` converts an old-style comparator.

```python
# key functions
lst.sort(key=lambda x: x[0])                    # by first
lst.sort(key=lambda x: (x[0], x[1]))            # by first, then second
lst.sort(key=lambda x: (x[0], -x[1]))           # first asc, second desc
lst.sort(key=lambda x: -x)                      # descending numbers
lst.sort(key=len)                               # by length
lst.sort(key=lambda s: (len(s), s))             # length, then alphabetical
lst.sort(key=str.lower)                         # case-insensitive
lst.sort(key=lambda p: p.age)
lst.sort(key=lambda p: p.age, reverse=True)
from operator import itemgetter, attrgetter
lst.sort(key=itemgetter(1))                     # by index/key 1
lst.sort(key=itemgetter(0, 1))
lst.sort(key=attrgetter("age"))

# comparator (rarely needed)
def compare(a, b):
    return -1 if a + b > b + a else 1           # e.g. "largest number" problem
lst.sort(key=cmp_to_key(compare))

# Examples
intervals.sort(key=lambda x: x[0])                          # list of [start, end]
words.sort(key=lambda w: (-cnt[w], w))                      # freq desc, then alpha
cs = sorted(s)                                              # list of sorted chars
sorted_s = "".join(sorted(s))                               # sort chars of a string

# Custom class
class Person:
    def __init__(self, age): self.age = age
    def __lt__(self, o): return self.age < o.age            # sort() & heapq only need __lt__
from functools import total_ordering                        # fill in the rest from __eq__ + one other
```
`list.sort()` and `sorted()` are stable (Timsort), so multi-pass sorts work: sort by the secondary key first, then the primary.
Descending on non-numeric keys: negate is impossible for strings, so sort twice (stable) or use `cmp_to_key`.
Mixing types in a key (`int` vs `None`) raises `TypeError`.

---

## 19. Bit sets / enums

```python
# int as a bitmask (arbitrary width)
mask = 0
mask |= 1 << i                  # set(i)
mask &= ~(1 << i)               # clear(i)
mask ^= 1 << i                  # flip(i)
(mask >> i) & 1                 # get(i)
mask.bit_count()                # cardinality
mask & -mask                    # lowest set bit
(mask & -mask).bit_length() - 1 # index of lowest set bit
mask.bit_length()               # length
a & b; a | b; a ^ b; a & ~b     # and / or / xor / andNot
full = (1 << n) - 1             # all n bits set
sub = mask
while sub:                      # iterate non-empty submasks
    sub = (sub - 1) & mask

# bool list as BitSet
bits = [False] * n

# enum
from enum import Enum, IntEnum, auto
class Color(Enum):
    RED = 1
    GREEN = 2
Color.RED; Color.RED.name; Color.RED.value; Color(1); list(Color)
em = {Color.RED: 0}             # enums are hashable -> use a dict as EnumMap
es = {Color.RED}                # set as EnumSet
```

Others you may see: `array.array` (typed compact arrays), `queue.Queue` (thread-safe), `collections.ChainMap`, `types.MappingProxyType` (read-only dict), `weakref.WeakValueDictionary`, `threading.Lock` (concurrency / special cases).

---

## 20. Common conversions

```python
list -> list copy:         lst[:]  /  list(lst)
int list -> str list:      list(map(str, a))   /   [str(x) for x in a]
str list -> int list:      list(map(int, a))   /   [int(x) for x in a]
str -> list of chars:      list(s)
list of chars -> str:      "".join(cs)
str -> list of ints:       [int(c) for c in s]     /   list(map(int, s))
list of lists -> list of tuples:   [tuple(r) for r in grid]     # hashable rows
tuple -> list:             list(t)
list -> tuple:             tuple(lst)
set -> list:               list(s)     /   sorted(s)
list -> set:               set(lst)
dict keys -> list:         list(d)     /   list(d.keys())
dict values -> list:       list(d.values())
dict items -> list:        list(d.items())
two lists -> dict:         dict(zip(keys, vals))
dict -> sorted list of pairs:   sorted(d.items())
dict swap key/value:       {v: k for k, v in d.items()}
list of pairs -> dict:     dict(pairs)
str -> int:                int(s)         int(s, base)
int -> str:                str(n)
int -> list of digits:     [int(c) for c in str(n)]
list of digits -> int:     int("".join(map(str, digits)))
int -> binary str:         bin(n)[2:]   /   format(n, 'b')
list -> deque:             deque(lst)
list -> heap:              heapify(lst)   # in place
iterator/generator -> list: list(gen)
flatten 2D:                [x for row in grid for x in row]   /   list(chain.from_iterable(grid))
list of lists copy:        [row[:] for row in grid]
```
