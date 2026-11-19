# Java Data Structures & Bit Manipulation Interview Cheatsheet

A comprehensive reference covering essential Java data structures, Big-O complexities, tree structures, and bit manipulation tricks for technical interviews.

---

## 1. Common Java Data Structures & Time Complexities

| Data Structure | Java Class / Interface | Access / Search | Insert / Delete | Best Used For |
| :--- | :--- | :--- | :--- | :--- |
| **Dynamic Array** | `ArrayList<E>` | $O(1)$ / $O(n)$ | $O(1)$ amortised (end) | Fast index lookups, iteration |
| **Linked List** | `LinkedList<E>` | $O(n)$ / $O(n)$ | $O(1)$ (at ends) | Frequent head/tail insertions |
| **Stack (LIFO)** | `ArrayDeque<E>` | $O(n)$ / $O(n)$ | $O(1)$ (top) | DFS, parentheses matching, undo |
| **Queue (FIFO)** | `ArrayDeque<E>` | $O(n)$ / $O(n)$ | $O(1)$ (front/back) | BFS, level-order traversal |
| **Hash Map** | `HashMap<K, V>` | $O(1)$ average | $O(1)$ average | Frequency counting, Two Sum lookups |
| **Hash Set** | `HashSet<E>` | $O(1)$ average | $O(1)$ average | Deduplication, fast existence checks |
| **Heap / Priority Queue** | `PriorityQueue<E>` | $O(1)$ peek / $O(n)$ | $O(\log n)$ | Top K elements, Dijkstra's, merging |
| **Balanced BST (Map/Set)** | `TreeMap` / `TreeSet` | $O(\log n)$ | $O(\log n)$ | Sorted keys, floor/ceiling queries |
| **Trie (Prefix Tree)** | Custom Class | $O(L)$ ($L$ = length) | $O(L)$ | Autocomplete, word search grids |
| **Graph** | `Map<V, List<V>>` | $O(V + E)$ traversal | $O(1)$ add edge | Networks, dependencies, shortest path |

---

## 2. Java Implementation Snippets

### Linear Structures
```java
// Dynamic Array
List<Integer> list = new ArrayList<>();
list.add(10);                    // Add to end: O(1)
int val = list.get(0);           // Access by index: O(1)
list.set(0, 20);                 // Update index: O(1)
list.remove(list.size() - 1);    // Remove from end: O(1)

// Stack (LIFO) - Prefer ArrayDeque over legacy Stack
Deque<Integer> stack = new ArrayDeque<>();
stack.push(1);                   // Add to top: O(1)
int top = stack.peek();          // View top: O(1)
int popped = stack.pop();        // Remove top: O(1)

// Queue (FIFO)
Queue<Integer> queue = new ArrayDeque<>();
queue.offer(1);                  // Add to back: O(1)
int front = queue.peek();        // View front: O(1)
int polled = queue.poll();       // Remove front: O(1)

// Singly Linked List Node
class ListNode {
    int val;
    ListNode next;
    ListNode(int val) { this.val = val; }
}
```

### Hashing Structures
```java
// HashMap (Key-Value pairs)
Map<String, Integer> map = new HashMap<>();
map.put("apple", 1);
map.put("apple", map.getOrDefault("apple", 0) + 1); // Frequency pattern
boolean hasKey = map.containsKey("apple");

// HashSet (Unique elements)
Set<Integer> set = new HashSet<>();
set.add(5);
boolean exists = set.contains(5); // O(1)
set.remove(5);
```

### Trees & Heaps
```java
// Min-Heap (Default)
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
minHeap.offer(10);
minHeap.offer(2);
int smallest = minHeap.poll(); // Returns 2: O(log n)

// Max-Heap
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
maxHeap.offer(10);
maxHeap.offer(2);
int largest = maxHeap.poll();  // Returns 10: O(log n)

// Ordered Map / Set (Red-Black Tree)
TreeMap<Integer, String> treeMap = new TreeMap<>();
treeMap.put(10, "Ten");
treeMap.put(20, "Twenty");
Integer lower = treeMap.floorKey(15);   // Greatest key <= 15 (returns 10): O(log n)
Integer higher = treeMap.ceilingKey(15); // Smallest key >= 15 (returns 20): O(log n)

// Binary Tree Node
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int val) { this.val = val; }
}
```

### Advanced Structures
```java
// Trie (Prefix Tree)
class TrieNode {
    TrieNode[] children = new TrieNode[26];
    boolean isEndOfWord = false;
}

class Trie {
    private TrieNode root = new TrieNode();

    public void insert(String word) {
        TrieNode curr = root;
        for (char c : word.toCharArray()) {
            int idx = c - 'a';
            if (curr.children[idx] == null) {
                curr.children[idx] = new TrieNode();
            }
            curr = curr.children[idx];
        }
        curr.isEndOfWord = true;
    }
}

// Graph (Adjacency List)
Map<Integer, List<Integer>> graph = new HashMap<>();
graph.computeIfAbsent(0, k -> new ArrayList<>()).add(1); // Directed edge 0 -> 1
```

---

## 3. Binary Tree vs. Red-Black Tree

| Feature | Binary Tree (Standard) | Binary Search Tree (BST) | Red-Black Tree |
| :--- | :--- | :--- | :--- |
| **Ordering Rule** | None | Left child < Parent < Right child | Left child < Parent < Right child |
| **Self-Balancing** | No | No (can degrade into a linked list) | **Yes** (via rotations & recolouring) |
| **Worst-Case Search** | $O(n)$ | $O(n)$ (when skewed) | **$O(\log n)$ guaranteed** |
| **Worst-Case Insert/Delete** | $O(n)$ | $O(n)$ (when skewed) | **$O(\log n)$ guaranteed** |
| **Extra Memory** | None | None | 1 bit per node (Red or Black colour) |
| **Java Equivalent** | Custom `TreeNode` | Custom `TreeNode` | `TreeMap` and `TreeSet` |

### Red-Black Tree Invariants (5 Rules)
1. **Colour Rule:** Every node is either Red or Black.
2. **Root Rule:** The root node is always Black.
3. **Leaf Rule:** All null leaf pointers are considered Black.
4. **No Double Reds:** A Red node cannot have a Red child.
5. **Equal Black Depth:** Every path from a given node to its descendant null leaves contains the same number of Black nodes.

---

## 4. Bit Manipulation Essentials

### Operators & Shifts
* **XOR (`^`):** Returns `1` only if bits differ.
  * Self-cancellation: `a ^ a = 0`
  * Identity: `a ^ 0 = a`
  * Commutative & Associative: `a ^ b ^ a = b`
* **Signed Right Shift (`>>`):** Shifts bits right and fills leftmost empty spots with the **sign bit** (`0` for positive, `1` for negative).
* **Unsigned Right Shift (`>>>`):** Shifts bits right and always fills leftmost empty spots with **`0`**. Essential for `while (n != 0)` loops on negative integers.
* **Left Shift (`<<`):** Shifts bits left and fills rightmost spots with `0` (no `<<<` exists).

### Two's Complement (Reading Negative Binary)
* The leftmost bit has a negative weight ($-2^{n-1}$).
* **Quick Mental Shortcut ("Flip and Add 1"):** To find the magnitude of a binary number starting with `1`:
  1. Invert all bits (`0` $\leftrightarrow$ `1`).
  2. Add `1`.
  3. Apply a minus sign.

### Classic Interview Patterns

#### 1. Number of 1 Bits / Hamming Weight (Brian Kernighan's Algorithm)
`n & (n - 1)` clears the rightmost `1` bit, running only once per set bit.
```java
public int hammingWeight(int n) {
    int count = 0;
    while (n != 0) {
        n = n & (n - 1);
        count++;
    }
    return count;
}
```

#### 2. Single Number (Every element appears twice except one)
```java
public int singleNumber(int[] nums) {
    int result = 0;
    for (int num : nums) {
        result ^= num;
    }
    return result;
}
```

#### 3. Single Number II (Every element appears 3 times except one)
Count bits per column across all 32 positions and take modulo 3.
```java
public int singleNumberII(int[] nums) {
    int ans = 0;
    for (int i = 0; i < 32; i++) {
        int sum = 0;
        for (int num : nums) {
            sum += (num >> i) & 1;
        }
        sum %= 3;
        ans |= (sum << i);
    }
    return ans;
}
```

#### 4. Bitwise AND of Numbers Range (`[left, right]`)
Strip the rightmost differing bits from `right` until only the shared binary prefix remains.
```java
public int rangeBitwiseAnd(int left, int right) {
    while (right > left) {
        right = right & (right - 1);
    }
    return right;
}
```
