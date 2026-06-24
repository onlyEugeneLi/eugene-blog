# 🐍 Python Interview Quick Reference

A quick-lookup guide for common interview patterns: string-to-list conversion, input handling, and integer constraints.

---

## 📝 String to List Conversion

| Method | Use Case | Example | Notes |
|---|---|---|---|
| `split()` | Delimited strings | `'a,b,c'.split(',')` → `['a', 'b', 'c']` | Fastest; default splits on whitespace and strips edges |
| `split(delimiter)` | Custom delimiter | `'a;b;c'.split(';')` | Splits only on specified delimiter |
| `list()` | Character-by-character | `list('abc')` → `['a', 'b', 'c']` | Includes spaces and special characters |
| `list comprehension` | Fine-grained control | `[char for char in string if char.isalpha()]` | Filter/transform during conversion |
| `re.split()` | Multiple delimiters | `re.split(r'[;,]', string)` | Regex support for variable delimiters |
| `json.loads()` | JSON-encoded strings | `json.loads('["a","b"]')` | Convert JSON string to Python list |

---

## 📄 String Operations

### Slicing
| Operation | Syntax | Example | Result |
|---|---|---|---|
| Substring | `str[start:end]` | `'hello'[1:4]` | `'ell'` |
| Every nth char | `str[start:end:step]` | `'hello'[::2]` | `'hlo'` |
| Reverse | `str[::-1]` | `'hello'[::-1]` | `'olleh'` |
| From index to end | `str[start:]` | `'hello'[2:]` | `'llo'` |

### Concatenate & Modify
| Operation | Example | Notes |
|---|---|---|
| Concatenate | `'a' + 'b'` | Creates new string (immutable) |
| Repeat | `'ab' * 3` | `'ababab'` |
| Replace | `'hello'.replace('l', 'x')` | `'hexxo'` |
| Case | `'Hello'.lower()`, `.upper()`, `.title()` | Returns new string |
| Strip whitespace | `'  hello  '.strip()` | Also `.lstrip()`, `.rstrip()` |

### Format & Interpolation
| Method | Syntax | Example |
|---|---|---|
| f-string (3.6+) | `f'Hello {name}'` | `f'Count: {i*2}'` |
| `.format()` | `'Hello {}'.format(name)` | `'Hello {0} {1}'.format(a, b)` |
| `%` operator (legacy) | `'Hello %s' % name` | `'Value: %d' % 42` |
| Join list | `', '.join(['a', 'b', 'c'])` | `'a, b, c'` |

### Escape Sequences
| Escape | Meaning | Example |
|---|---|---|
| `\n` | Newline | `'line1\nline2'` |
| `\t` | Tab | `'col1\tcol2'` |
| `\\` | Backslash | `'C:\\path'` |
| `\'` | Single quote | `'don\'t'` |
| `\"` | Double quote | `"He said \"Hi\""` |
| `\r` | Carriage return | Used in files |

### Common String Methods
| Method | Use | Example |
|---|---|---|
| `.find(sub)` | Find substring index | `'hello'.find('l')` → `2` |
| `.count(sub)` | Count occurrences | `'hello'.count('l')` → `2` |
| `.startswith()`, `.endswith()` | Check prefix/suffix | `'hello'.endswith('lo')` → `True` |
| `.isdigit()`, `.isalpha()` | Type checks | `'123'.isdigit()` → `True` |
| `.split()`, `.rsplit()` | Split string | `'a,b,c'.split(',')` → `['a','b','c']` |
| `.partition(sep)` | Split into 3-tuple | `'a-b-c'.partition('-')` → `('a', '-', 'b-c')` |

---

## 🖨️ print() Function & Parameters

| Parameter | Default | Purpose | Example |
|---|---|---|---|
| `*objects` | Required | Values to print (comma-separated) | `print(1, 2, 3)` → `1 2 3` |
| `sep` | `' '` (space) | Separator between objects | `print('a', 'b', sep=',')` → `a,b` |
| `end` | `'\n'` (newline) | String appended after all objects | `print('x', end='')` → no newline |
| `file` | `sys.stdout` | Output stream (stdout or file object) | `print('msg', file=open('log.txt', 'w'))` |
| `flush` | `False` | Force buffer flush immediately | `print('...', flush=True)` |

**Common patterns:**
- **No newline:** `print('text', end='')`
- **Custom separator:** `print('a', 'b', 'c', sep='-')` → `a-b-c`
- **Write to file:** `print(data, file=f)`
- **Multiple values:** `print(name, age, city, sep=' | ')` → formatted output

---

## 📥 Input Methods (stdin)

| Method | Best For | Example | Notes |
|---|---|---|---|
| `input()` | Interactive prompts | `name = input("Enter name: ")` | Simplest; waits for user response during execution |
| `sys.stdin` | Stream processing | `for line in sys.stdin: process(line)` | Direct stdin access; useful for piped input |
| `fileinput.input()` | Multiple files | `for line in fileinput.input(['file1', 'file2']):` | Reads multiple files or accepts from command line |

> **Tip:** `input()` removes trailing newline; `sys.stdin` preserves it.

---

## 🔧 Command-Line Arguments

| Method | Use Case | Example | Notes |
|---|---|---|---|
| `sys.argv` | Access all CLI args | `python script.py arg1 arg2` → `sys.argv = ['script.py', 'arg1', 'arg2']` | Index 0 is script name; simple but manual parsing |
| `sys.argv[1:]` | Skip script name | `args = sys.argv[1:]` | Get only user-provided arguments |
| `argparse` | Flexible argument parsing | `parser = argparse.ArgumentParser()` | Industry standard; handles flags, options, help text |
| `getopt` | Unix-style options | `opts, args = getopt.getopt(sys.argv[1:], 'hi:o:')` | Simpler than argparse; good for `-h`, `-i input` patterns |

**Quick sys.argv example:**
```python
import sys
# python script.py name 25
print(sys.argv)      # ['script.py', 'name', '25']
name = sys.argv[1]   # 'name'
age = sys.argv[2]    # '25'
```

**Quick argparse example:**
```python
import argparse
parser = argparse.ArgumentParser()
parser.add_argument('name', help='User name')
parser.add_argument('-a', '--age', type=int, help='User age')
args = parser.parse_args()  # Handles --help automatically
print(args.name, args.age)
```

> **Interview tip:** Use `sys.argv` for simple scripts; `argparse` for production code. Mention both.

### CLI Flags to Class Constructor (argparse → **kwargs Pattern)

**Production pattern:** Parse terminal flags into a dictionary and unpack with `**` into a class or function.

```python
import argparse
from typing import Dict, Any

class ValidationError(Exception):
    def __init__(self, **kwargs):
        self.message = kwargs.get("message", "Default error")
        self.code = kwargs.get("code", "500")
        super().__init__(self.message)

def parse_cli_to_kwargs() -> Dict[str, Any]:
    """Converts command-line flags to dictionary."""
    parser = argparse.ArgumentParser(description="Validation flags")
    parser.add_argument("--message", type=str, help="Error message")
    parser.add_argument("--code", type=str, help="Error code")
    
    parsed_args = parser.parse_args()
    return vars(parsed_args)  # Namespace → dict

def main():
    cli_kwargs = parse_cli_to_kwargs()
    # Strip None values so class defaults aren't overridden
    clean_kwargs = {k: v for k, v in cli_kwargs.items() if v is not None}
    
    try:
        raise ValidationError(**clean_kwargs)
    except ValidationError as e:
        print(f"Message: {e.message}, Code: {e.code}")

if __name__ == "__main__":
    main()
```

**Terminal usage:**
```bash
python script.py --message "Invalid input" --code "400"
# Output: Message: Invalid input, Code: 400
```

**What happens under the hood:**
1. Terminal input `--message "value" --code "400"` → `sys.argv` list
2. `parser.parse_args()` → `Namespace(message='value', code='400')`
3. `vars()` → `{'message': 'value', 'code': '400'}` (plain dict)
4. `ValidationError(**clean_kwargs)` → unpacks dict as keyword arguments

> **Interview tip:** Explain the flow: raw strings → argparse Namespace → `vars()` to dict → `**` unpacking to constructor. Always filter out `None` values to preserve class defaults.

---

## ⚡ Operators: lambda, walrus (:=), map(), filter()

| Operator/Function | Syntax | Example | Purpose |
|---|---|---|---|
| `lambda` | `lambda args: expression` | `square = lambda x: x**2` | Anonymous function for simple operations |
| `:=` (walrus) | `(var := expression)` | `if (n := len(data)) > 10: print(n)` | Assign AND use in same expression (Python 3.8+) |
| `map()` | `map(func, iterable)` | `list(map(lambda x: x*2, [1,2,3]))` | Apply function to all items → `[2, 4, 6]` |
| `filter()` | `filter(func, iterable)` | `list(filter(lambda x: x>2, [1,2,3,4]))` | Keep only items matching condition → `[3, 4]` |
| List comprehension | `[expr for item in iter if cond]` | `[x*2 for x in [1,2,3] if x>1]` | Concise alternative to map/filter → `[4, 6]` |

**Walrus operator (:=) examples:**
```python
# Without walrus (compute twice)
result = compute_expensive()
if result > threshold:
    print(result)

# In conditionals
if (n := random.randint(0, 10)) <= 5:
    print(f"Got {n}, continuing...")
```

> **Interview tip:** Walrus `:=` reduces redundant computation when you need to use a value in a condition. Use it to avoid calling expensive functions or lookups twice.

**Lambda vs Named Function:**
```python
# Lambda
result = map(lambda x: x + 1, [1, 2, 3])  # Concise, one-liner

# Named function (more readable for complex logic)
def add_one(x):
    return x + 1
result = map(add_one, [1, 2, 3])
```

**map() common patterns:**
```python
# Convert strings to integers
nums = list(map(int, ['1', '2', '3']))  # [1, 2, 3]

# Apply function to each element
squared = list(map(lambda x: x**2, [1, 2, 3]))  # [1, 4, 9]

# Chain map operations
result = list(map(lambda x: x*2, map(int, ['1', '2'])))  # [2, 4]
```

> **Interview tip:** Prefer list comprehension over `map()` + `lambda` for readability; use `map()` when passing named functions.

---

## 🌟 Unpacking: * and **

**Key insight:** `*` and `**` have opposite meanings in function definitions vs function calls.

### In Function Definitions (Packing Arguments)

| Operator | What It Catches | Example | Result |
|---|---|---|---|
| `*args` | Extra positional arguments | `def func(*args): print(args)` called with `func(1, 2, 3)` | `args = (1, 2, 3)` tuple |
| `**kwargs` | Extra keyword arguments | `def func(**kwargs): print(kwargs)` called with `func(a=1, b=2)` | `kwargs = {'a': 1, 'b': 2}` dict |

```python
def print_info(*args, **kwargs):
    print(f"Positional: {args}")    # Tuple of all positional args
    print(f"Keyword: {kwargs}")     # Dict of all keyword args

print_info("IP1", "IP2", host="localhost", port=5433)
# Output: Positional: ('IP1', 'IP2')
#         Keyword: {'host': 'localhost', 'port': 5433}
```

### In Function Calls (Unpacking Collections)

| Operator | What It Unpacks | Example | Result |
|---|---|---|---|
| `*iterable` | Spreads list/tuple elements | `print(*[1, 2, 3])` | Arguments become: `print(1, 2, 3)` |
| `**dict` | Spreads dict as keyword args | `func(**{'a': 1, 'b': 2})` | Arguments become: `func(a=1, b=2)` |

```python
ips = ["10.0.0.1", "10.0.0.2"]
print(*ips)  # Unpacks list → print("10.0.0.1", "10.0.0.2")
# Output: 10.0.0.1 10.0.0.2

config = {"host": "127.0.0.1", "port": 5433}
connect_to_db(**config)  # Unpacks dict → connect_to_db(host="127.0.0.1", port=5433)
```

### Quick Reference Table

| Context | `*` | `**` |
|---|---|---|
| **Function definition** | Packs positional args → tuple | Packs keyword args → dict |
| **Function call** | Unpacks iterable → individual args | Unpacks dict → keyword args |

### Advanced: Keyword-Only Arguments (Single `*`)

A standalone `*` in the function signature forces all following parameters to be passed explicitly by name:

```python
def drop_database(db_name, *, force=False):
    pass

drop_database("prod", True)         # ❌ TypeError—can't use positional
drop_database("prod", force=True)   # ✅ Correct—must use keyword

# Prevents accidental unsafe calls in production code
```

> **Interview tip:** Explain both packing (definition) and unpacking (call) clearly. Bonus: mention keyword-only barrier `*` prevents production bugs.

---

## 🎨 Decorators & Wrappers

### Decorator Syntax & Purpose

| Concept | Purpose | Syntax |
|---|---|---|
| **Decorator** | Function that wraps another function to modify behavior | `@decorator` above function def |
| **Wrapper** | Inner function that executes before/after target function | Nested function inside decorator |
| **@functools.wraps** | Preserve metadata (name, docstring) of wrapped function | Import and use inside wrapper |

### Basic Decorator Pattern
```python
def decorator(func):
    def wrapper(*args, **kwargs):
        # Before function execution
        print(f"Calling {func.__name__}")
        result = func(*args, **kwargs)
        # After function execution
        return result
    return wrapper

@decorator
def greet(name):
    return f"Hello {name}"

# Equivalent to: greet = decorator(greet)
```

### Common Decorator Use Cases

| Use Case | Example | Purpose |
|---|---|---|
| **Logging** | Log when function is called | `@log_calls` |
| **Timing** | Measure execution time | `@timer` |
| **Caching** | Cache function results | `@functools.lru_cache(maxsize=128)` |
| **Type checking** | Validate argument types | `@type_check` |
| **Authentication** | Verify permissions | `@login_required` |

### Decorator with Arguments
```python
def repeat(times):
    def decorator(func):
        def wrapper(*args, **kwargs):
            result = None
            for _ in range(times):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator

@repeat(times=3)
def greet(name):
    print(f"Hello {name}")

# Calls greet() 3 times
```

### Practical Decorator Examples

**Simple timing decorator:**
```python
import time
from functools import wraps

def timer(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        elapsed = time.time() - start
        print(f"{func.__name__} took {elapsed:.2f}s")
        return result
    return wrapper

@timer
def slow_task():
    time.sleep(1)
```

**Caching decorator (built-in):**
```python
from functools import lru_cache

@lru_cache(maxsize=128)
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)
```

> **Interview tip:** Always use `@functools.wraps` to preserve function metadata. Decorators are commonly tested in interviews—know the pattern!

---

## 📊 Data Structures & Time/Space Complexity

**Mental model:** Choose your data structure based on how you need to *access, search, or modify* your data.

### Core Data Structures Comparison

| Structure | Access | Search | Insert/Delete | Space | Key Use Case | Hashable? |
|---|---|---|---|---|---|---|
| `list` | O(1) by index | O(n) by value | O(1) append / O(n) insert | Low | Ordered sequence, preserves order | ❌ No |
| `dict` | O(1) by key | O(1) average | O(1) average | High | Key-value lookup, fast exact match | ❌ No |
| `set` | N/A | O(1) average | O(1) average | High | Uniqueness, membership, deduplication | ❌ No |
| `tuple` | O(1) by index | O(n) by value | N/A (immutable) | Lowest | Fixed records, hashable composite keys | ✅ Yes |

### When to Use Each

| Use Case | Data Structure | Why | Example |
|---|---|---|---|
| **Sequential processing** | `list` | Order matters, mutable | Log lines processed sequentially |
| **Fast exact lookups** | `dict` | O(1) key → value mapping | Node IP → health status metadata |
| **Deduplication + math** | `set` | Instant uniqueness + operations (union, diff) | Deduplicate target hostnames before probing |
| **Immutable composite key** | `tuple` | Can be dict key or set element | IP-port pair: `("10.0.0.1", 5433)` |

### Interview Answer Template

**Question:** "When would you use a list vs. a set vs. a dict?"

**Structure your answer:**
1. **Frame by access pattern:** "I choose based on lookup patterns. Lists are O(n) for value search—sets and dicts are O(1) using hash tables."
2. **Trade-off:** "But dicts/sets use more memory for hash table overhead. For massive static data, tuples/lists are more memory-efficient."
3. **Production context:** "In infrastructure validation, I ingest config lines into a list (order matters), convert keys to a set for O(1) validation checks, then output a dict for consumers."

### collections Library: Extended Data Structures

From `from collections import ...`:

| Type | Purpose | Retrieve Underlying Data | Example |
|---|---|---|---|
| `defaultdict` | Dict with default value for missing keys | Direct access (it is a dict) | `d = defaultdict(list); d['key'].append(item)` |
| `Counter` | Count occurrences of elements | Direct access (subclass of dict) | `Counter(['a','b','a']) → Counter({'a': 2, 'b': 1})` |
| `deque` | Double-ended queue, O(1) on both ends | Iteration or convert to list | `d = deque([1,2,3]); d.appendleft(0)` |
| `namedtuple` | Immutable tuple with named fields | Attribute access or `._asdict()` | `Point = namedtuple('Point', ['x', 'y']); p = Point(10, 20); p.x` |
| `UserString` | Extensible string wrapper | `.data` attribute (original string) | `us = UserString("hello"); print(us.data)` |
| `UserDict` | Extensible dict wrapper | `.data` attribute (original dict) | `ud = UserDict({'a': 1}); print(ud.data)` |
| `UserList` | Extensible list wrapper | `.data` attribute (original list) | `ul = UserList([1,2,3]); print(ul.data)` |
| `OrderedDict` | Dict that remembers insertion order (Python 3.7+ dicts do this by default) | Direct access (legacy, use regular dict) | `od = OrderedDict([('a', 1), ('b', 2)])` |

**Quick collections examples:**
```python
from collections import defaultdict, Counter, deque, namedtuple, UserString, UserDict, UserList

# defaultdict — no KeyError on missing keys
config = defaultdict(list)
config['servers'].append('10.0.0.1')

# Counter — frequency analysis
errors = Counter(['timeout', 'timeout', 'auth_failed'])
print(errors.most_common(1))  # [('timeout', 2)]

# deque — efficient pop from both ends
queue = deque([1, 2, 3])
queue.appendleft(0)  # O(1) left append
queue.pop()          # O(1) right pop

# namedtuple — immutable labeled records
Address = namedtuple('Address', ['ip', 'port'])
addr = Address('127.0.0.1', 5433)
print(addr.ip)  # '127.0.0.1'

# UserString, UserDict, UserList — access underlying data via .data
user_str = UserString("hello")
print(user_str.data)  # Original string: 'hello'

user_dict = UserDict({'server': 'prod', 'port': 5433})
print(user_dict.data)  # Original dict: {'server': 'prod', 'port': 5433}

user_list = UserList([1, 2, 3, 4, 5])
print(user_list.data)  # Original list: [1, 2, 3, 4, 5]
```

> **Interview tip:** "I use defaultdict when I need dict behavior without KeyError, Counter for frequency problems, and namedtuple for immutable structured data that might be dict keys or set elements."

---

## 🔍 Complexity Analysis: Common Algorithms

### Common Data Structure Operations

| Operation | list | dict | set | string | Space |
|---|---|---|---|---|---|
| **Access by index/key** | O(1) | O(1) avg | N/A | O(1) | O(1) |
| **Search (contains?)** | O(n) | O(1) avg | O(1) avg | O(n) | O(1) |
| **Insert at end** | O(1) amortized | O(1) avg | O(1) avg | N/A (immutable) | O(1) |
| **Insert at middle** | O(n) | N/A | N/A | N/A | O(1) |
| **Delete element** | O(n) | O(1) avg | O(1) avg | N/A | O(1) |
| **Iterate all** | O(n) | O(n) | O(n) | O(n) | O(1) |
| **Slice/substring** | O(k) where k=slice size | N/A | N/A | O(k) copies new string | O(k) |
| **Sort** | O(n log n) | N/A | N/A | O(n log n) | O(n) or O(1) depending on algorithm |
| **Compare (==)** | O(n) element-by-element | O(n) key-value pairs | O(n) element-by-element | O(n) character-by-character | O(1) |

> **Note on dict operations:** Most dict operations average O(1) due to hash table implementation, but worst-case is O(n) with hash collisions. Reference: [Python Complexity — dict operations](https://pythoncomplexity.com/builtins/dict/#complexity-reference)

### Why String/List Comparison is O(n)

When Python evaluates `word == "work"`, it compares **element by element**:
```python
# "word" == "work"
# Compares: 'w'=='w' (✓) → 'o'=='o' (✓) → 'r'=='r' (✓) → 'd'!='k' (✗)
# Total comparisons: O(len(word))
```

### Practical Algorithm Complexity Examples

#### Sliding Window (Finding Substring)
```python
# Find if pattern exists in text using sliding window
def sliding_window(text, pattern):
    """
    Time: O(N * M) where N=len(text), M=len(pattern)
    Why: For each position N in text, compare pattern M characters
    At each window position, pattern == text[i:i+M] is O(M)
    """
    n, m = len(text), len(pattern)
    for i in range(n - m + 1):
        if text[i:i+m] == pattern:  # This comparison alone is O(M)
            return i
    return -1

# Example: text="hello", pattern="ll"
# i=0: "he" == "ll"? → O(2) comparisons
# i=1: "el" == "ll"? → O(2) comparisons
# i=2: "ll" == "ll"? → O(2) comparisons, MATCH!
# Total: O(5 * 2) = O(10) = O(N*M)
```

#### Two Pointers (Finding Pair in Sorted Array)
```python
def two_pointer_sum(arr, target):
    """
    Time: O(N) where N=len(arr)
    Why: Each pointer moves exactly once left or right
    No nested loops, single pass through array
    """
    left, right = 0, len(arr) - 1
    while left < right:
        current_sum = arr[left] + arr[right]
        if current_sum == target:
            return (left, right)
        elif current_sum < target:
            left += 1
        else:
            right -= 1
    return None

# Example: arr=[1,3,5,7], target=8
# Pointer movements: left→right, right→left, etc.
# Each element visited at most once = O(N)
```

#### Hash Map Deduplication (vs List Search)
```python
# SLOW: Using list search - O(N²)
def deduplicate_slow(items):
    """Time: O(N²) because contains check is O(N) for each item"""
    seen = []
    for item in items:
        if item not in seen:  # O(N) search in list
            seen.append(item)
    return seen

# FAST: Using set - O(N)
def deduplicate_fast(items):
    """Time: O(N) because 'in' check is O(1) for sets"""
    return list(set(items))

# With 1000 items:
# List version: ~1,000,000 operations
# Set version: ~1,000 operations
```

#### Nested Loop Pitfall - O(N²)
```python
# DON'T: Nested loops over same collection
def has_pair_slow(arr):
    """Time: O(N²) - comparing each element with every other element"""
    for i in range(len(arr)):
        for j in range(i+1, len(arr)):  # Inner loop repeats N times
            if arr[i] + arr[j] == target:
                return True
    return False

# DO: Use hash set - O(N)
def has_pair_fast(arr, target):
    """Time: O(N) - single pass with set lookup"""
    seen = set()
    for num in arr:
        if target - num in seen:  # O(1) lookup in set
            return True
        seen.add(num)  # O(1) insertion in set
    return False
```

### Space Complexity Patterns

| Pattern | Space | Notes |
|---|---|---|
| **In-place modification** | O(1) | Modify without extra data structures (reverse array in-place) |
| **Input copy** | O(n) | Copy input for safety (most scripts do this) |
| **Hash map/set storage** | O(n) | Store n elements in dict/set for O(1) lookups |
| **Recursion call stack** | O(h) | h = height of recursion tree (e.g., O(log n) for binary search) |
| **Output storage** | O(n) | Result needs space (sorting, deduplication, etc.) |

**Example: Space trade-off**
```python
# Space: O(1) - But slow: O(N²) time
def has_pair_no_space(arr, target):
    for i in range(len(arr)):
        for j in range(i+1, len(arr)):
            if arr[i] + arr[j] == target:
                return True

# Space: O(N) - But fast: O(N) time
def has_pair_with_space(arr, target):
    seen = set()  # Extra O(N) space
    for num in arr:
        if target - num in seen:
            return True
        seen.add(num)
```

### Interview Checklist: Analyzing Complexity

When solving a problem, ask yourself:

1. **What repeats?** (identifies nested loops → polynomial complexity)
   - No loop: O(1)
   - Single loop: O(n)
   - Nested loops: O(n²), O(n³), etc.
   - Divide and conquer: O(n log n)

2. **What is the "unit of work"?** (what happens per iteration?)
   - String/list comparison: O(length of comparison)
   - Hash lookup: O(1) average
   - Linear search: O(n)

3. **Do I have a choice?** (time vs space trade-off?)
   - Set for O(1) lookups (costs O(n) space)
   - Sorted array for binary search (costs sort time O(n log n))

> **Interview tip:** "I always identify the hidden loops and clarify what the 'unit of work' is per iteration. For example, in sliding window, each window movement is one iteration, but the pattern comparison inside is O(M), making total O(N*M). Don't overlook the cost of comparisons and built-in operations."

---

## 🔄 Iteration Utilities

| Function | Purpose | Return Type | Time | Space |
|---|---|---|---|---|
| `enumerate()` | Track index + item simultaneously | Iterator of (index, item) tuples | O(n) | O(1) |
| `zip()` | Pair elements from multiple iterables by index | Iterator of tuples | O(n) | O(1) |
| `reversed()` | Iterate backwards without modifying original | Iterator in reverse order | O(n) | O(1) |
| `sorted()` | Create sorted copy and iterate | New sorted list | O(n log n) | O(n) |
| `filter()` | Iterate only matching items (predicate) | Iterator of filtered items | O(n) | O(1) |

---

## 🚨 Exception Handling (try-except)

| Clause | Purpose | Syntax |
|---|---|---|
| `try` | Code that might raise exception | `try: risky_code()` |
| `except` | Catch specific exception(s) | `except ValueError: handle()` |
| `else` | Execute if no exception occurred | `else: success_code()` |
| `finally` | Always execute (cleanup) | `finally: cleanup()` |

### Try-Except Examples

**Catch single exception:**
```python
try:
    num = int("abc")
except ValueError:
    print("Not a valid number")
```

**Catch multiple exceptions:**
```python
try:
    data = json.loads(response)
except (ValueError, TypeError) as e:
    print(f"Parse error: {e}")
```

**Catch all exceptions (avoid in production):**
```python
try:
    risky_operation()
except Exception as e:
    print(f"Error: {e}")
```

**Complete try-except-else-finally:**
```python
try:
    file = open('data.txt')
    data = file.read()
except FileNotFoundError:
    print("File not found")
except IOError as e:
    print(f"Read error: {e}")
else:
    print("Success! Data read.")
finally:
    file.close()  # Always executes
```

### Common Built-in Exceptions

| Exception | Cause | Example |
|---|---|---|
| `ValueError` | Invalid value for type | `int("abc")` |
| `TypeError` | Wrong operand type | `"5" + 5` |
| `KeyError` | Dictionary key not found | `dict["missing"]` |
| `IndexError` | List index out of range | `list[999]` |
| `FileNotFoundError` | File doesn't exist | `open("nonexistent.txt")` |
| `ZeroDivisionError` | Division by zero | `1 / 0` |
| `AttributeError` | Object has no attribute | `obj.missing_attr` |
| `RuntimeError` | Generic runtime error | Raised by user code |

### Raise Custom Exceptions

**Pattern: Always call `super().__init__(message)` in custom exceptions:**

```python
class ValidationError(Exception):
    def __init__(self, **kwargs):
        self.message = kwargs.get("message", "Default validation error")
        self.code = kwargs.get("code", "500")
        super().__init__(self.message)  # ← Pass message to Exception

# Proper usage
try:
    raise ValidationError(message="Bad input", code="400")
except ValidationError as e:
    print(e)  # Prints: Bad input (message displays correctly)
    print(e.code)  # Prints: 400 (custom attribute accessible)
```

**Why `super().__init__(message)` is required:**
- Stores message in `Exception.args` so exception displays when raised/logged
- Without it, `str(e)` prints empty or garbage
- Tracebacks show proper error message
- Logging captures the right context

**Simple custom exception:**
```python
if age < 0:
    raise ValueError("Age cannot be negative")

try:
    validate(data)
except ValueError as e:
    print(f"Validation failed: {e}")
```

### logging.error() vs logger.exception() vs raise

These three do different things and are often used **together** in production:

| Method | Logs Message? | Logs Traceback? | Program Continues? | Use When |
|---|---|---|---|---|
| `logging.error(f"{e}")` | ✅ Yes | ❌ No | ✅ Yes | Want to record error but keep running |
| `logging.exception("msg")` | ✅ Yes | ✅ Yes (auto) | ✅ Yes | Need full context + traceback; must be in except block |
| `raise` | ❌ No | ❌ No | ❌ No (propagates) | Caller needs to know; program stops/passes exception up |

**Production pattern:**
```python
import logging

logger = logging.getLogger(__name__)

try:
    validate_user(data)
except ValidationError as e:
    # DO BOTH: log it AND raise it
    logger.exception(f"Validation failed -- {e}")  # Logs message + full traceback
    raise  # Propagates to caller; use bare 'raise' to preserve original traceback
```

**Why both?**
- `logger.exception()` — creates a permanent record for debugging
- `raise` — tells the caller/program that something went wrong and stops execution
- Use bare `raise` (not `raise ValidationError()`) to preserve the original stack trace

**Other patterns:**
```python
# Log and handle locally (program continues)
except DatabaseError as e:
    logger.exception("DB failed, retrying...")
    return None  # or retry logic

# Log, raise to inform caller
except AuthError as e:
    logger.exception("Auth failed")
    raise  # Propagate

# Just raise (no logging needed here if parent catches and logs)
except IOError as e:
    raise  # Parent handler will log it
```

> **Interview tip:** "In production, I log exceptions with full traceback using `logger.exception()` inside the except block, then raise to propagate if it's a critical error. This gives me both a record and proper error handling."

---

> **Interview tip:** Catch specific exceptions, not bare `except:`. Use `finally` for resource cleanup.

---

## 📋 Logging

### Mental Model: Logger → Handler → Formatter

```
Logger     →  Handler   →  Formatter
(what)        (where)      (how it looks)

Logger controls minimum level to process (logger.setLevel)
Handler controls where logs go (file, console, network)
Formatter controls the string format of each log line
```

> **Key insight:** Format lives on the handler, not the logger — different destinations might need different formats (e.g., file with timestamps vs console with just the message).

### Log Levels

| Level | Value | Use Case | Example |
|---|---|---|---|
| `DEBUG` | 10 | Detailed diagnostic info | `logger.debug("Variable x = 5")` |
| `INFO` | 20 | General informational | `logger.info("User logged in")` |
| `WARNING` | 30 | Warning, unexpected behavior | `logger.warning("Low memory")` |
| `ERROR` | 40 | Error, serious problem | `logger.error("Connection failed")` |
| `CRITICAL` | 50 | Critical, system might fail | `logger.critical("Database down")` |

### Approach 1: basicConfig (Quick Setup, One Destination)

**Use for:** Scripts, quick tools, prototypes

```python
import logging

# One-liner setup — configures root logger globally
logging.basicConfig(
    filename="dev.log",
    format='%(asctime)s: %(levelname)s: %(message)s',
    level=logging.INFO
)

logger = logging.getLogger()
logger.info("Application started")
```

| Aspect | Details |
|---|---|
| **Logger** | Root logger (global) |
| **Destinations** | One (file or console, not both) |
| **Flexibility** | Low—all-or-nothing setup |
| **Reusability** | Quick but not suitable for production |

> **Note:** Format attribute is `%(levelname)s`, not `%(level)s`.

### Approach 2: Handlers (Production-Grade, Multiple Destinations)

**Use for:** Production code, libraries, needing multiple handlers

```python
import logging

# Named logger (not root)—allows scoped control
logger = logging.getLogger("MyApp")
logger.setLevel(logging.DEBUG)

# Reusable formatter
formatter = logging.Formatter('%(asctime)s: %(levelname)s: %(message)s')

# Handler 1: File gets ERROR and above
file_handler = logging.FileHandler("errors.log")
file_handler.setLevel(logging.ERROR)
file_handler.setFormatter(formatter)

# Handler 2: Console gets INFO and above
console_handler = logging.StreamHandler()
console_handler.setLevel(logging.INFO)
console_handler.setFormatter(formatter)

# Stack handlers—logs go to both destinations
logger.addHandler(file_handler)
logger.addHandler(console_handler)

logger.debug("Debug message")     # Only in file (if DEBUG enabled)
logger.info("Info message")       # Console + file
logger.error("Error occurred")    # Console + file
```

| Aspect | Details |
|---|---|
| **Logger** | Named logger (scoped, reusable) |
| **Destinations** | Many—stack multiple handlers |
| **Flexibility** | High—each handler has its own level/format |
| **Production-ready** | Yes |

### Exception Logging: logger.exception()

**Use `logger.exception()` ONLY inside except blocks.** It automatically captures the full stack trace.

```python
import logging

logger = logging.getLogger(__name__)

try:
    result = 10 / 0
except ZeroDivisionError:
    # Logs error at ERROR level + full traceback automatically
    logger.exception("Division failed")
    # Output includes: "Division failed" + full stack trace
```

**Comparison:**

| Method | Level | Traceback | Use Case |
|---|---|---|---|
| `logger.exception()` | ERROR | ✅ Automatic | Inside except block only |
| `logger.error("msg", exc_info=True)` | ERROR | ✅ Manual flag | When you need error level with traceback |
| `logger.error("msg")` | ERROR | ❌ None | Regular error, no exception context |

**Production example:**
```python
try:
    connect_to_db()
except ConnectionError as e:
    logger.exception("Database connection failed")  # Logs error + full traceback
    # Don't raise; log and handle gracefully
except Exception as e:
    logger.exception("Unexpected error during connection")  # Catches all, logs traceback
    raise  # Re-raise for higher-level handling
```

> **Interview tip:** Use `logger.exception()` inside except blocks to capture full tracebacks automatically. It logs at ERROR level with `exc_info=True` implicitly—don't use it outside try-except.

### Common Patterns

| Pattern | Code |
|---|---|
| **Log exceptions with traceback** | `logger.exception("Error")` inside except block (auto includes traceback) |
| **Same format, different destinations** | `formatter.setFormatter(formatter)` for both file and console handlers |
| **Different levels per handler** | File handler at ERROR, console at INFO |
| **Context in logs** | `logger.info(f"User {user_id} action completed")` |

### basicConfig vs Handlers (Quick Comparison)

| Aspect | basicConfig | Handlers |
|---|---|---|
| Use case | Scripts, throwaway tools | Production, libraries |
| Destinations | One (all-or-nothing) | Many (stack them) |
| Logger | Root (global) | Named (scoped) |
| Flexibility | Low | High |
| Multiple handlers | ❌ No | ✅ Yes |
| Per-handler levels | ❌ No | ✅ Yes |

> **Interview tip:** "basicConfig for scripts, handlers for production. Use named loggers with multiple handlers when you need logs in more than one place or at different levels."

---

## 🔢 Integer Overflow & Hardware Bounds

### Standard Hardware Limits

| Data Type | Minimum Value | Maximum Value |
|---|---|---|
| **Signed 32-bit** (`int32`) | `-2,147,483,648` (`-2**31`) | `2,147,483,647` (`2**31 - 1`) |
| **Unsigned 32-bit** (`uint32`) | `0` | `4,294,967,295` (`2**32 - 1`) |
| **Signed 64-bit** (`int64`) | `-9,223,372,036,854,775,808` (`-2**63`) | `9,223,372,036,854,775,807` (`2**63 - 1`) |
| **Unsigned 64-bit** (`uint64`) | `0` | `18,446,744,073,709,551,615` (`2**64 - 1`) |

### Python Constants for Testing

```python
# 32-bit Bounds
INT32_MIN = -2**31      # -2,147,483,648
INT32_MAX = 2**31 - 1   #  2,147,483,647

# 64-bit Bounds
INT64_MIN = -2**63      # -9,223,372,036,854,775,808
INT64_MAX = 2**63 - 1   #  9,223,372,036,854,775,807
```

### Safe Algorithms (Prevent Overflow)

| Problem | Bad Approach | Safe Approach |
|---|---|---|
| Binary search midpoint | `mid = (low + high) // 2` | `mid = low + (high - low) // 2` |
| Average of two numbers | `avg = (x + y) // 2` | `avg = x + (y - x) // 2` or `(x & y) + ((x ^ y) >> 1)` |
| Multiply then divide | `result = (a * b) // c` | `result = (a // c) * b` |

### Pre-Check Validations (Before Operation)

**Multiplication overflow (A × B):**
```python
if A > 0 and B > 0 and A > (INT32_MAX // B):
    raise OverflowError("Multiplication overflow")
```

**Digit accumulation (result × 10 + digit):**
```python
# Positive bounds
if result > INT32_MAX // 10 or (result == INT32_MAX // 10 and digit > 7):
    return INT32_MAX

# Negative bounds
if result < INT32_MIN // 10 or (result == INT32_MIN // 10 and digit < -8):
    return INT32_MIN
```

### Hardware Emulation (Wrap-Around)

**32-bit unsigned wrap:**
```python
wrapped = raw_value & 0xFFFFFFFF
```

**32-bit signed wrap:**
```python
def to_int32(val):
    val = val & 0xFFFFFFFF
    if val >= 0x80000000:
        val -= 0x100000000
    return val
```

---

## 🧮 Modular Arithmetic (MOD = 10^9 + 7)

| Operation | Formula | Purpose |
|---|---|---|
| Addition | `(A + B) % MOD` | Keep sums within bounds |
| Subtraction | `(A - B + MOD) % MOD` | Handle negatives correctly |
| Multiplication | `(A * B) % MOD` | Prevent overflow in intermediate steps |
| Division | `(A * pow(B, MOD - 2, MOD)) % MOD` | Modular inverse via Fermat's Little Theorem |

---

## 🔁 Generators

**What:** Function using `yield` instead of `return` that returns data one piece at a time on demand (lazy evaluation).

| Aspect | Generator Function | Generator Expression |
|---|---|---|
| **Syntax** | `def func(): yield value` | `(expr for item in iterable)` |
| **Memory** | O(1) — streams data | O(1) — lazy evaluation |
| **When to use** | Complex logic, state management | Simple transformations |
| **Example** | `def read_logs(file): yield line` | `(x*2 for x in range(1000000))` |
| **vs List** | Streams 10GB file safely | List comprehension loads all into RAM |

### return vs yield

| Keyword | Behavior | State |
|---|---|---|
| `return` | Terminates function, returns entire result | Destroys local variables |
| `yield` | Pauses function, returns single value | Freezes execution state, resumes next call |

**Production example:**
```python
# Stream massive log file safely (no OOM crash)
def stream_logs(file_path: str):
    with open(file_path, "r") as file:
        for line in file:
            if "ERROR" in line:
                yield line.strip()  # Return one line at a time

# Usage — file NOT loaded into memory
for error in stream_logs("massive_10gb.log"):
    process(error)  # Process one error per iteration
```

### Generator Exhaustion (Important!)

Generators are **one-shot**: once consumed, they're empty. Must re-instantiate to iterate again.

```python
gen = (x*2 for x in range(3))
list(gen)  # [0, 2, 4]
list(gen)  # [] ← Empty! Generator exhausted

# Must create new generator
gen = (x*2 for x in range(3))
list(gen)  # [0, 2, 4] ← Works
```

> **Interview tip:** "Generators trade CPU cycles for space savings—O(1) memory regardless of dataset size. Use for file streaming, API pagination, live event processing. Remember: generators exhaust after one pass."

---

## ⚙️ Async & Multithreading

### Quick Comparison

| Aspect | Asynchronous (asyncio) | Multithreading |
|---|---|---|
| **Analogy** | One efficient cook | Multiple cooks sharing kitchen |
| **Threads** | Single thread | Multiple threads |
| **Execution** | Cooperative (yield control) | Preemptive (OS scheduler) |
| **Best For** | I/O-bound (network, database, file) | CPU-bound heavy processing |
| **Race Conditions** | ❌ No (single-threaded) | ⚠️ Yes (need locks) |
| **Shared Memory** | N/A | ⚠️ Requires synchronization |
| **Complexity** | Low | High |
| **Syntax** | `async def`, `await` | `Thread()`, `.start()` |

### Real-World Use Cases

| Scenario | Use Async | Use Threading |
|---|---|---|
| Loading feed from server (wait 2s, UI stays responsive) | ✅ Yes | ❌ No |
| Image filter on millions of pixels (heavy CPU math) | ❌ No | ✅ Yes |
| Web scraping 100 URLs simultaneously | ✅ Yes | ❌ No |
| Image processing with parallel threads | ❌ No | ✅ Yes |
| Database query while keeping UI responsive | ✅ Yes | ❌ No |

> **Rule of thumb:** Async for waiting (I/O), Threading for computing (CPU).

---
