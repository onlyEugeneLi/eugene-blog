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

---

## ⚡ map() & lambda

| Concept | Syntax | Example | Output |
|---|---|---|---|
| `lambda` | `lambda args: expression` | `square = lambda x: x**2` | Function object |
| `map()` | `map(func, iterable)` | `list(map(lambda x: x*2, [1,2,3]))` | `[2, 4, 6]` |
| `filter()` | `filter(func, iterable)` | `list(filter(lambda x: x>2, [1,2,3,4]))` | `[3, 4]` |
| List comprehension | `[expr for item in iter if cond]` | `[x*2 for x in [1,2,3] if x>1]` | `[4, 6]` |

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
