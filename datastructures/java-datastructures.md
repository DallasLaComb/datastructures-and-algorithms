# Java Data Structures Cheat Sheet (LeetCode Reference)

```java
import java.util.*;            // covers almost everything below
import java.util.stream.*;     // only if you use streams
```

Syntax and API reference only.

## Contents
1. [Array](#1-array)
2. [String / StringBuilder](#2-string--stringbuilder)
3. [ArrayList](#3-arraylist)
4. [LinkedList](#4-linkedlist)
5. [Stack / Deque / ArrayDeque](#5-stack--deque--arraydeque)
6. [Queue](#6-queue)
7. [PriorityQueue (Heap)](#7-priorityqueue-heap)
8. [HashMap](#8-hashmap)
9. [LinkedHashMap](#9-linkedhashmap)
10. [TreeMap](#10-treemap)
11. [HashSet / LinkedHashSet](#11-hashset--linkedhashset)
12. [TreeSet](#12-treeset)
13. [Iterator](#13-iterator)
14. [Collections utility class](#14-collections-utility-class)
15. [Arrays utility class](#15-arrays-utility-class)
16. [Math / Integer / Character helpers](#16-math--integer--character-helpers)
17. [ListNode, TreeNode, records](#17-listnode-treenode-records)
18. [Comparators & sorting](#18-comparators--sorting)
19. [BitSet / EnumMap](#19-bitset--enummap)
20. [Common conversions](#20-common-conversions)

---

## 1. Array

Fixed size, primitives or objects. `O(1)` access.

```java
int[] a = new int[5];                    // [0,0,0,0,0]
int[] b = {1, 2, 3};
int[] c = new int[]{1, 2, 3};
int[][] grid = new int[3][4];            // 3 rows, 4 cols
int[][] jag = new int[3][];              // jagged; each row set later
int[][] lit = {{1,2},{3,4}};
String[] s = new String[3];              // nulls
boolean[] seen = new boolean[n];         // false
char[] cs = "hello".toCharArray();

a[0] = 10;                               // set
int x = a[0];                            // get
int len = a.length;                      // length (field, no parens!)
int rows = grid.length, cols = grid[0].length;
```

Handy (`Arrays` class, see [section 15](#15-arrays-utility-class)): `Arrays.fill`, `Arrays.sort`, `Arrays.toString`, `Arrays.copyOf`, `Arrays.asList`, ...

---

## 2. String / StringBuilder

`String` is immutable. Use `StringBuilder` for repeated modification.

### String
```java
String s = "hello";
String t = new String("hello");

s.length();                  // 5
s.charAt(1);                 // 'e'
s.substring(1);              // "ello"
s.substring(1, 3);           // "el"  (end exclusive)
s.indexOf('l');              // 2   (-1 if missing)
s.indexOf("lo");             // 3
s.indexOf('l', 3);           // 3   (start from index)
s.lastIndexOf('l');          // 3
s.contains("ell");           // true
s.startsWith("he");          // true
s.endsWith("lo");            // true
s.equals("hello");           // true   (NEVER use == for content)
s.equalsIgnoreCase("HELLO"); // true
s.compareTo("help");         // <0, 0, >0 lexicographic
s.isEmpty();                 // length()==0
s.isBlank();                 // empty or only whitespace
s.toUpperCase(); s.toLowerCase();
s.trim();                    // strip ends (ASCII ws)
s.strip();                   // unicode-aware trim
s.replace('l', 'L');         // all occurrences (char or CharSequence)
s.replace("ll", "LL");
s.replaceAll("[aeiou]", ""); // regex
s.split(" ");                // String[]
s.split("\\s+");             // regex split on whitespace
s.split(",", -1);            // keep trailing empty strings
s.toCharArray();             // char[]
s.chars();                   // IntStream of chars
s.repeat(3);                 // "hellohellohello"
s.concat("!");               // or s + "!"
s.hashCode();
s.intern();
String.valueOf(123);         // "123"   (also char[], char, boolean...)
String.valueOf(charArr);     // from char[]
new String(charArr);         // from char[]
new String(charArr, 0, 3);   // from char[] range
String.join(",", list);      // join Iterable / varargs
String.join("-", "a", "b");  // "a-b"
String.format("%d-%s-%.2f", 1, "x", 3.14159);
"a,b".compareToIgnoreCase("A,B");
```

Char arithmetic:
```java
char c = 'a';
c - 'a';                     // 0   (char -> int)
(char)('a' + 2);             // 'c' (int -> char)
```

### StringBuilder
```java
StringBuilder sb = new StringBuilder();
StringBuilder sb2 = new StringBuilder("abc");

sb.append("x").append(1).append('c');   // chainable; any type
sb.insert(0, "start");                  // insert at index
sb.charAt(i);                           // get
sb.setCharAt(i, 'z');                   // set
sb.deleteCharAt(i);
sb.delete(start, end);                  // end exclusive
sb.replace(start, end, "str");
sb.reverse();                           // in place
sb.length();
sb.setLength(n);                        // truncate/extend to length n
sb.indexOf("x");
sb.toString();
sb.isEmpty();
sb.compareTo(other);
```

---

## 3. ArrayList

Resizable array. `get/set` O(1), `add` end amortized O(1), `add/remove` middle O(n).

```java
List<Integer> list = new ArrayList<>();
List<Integer> list2 = new ArrayList<>(otherCollection);   // copy
List<Integer> list3 = new ArrayList<>(100);               // initial capacity
List<Integer> imm = List.of(1, 2, 3);                     // IMMUTABLE
List<Integer> fixed = Arrays.asList(1, 2, 3);             // fixed-size (set ok, add/remove throws)
List<List<Integer>> nested = new ArrayList<>();
List<Integer> filled = new ArrayList<>(Collections.nCopies(n, 0));
```

### Set / Get
```java
list.add(5);                  // append
list.add(0, 9);               // insert at index (shifts right)
list.set(0, 7);               // replace, returns old value
list.get(0);                  // read
list.size();
```

### Other methods
```java
list.addAll(otherList);               // append all
list.addAll(index, otherList);
list.remove(0);                       // by INDEX (int)  -> returns element
list.remove(Integer.valueOf(5));      // by VALUE (first occurrence) -> boolean
list.removeIf(x -> x % 2 == 0);
list.removeAll(otherList);
list.retainAll(otherList);            // intersection
list.contains(5);
list.indexOf(5);                      // -1 if missing
list.lastIndexOf(5);
list.isEmpty();
list.clear();
list.subList(1, 3);                   // VIEW [1,3) (changes write through)
new ArrayList<>(list.subList(1, 3));  // independent copy
list.sort(null);                      // natural order
list.sort(Comparator.reverseOrder());
list.sort((a, b) -> a - b);
list.forEach(x -> System.out.println(x));
list.replaceAll(x -> x * 2);
list.toArray(new Integer[0]);         // -> Integer[]
list.stream().mapToInt(Integer::intValue).toArray();   // -> int[]
list.equals(other);
list.hashCode();
list.iterator(); list.listIterator();
list.reversed();                      // Java 21+
list.getFirst(); list.getLast();      // Java 21+
list.removeFirst(); list.removeLast();// Java 21+
```

Gotchas:
- `remove(int)` vs `remove(Object)`: `list.remove(1)` removes **index** 1.
- Don't modify while iterating with for-each (`ConcurrentModificationException`). Use `Iterator.remove()` or `removeIf`.
- Adding a list to another list stores a reference; `new ArrayList<>(x)` makes a copy.

Convert `int[]` <-> `List<Integer>`:
```java
List<Integer> l = Arrays.stream(arr).boxed().collect(Collectors.toList());
int[] arr = l.stream().mapToInt(i -> i).toArray();
```

---

## 4. LinkedList

Doubly linked list; implements both `List` and `Deque`. Index access O(n); add/remove at ends O(1).

```java
LinkedList<Integer> ll = new LinkedList<>();
Deque<Integer> dq = new LinkedList<>();   // allows null (ArrayDeque doesn't)

ll.add(1);              ll.addLast(1);   // tail
ll.addFirst(0);         ll.push(0);      // head
ll.add(1, 5);                            // index insert
ll.get(0);                               // O(n)
ll.set(0, 9);
ll.getFirst();   ll.getLast();           // throws if empty
ll.peekFirst();  ll.peekLast();  ll.peek();  // null if empty
ll.removeFirst(); ll.removeLast(); ll.remove(); ll.pop();  // throws if empty
ll.pollFirst();   ll.pollLast();   ll.poll();              // null if empty
ll.offer(1); ll.offerFirst(1); ll.offerLast(1);
ll.remove(Integer.valueOf(5));           // by value
ll.removeFirstOccurrence(5); ll.removeLastOccurrence(5);
ll.contains(5); ll.indexOf(5); ll.size(); ll.isEmpty(); ll.clear();
ll.descendingIterator();
```
(Also all `List` methods from ArrayList.)

---

## 5. Stack / Deque / ArrayDeque

**Prefer `ArrayDeque` over `Stack`** (`Stack` is legacy/synchronized/slow).

### ArrayDeque as a stack (LIFO)
```java
Deque<Integer> stack = new ArrayDeque<>();

stack.push(1);          // add to FRONT (top)
stack.peek();           // look at top, null if empty
stack.pop();            // remove top, throws NoSuchElementException if empty
stack.isEmpty();
stack.size();
stack.contains(1);
stack.clear();
```
Iterating a Deque used as a stack goes top -> bottom.

### ArrayDeque as a queue (FIFO) / full Deque API
```java
Deque<Integer> dq = new ArrayDeque<>();
// Insert                      throws         returns special value
dq.addFirst(1);   dq.offerFirst(1);   // offerFirst returns boolean
dq.addLast(2);    dq.offerLast(2);
// Remove
dq.removeFirst(); dq.pollFirst();     // poll -> null if empty
dq.removeLast();  dq.pollLast();
// Examine
dq.getFirst();    dq.peekFirst();     // peek -> null if empty
dq.getLast();     dq.peekLast();
dq.size(); dq.isEmpty(); dq.contains(x); dq.clear();
dq.iterator(); dq.descendingIterator();
dq.removeFirstOccurrence(x); dq.removeLastOccurrence(x);
```
**ArrayDeque does NOT allow `null` elements.**

### Legacy Stack (avoid if possible)
```java
Stack<Integer> st = new Stack<>();
st.push(1); st.pop(); st.peek(); st.empty(); st.isEmpty(); st.search(1); st.size();
st.get(i);   // it's a Vector, so index access works
```
`Stack.pop()/peek()` throw `EmptyStackException` when empty.

---

## 6. Queue

```java
Queue<Integer> q = new LinkedList<>();     // or ArrayDeque (faster, no nulls)
Queue<Integer> q2 = new ArrayDeque<>();

q.offer(1);       // enqueue (returns boolean)    ~ q.add(1) throws if capacity-bound
q.poll();         // dequeue; null if empty       ~ q.remove() throws if empty
q.peek();         // front, null if empty         ~ q.element() throws if empty
q.size();
q.isEmpty();
q.contains(1);
q.clear();
```


---

## 7. PriorityQueue (Heap)

Binary heap. **Min-heap by default.** `offer/poll` O(log n), `peek` O(1), `remove(Object)` O(n). Iteration order is NOT sorted.

```java
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
PriorityQueue<Integer> maxHeap2 = new PriorityQueue<>((a, b) -> b - a);   // beware overflow; prefer Integer.compare(b, a)
PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);     // by first element
PriorityQueue<int[]> pq2 = new PriorityQueue<>(Comparator.comparingInt(a -> a[1]));
PriorityQueue<Integer> pq3 = new PriorityQueue<>(initialCapacity, comparator);
PriorityQueue<Integer> fromColl = new PriorityQueue<>(list);              // heapify

pq.offer(x);      pq.add(x);        // insert
pq.peek();                          // min (or null)
pq.poll();                          // remove & return min (or null)
pq.remove();                        // throws if empty
pq.remove(obj);                     // O(n) remove specific
pq.size(); pq.isEmpty(); pq.contains(x); pq.clear();
pq.toArray();
```

Multi-field comparator:
```java
PriorityQueue<Map.Entry<String,Integer>> pq = new PriorityQueue<>(
    (a, b) -> a.getValue().equals(b.getValue())
        ? a.getKey().compareTo(b.getKey())
        : b.getValue() - a.getValue());
```

---

## 8. HashMap

Key -> value, unordered, O(1) average. Allows one `null` key and `null` values.

```java
Map<String, Integer> map = new HashMap<>();
Map<String, Integer> copy = new HashMap<>(map);
Map<String, List<Integer>> multi = new HashMap<>();
Map<Character, Integer> freq = new HashMap<>();
Map<String,Integer> imm = Map.of("a", 1, "b", 2);              // IMMUTABLE (<=10 pairs)
Map<String,Integer> imm2 = Map.ofEntries(Map.entry("a", 1));
```

### Set / Get
```java
map.put("a", 1);                    // set; returns previous value or null
map.get("a");                       // get; null if missing (unboxing null -> NPE)
map.getOrDefault("a", 0);           // get with fallback
map.size();
map.isEmpty();
```

### Built-in tools
```java
map.containsKey("a");               // boolean
map.containsValue(1);               // boolean, O(n)
map.remove("a");                    // returns removed value or null
map.remove("a", 1);                 // remove only if currently mapped to 1
map.clear();

map.putIfAbsent("a", 1);            // only if key missing (or null value); returns existing/null
map.putAll(otherMap);

map.computeIfAbsent(k, key -> new ArrayList<>());          // returns the value (existing or new)
map.computeIfPresent(k, (key, old) -> old + 1);            // return null to remove
map.compute(k, (key, old) -> old == null ? 1 : old + 1);
map.merge(k, 1, Integer::sum);                             // insert 1, or combine old and 1 with the function
map.replace(k, newVal);                                    // only if key exists
map.replace(k, oldVal, newVal);
map.replaceAll((k, v) -> v * 2);

map.keySet();                       // Set<K> view (removing from it removes from map)
map.values();                       // Collection<V> view
map.entrySet();                     // Set<Map.Entry<K,V>> view
map.forEach((k, v) -> System.out.println(k + "=" + v));
map.equals(other);
map.hashCode();
```

### Iteration
```java
for (String key : map.keySet()) { ... }
for (int val : map.values()) { ... }
for (Map.Entry<String, Integer> e : map.entrySet()) {
    String k = e.getKey();
    int v = e.getValue();
    e.setValue(v + 1);              // writes through
}
```

### Sorting entries
```java
List<Map.Entry<String,Integer>> es = new ArrayList<>(map.entrySet());
es.sort((a, b) -> b.getValue() - a.getValue());
es.sort(Map.Entry.comparingByValue(Comparator.reverseOrder()));
es.sort(Map.Entry.comparingByKey());
```

Gotchas:
- Array keys use identity hashing (`equals`/`hashCode` are not value-based); `List`, `String` and records are value-based.
- `int x = map.get(k)` NPEs if missing. Use `getOrDefault`.
- Mutating keys after insertion breaks lookups.
- Comparing boxed values: `map.get(a).equals(map.get(b))`, not `==` (breaks outside -128..127).

---

## 9. LinkedHashMap

HashMap that remembers **insertion order** (or access order).

```java
Map<String,Integer> lhm = new LinkedHashMap<>();         // insertion order
Map<String,Integer> acc = new LinkedHashMap<>(16, 0.75f, true);   // access order
// all HashMap methods apply

// Override to run after each put; return true to remove the eldest entry
new LinkedHashMap<String,Integer>() {
    @Override protected boolean removeEldestEntry(Map.Entry<String,Integer> e) { return size() > 3; }
};

// Java 21+: sequenced methods
lhm.firstEntry(); lhm.lastEntry(); lhm.pollFirstEntry(); lhm.pollLastEntry();
lhm.putFirst(k, v); lhm.putLast(k, v); lhm.sequencedKeySet();
```

---

## 10. TreeMap

Sorted by key (red-black tree). `O(log n)` for most operations. No `null` keys.

```java
TreeMap<Integer, String> tm = new TreeMap<>();
TreeMap<Integer, String> desc = new TreeMap<>(Collections.reverseOrder());
TreeMap<String, Integer> byLen = new TreeMap<>(Comparator.comparingInt(String::length));
```

All `Map` methods (`put`, `get`, `containsKey`, `getOrDefault`, `merge`, ...) plus:

```java
tm.firstKey();   tm.lastKey();          // throws if empty
tm.firstEntry(); tm.lastEntry();        // Map.Entry or null
tm.pollFirstEntry(); tm.pollLastEntry();// remove & return, null if empty

tm.floorKey(k);     // greatest key <= k   (null if none)
tm.ceilingKey(k);   // smallest key >= k
tm.lowerKey(k);     // greatest key <  k
tm.higherKey(k);    // smallest key >  k
tm.floorEntry(k);   tm.ceilingEntry(k);   tm.lowerEntry(k);   tm.higherEntry(k);

tm.headMap(k);              // keys <  k   (view)
tm.headMap(k, true);        // keys <= k
tm.tailMap(k);              // keys >= k   (view)
tm.tailMap(k, false);       // keys >  k
tm.subMap(from, to);        // [from, to)
tm.subMap(from, true, to, true);   // inclusive flags

tm.descendingMap();         // reverse view
tm.descendingKeySet();
tm.navigableKeySet();
tm.keySet(); tm.values(); tm.entrySet();   // sorted order
```

---

## 11. HashSet / LinkedHashSet

Unique elements. O(1) average. `LinkedHashSet` keeps insertion order.

```java
Set<Integer> set = new HashSet<>();
Set<Integer> fromList = new HashSet<>(list);
Set<Integer> fromArr = new HashSet<>(Arrays.asList(1, 2, 3));
Set<Integer> imm = Set.of(1, 2, 3);                 // IMMUTABLE, throws on duplicates
Set<Integer> ordered = new LinkedHashSet<>();
Set<List<Integer>> setOfLists = new HashSet<>();    // List has value-based equals/hashCode

set.add(5);             // returns true if newly added, false if already present
set.contains(5);
set.remove(5);          // returns boolean
set.size();
set.isEmpty();
set.clear();

set.addAll(other);          // union
set.retainAll(other);       // intersection
set.removeAll(other);       // difference
set.containsAll(other);     // subset check
set.removeIf(x -> x < 0);
set.iterator();
set.stream()...;
set.toArray(new Integer[0]);
for (int x : set) { ... }
```

---

## 12. TreeSet

Sorted unique elements (red-black tree). `O(log n)`.

```java
TreeSet<Integer> ts = new TreeSet<>();
TreeSet<Integer> desc = new TreeSet<>(Collections.reverseOrder());
TreeSet<int[]> byFirst = new TreeSet<>((a, b) -> a[0] - b[0]);   // comparator defines equality!
TreeSet<Integer> fromColl = new TreeSet<>(list);

ts.add(5); ts.remove(5); ts.contains(5); ts.size(); ts.isEmpty(); ts.clear();

ts.first();  ts.last();            // throws if empty
ts.pollFirst(); ts.pollLast();     // remove & return, null if empty

ts.floor(x);     // greatest <= x   (null if none)
ts.ceiling(x);   // smallest >= x
ts.lower(x);     // greatest <  x
ts.higher(x);    // smallest >  x

ts.headSet(x);               // elements <  x (view)
ts.headSet(x, true);         // <= x
ts.tailSet(x);               // elements >= x
ts.tailSet(x, false);        // >  x
ts.subSet(a, b);             // [a, b)
ts.subSet(a, true, b, true);

ts.descendingSet();          // reverse view
ts.descendingIterator();
```
Gotcha: `headSet/tailSet(...).size()` is O(n) (view counting).

---

## 13. Iterator

```java
Iterator<Integer> it = list.iterator();
while (it.hasNext()) {
    int x = it.next();
    if (x % 2 == 0) it.remove();         // safe removal during iteration
}

ListIterator<Integer> li = list.listIterator();    // bidirectional (List only)
li.hasNext(); li.next(); li.hasPrevious(); li.previous();
li.add(x); li.set(x); li.remove(); li.nextIndex(); li.previousIndex();

// Remove from map while iterating
Iterator<Map.Entry<K,V>> eit = map.entrySet().iterator();
while (eit.hasNext()) { if (cond(eit.next())) eit.remove(); }
```

---

## 14. Collections utility class

```java
Collections.sort(list);                          // natural order, stable
Collections.sort(list, comparator);
Collections.reverse(list);
Collections.shuffle(list);
Collections.swap(list, i, j);
Collections.max(coll);  Collections.min(coll);
Collections.max(coll, comparator);
Collections.frequency(coll, x);
Collections.binarySearch(sortedList, key);       // index, or -(insertion point) - 1
Collections.reverseOrder();                      // Comparator
Collections.reverseOrder(cmp);
Collections.unmodifiableList(list);
Collections.emptyList(); Collections.emptyMap(); Collections.emptySet();
Collections.singletonList(x);
Collections.nCopies(n, x);                       // immutable list of n copies
Collections.fill(list, x);
Collections.addAll(coll, a, b, c);
Collections.rotate(list, k);
Collections.disjoint(c1, c2);
Collections.synchronizedList(list);
```

---

## 15. Arrays utility class

```java
Arrays.toString(arr);                      // "[1, 2, 3]"
Arrays.deepToString(grid);                 // 2D+
Arrays.sort(arr);                          // primitives: dual-pivot quicksort; objects: stable merge/TimSort
Arrays.sort(arr, from, to);                // range [from, to)
Arrays.sort(objArr, comparator);           // Integer[], String[], int[][] with comparator
Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
Arrays.sort(arr, Collections.reverseOrder());   // needs Integer[] not int[]
Arrays.fill(arr, -1);
Arrays.fill(arr, from, to, val);
for (int[] row : grid) Arrays.fill(row, -1);     // fill 2D
Arrays.copyOf(arr, newLen);                // pad with zeros / truncate
Arrays.copyOfRange(arr, from, to);         // [from, to)
arr.clone();                               // shallow copy (2D: rows still shared!)
Arrays.equals(a, b);
Arrays.deepEquals(a2d, b2d);
Arrays.hashCode(arr);
Arrays.binarySearch(sorted, key);          // index, or -(insertion point) - 1
Arrays.asList(1, 2, 3);                    // fixed-size List (use Integer[], not int[])
Arrays.stream(arr);                        // IntStream for int[]
Arrays.stream(arr).sum(); .max().getAsInt(); .min().getAsInt(); .average();
Arrays.stream(arr).boxed().collect(Collectors.toList());
Arrays.setAll(arr, i -> i * i);
Arrays.mismatch(a, b);
Arrays.compare(a, b);
```

Sort `int[]` descending: `Arrays.stream(a).boxed().sorted(Collections.reverseOrder()).mapToInt(i->i).toArray();` or sort and reverse manually.

---

## 16. Math / Integer / Character helpers

```java
// Limits
Integer.MAX_VALUE;  Integer.MIN_VALUE;     // 2147483647, -2147483648
Long.MAX_VALUE;     Long.MIN_VALUE;
Double.MAX_VALUE;   Double.POSITIVE_INFINITY;
// Integer.MAX_VALUE + 1 wraps to Integer.MIN_VALUE (int overflow is silent)

// Math
Math.max(a, b); Math.min(a, b); Math.abs(x);
Math.pow(a, b);          // double
Math.sqrt(x);            // double
Math.floor(x); Math.ceil(x); Math.round(x);
Math.floorDiv(a, b); Math.floorMod(a, b);   // proper negative handling
Math.addExact(a, b); Math.multiplyExact(a, b);  // throw on overflow
Math.log(x); Math.log10(x); Math.hypot(a, b);
Math.cbrt(x);
a % b;                  // sign follows the dividend (can be negative)

// Integer
Integer.parseInt("123");
Integer.valueOf("123");
Integer.toString(5);  String.valueOf(5);
Integer.toBinaryString(5);      // "101"
Integer.toHexString(255);       // "ff"
Integer.toString(10, 2);        // base conversion
Integer.parseInt("101", 2);     // parse in base 2
Integer.bitCount(x);            // popcount
Integer.highestOneBit(x);
Integer.numberOfTrailingZeros(x); Integer.numberOfLeadingZeros(x);
Integer.reverse(x);
Integer.compare(a, b);          // safe comparator (no overflow)
Integer.signum(x);
Integer.sum(a, b); Integer.max(a, b);
Long.parseLong("..."); Long.bitCount(x);

// Character
Character.isDigit(c); Character.isLetter(c); Character.isLetterOrDigit(c);
Character.isUpperCase(c); Character.isLowerCase(c); Character.isWhitespace(c);
Character.toUpperCase(c); Character.toLowerCase(c);
Character.getNumericValue('7');   // 7
c - '0';                          // digit char -> int
(char)(d + '0');                  // int -> digit char
Character.toString(c);

// Bit operators
x & y;  x | y;  x ^ y;  ~x;
x << 1; x >> 1; x >>> 1;          // >>> is unsigned (logical) shift
```

---

## 17. ListNode, TreeNode, records

LeetCode supplies `ListNode` and `TreeNode`; shown here for syntax.

### ListNode
```java
class ListNode {
    int val; ListNode next;
    ListNode() {}
    ListNode(int val) { this.val = val; }
    ListNode(int val, ListNode next) { this.val = val; this.next = next; }
}
ListNode n = new ListNode(5);
n.next = new ListNode(6);
n.val; n.next;
```

### TreeNode
```java
class TreeNode {
    int val; TreeNode left, right;
    TreeNode() {}
    TreeNode(int val) { this.val = val; }
    TreeNode(int val, TreeNode left, TreeNode right) { this.val = val; this.left = left; this.right = right; }
}
```


### Pair / record (Java 16+)
```java
record Pair(int a, int b) {}               // auto equals/hashCode/toString
Pair p = new Pair(1, 2); p.a(); p.b();
Map.Entry<Integer,Integer> e = Map.entry(1, 2);          // immutable
AbstractMap.SimpleEntry<Integer,Integer> se = new AbstractMap.SimpleEntry<>(1, 2);   // mutable
```

---

## 18. Comparators & sorting

```java
// Lambda
(a, b) -> a - b                             // ascending (overflow risk with extreme values)
(a, b) -> Integer.compare(a, b)             // safe
(a, b) -> Integer.compare(b, a)             // descending

// Comparator factory methods
Comparator.naturalOrder();
Comparator.reverseOrder();
Comparator.comparingInt(a -> a[0]);
Comparator.comparingInt((int[] a) -> a[0]).thenComparingInt(a -> a[1]);
Comparator.comparing(String::length);
Comparator.comparing(String::length).thenComparing(Comparator.naturalOrder());
Comparator.comparing(Person::getAge).reversed();
Comparator.nullsFirst(cmp); Comparator.nullsLast(cmp);

// Examples
Arrays.sort(intervals, (a, b) -> a[0] - b[0]);                  // int[][]
Arrays.sort(words, (a, b) -> a.length() - b.length());          // String[]
Arrays.sort(words, String.CASE_INSENSITIVE_ORDER);
list.sort(Comparator.comparing(String::length).thenComparing(Comparator.naturalOrder()));
Collections.sort(list, Collections.reverseOrder());

// Sort chars of a string
char[] cs = s.toCharArray(); Arrays.sort(cs); String sorted = new String(cs);

// Custom class
class Person implements Comparable<Person> {
    public int compareTo(Person o) { return Integer.compare(this.age, o.age); }
}
```
Sorting is stable for objects (`Arrays.sort(Object[])`, `List.sort`, `Collections.sort`).
`Arrays.sort(int[])` cannot take a comparator, so use `Integer[]` or sort `int[][]`.

---

## 19. BitSet / EnumMap

```java
BitSet bs = new BitSet(n);
bs.set(i); bs.set(i, false); bs.set(from, to); bs.get(i); bs.clear(i); bs.flip(i);
bs.cardinality(); bs.nextSetBit(i); bs.nextClearBit(i); bs.length(); bs.size();
bs.and(o); bs.or(o); bs.xor(o); bs.andNot(o);

enum Color { RED, GREEN }
EnumMap<Color,Integer> em = new EnumMap<>(Color.class);
EnumSet<Color> es = EnumSet.of(Color.RED);   // also noneOf, allOf, range, complementOf
```

Others you may see: `Vector` (legacy ArrayList), `Hashtable` (legacy HashMap),
`ConcurrentHashMap`, `CopyOnWriteArrayList`, `IdentityHashMap`, `WeakHashMap` (concurrency / special cases).

---

## 20. Common conversions

```java
int[] -> List<Integer>:   Arrays.stream(a).boxed().collect(Collectors.toList())
List<Integer> -> int[]:   list.stream().mapToInt(i -> i).toArray()
String -> char[]:         s.toCharArray()
char[] -> String:         new String(cs)  /  String.valueOf(cs)
List<String> -> String[]: list.toArray(new String[0])
String[] -> List<String>: new ArrayList<>(Arrays.asList(arr))
Set -> List:              new ArrayList<>(set)
List -> Set:              new HashSet<>(list)
Map keys -> List:         new ArrayList<>(map.keySet())
List<List<Integer>> -> int[][]: no one-liner; loop, or list.stream().map(l -> l.stream().mapToInt(i->i).toArray()).toArray(int[][]::new)
```
