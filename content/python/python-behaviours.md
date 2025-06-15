# Boolean operators

[StackOverflow: Python: justification for boolean operators (and, or) not returning booleans](https://stackoverflow.com/questions/69510472/python-justification-for-boolean-operators-and-or-not-returning-booleans#:~:text=In%20Python%20\(and%20some%20other,falsey%2C%20then%20x%20%2C%20otherwise%20y)

### `or` behaviours

> `x or y`: if `x` is falsey, then `y`, otherwise `x` 

`or`: returns whichever is true-ish; if **both falsey**, return the **first** one

### `and` behaviours

> `x and y` if `x` is falsey, then `x`, otherwise `y`

`and`: returns whichever is falsey; if **both true**, return the **latter** one

# Arithmetic

## Ceiling Division

$$
ceil(\frac{x}{y}) = \frac{x + y - 1}{y}
$$


# Funtions

## Sorting

### `sort()` -- in place sort

The `sort()` method sorts the list ascending by default.

```python
sort(reverse=True|False, key=myFunc)
```

* `myFunc()`
    ```
    # A function that returns the length of the value:
    def myFunc(e):
    return len(e)

    cars = ['Ford', 'Mitsubishi', 'BMW', 'VW']

    cars.sort(key=myFunc)
    ```


### `sorted()` -- create new sorted list

```python
a = ["banana", "apple", "cherry"]

# Sorting the list
res = sorted(a)
print(res)
```

### Lexicographical order

Dicitonary order -- the way in which words are ordered in a dictionary

For example, the word *"children"* will appear before (and can be considered smaller) than the word *"chill"* because the first four letters of the two words are the same but the letter at the *fifth* position in "children" (i.e. *d* ) comes before (or is smaller than) the letter at the *fifth* position in "chill" (i.e. *l* ). 

## `ord()`

`ord()` gives you integer representation of a character. Take a look at an *ASCII table* to find out what they are. *'A'* has an ASCII value of *65*, *'B'* has an ASCII value of *66*, and so on.

```python
diff = ord('B') - ord('A')
print(diff)
```
> \> 1

## `bisect()`

This function returns the position where a number should be inserted to keep the list sorted. If the number already exists, it returns the rightmost insertion point.

```python
bisect.bisect(list, num, beg=0, end=len(list))
```

## 🔠 String 

### `.startswith()`

`startswith()` method in Python checks whether a given string starts with a specific prefix. 

```python
startswith("for")

# check the string using the start parameter to begin from a specific index
s.startswith("for", 5)

# check if the string starts with any one of several prefixes by passing a tuple of prefixes
s.startswith(("Geeks", "G"))
```

### `.endswith()`

Returns “true” when the last letters of a string match another string

### `.find()`

Returns index of the first occurrence of a substring; if the substring does not occur, -1 is returned.

[DigitalOcean - Join Python List: Techniques, Examples, and Best Practices](https://www.digitalocean.com/community/tutorials/python-join-list)

### `join()` function


```python
vowels = ["a", "e", "i", "o", "u"]

vowelsCSV = ",".join(vowels)

# vowelsCSV = "a,e,i,o,u" (String)
```

> 🚫 Multiple data-types cannot be combined into a single String with join() function.

### `split()` function

```python
split = vowelsCSV.split(',')
print('List: {0}'.format(split))

# List: ["a", "e", "i", "o", "u"]
```

### `format()` function

💡 [W3School - Python String format() Method](https://www.w3schools.com/python/ref_string_format.asp)

#### Insert values into strings

```python
txt1 = "My name is {fname}, I'm {age}".format(fname = "John", age = 36)
txt2 = "My name is {0}, I'm {1}".format("John",36)
txt3 = "My name is {}, I'm {}".format("John",36)
```

#### Format numbers -- decimals display

```python
txt = "For only {price:.2f} dollars!"
print(txt.format(price = 49))
```

| Specifier | Description                                                                 |
|-----------|-----------------------------------------------------------------------------|
| `:<`      | Left aligns the result (within the available space)                         |
| `:>`      | Right aligns the result (within the available space)                        |
| `:^`      | Center aligns the result (within the available space)                       |
| `:=`      | Places the sign to the leftmost position                                    |
| `:+`      | Use a plus sign to indicate if the result is positive or negative           |
| `:-`      | Use a minus sign for negative values only                                   |
| `: `      | Use a space to insert an extra space before positive numbers                |
| `:,`      | Use a comma as a thousand separator                                        |
| `:_`      | Use an underscore as a thousand separator                                  |
| `:b`      | Binary format                                                               |
| `:c`      | Converts the value into the corresponding Unicode character                |
| `:d`      | Decimal format                                                              |
| `:e`      | Scientific format (lowercase `e`)                                           |
| `:E`      | Scientific format (uppercase `E`)                                           |
| `:f`      | Fixed-point number format                                                   |
| `:F`      | Fixed-point format (uppercase, shows `INF`/`NAN`)                           |
| `:g`      | General format (auto-chooses `e` or `f`)                                    |
| `:G`      | General format (uses uppercase `E` for scientific)                          |
| `:o`      | Octal format                                                                |
| `:x`      | Hex format (lowercase)                                                      |
| `:X`      | Hex format (uppercase)                                                      |
| `:n`      | Number format (locale-aware, uses separators)                               |
| `:%`      | Percentage format                                                           |

Notes

* **Alignment** specifiers (`:<`, `:>`, `:^`, `:=`) work with a width value (e.g., `f"{x:<10}"`)

---

## 📚 Permutations and Combinations

### `itertools.product()`

Syntax 

`itertools.product(*iterables, repeat=1)`

```
from itertools import product
print(list(product("AB", "CD")))
```

Using the repeat parameter of the product() function to generate the Cartesian product of the list [0, 1] repeated 3 times.
```
from itertools import product
print(list(product([0, 1], repeat=3)))
```
--- 

## 🔄 Iterables

### `enumerate()`

enumerate() function adds a counter to each item in a list or other iterable

```
a = ["Geeks", "for", "Geeks"]

# Iterating list using enumerate to get both index and element
for i, name in enumerate(a):
    print(f"Index {i}: {name}")

# Converting to a list of tuples
print(list(enumerate(a)))

# Index 0: Geeks
# Index 1: for
# Index 2: Geeks
# [(0, 'Geeks'), (1, 'for'), (2, 'Geeks')]
```

# 🧠 Bit Manipulation


## 🔧 Bitwise Operators

| Operator     | Symbol | Description                     | Example (`a = 5`, `b = 3`)     | Result |
|--------------|--------|---------------------------------|--------------------------------|--------|
| AND          | `&`    | Bitwise AND                     | `a & b` → `0b0101 & 0b0011`    | `1`    |
| OR           | `|`    | Bitwise OR                      | `a | b` → `0b0101 | 0b0011`    | `7`    |
| XOR (exclusive or) | `^`    | Bitwise XOR                     | `a ^ b` → `0b0101 ^ 0b0011`    | `6`    |
| NOT          | `~`    | Bitwise NOT (1's complement)    | `~a` → `~0b0101`               | `-6`   |
| Left Shift   | `<<`   | Shift bits left (multiply by 2) | `a << 1` → `0b1010`            | `10`   |
| Right Shift  | `>>`   | Shift bits right (divide by 2)  | `a >> 1` → `0b0010`            | `2`    |
---
**XOR (Exclusive OR)**
* Mainly used to detect difference
* same -> 0, different -> 1
* 0 remains 0, 1 stands out
* 去同存异

**Mask `mask = i << i`**

i -- for loop index

* Used to detect whether a bit is 1 `a & mask`

---

## 🧮 Common Bit Operations

### 1. Check if a bit is set
```python
n = 5  # 0b0101
bit = 2
is_set = (n & (1 << bit)) != 0
```

### 2. Set a bit

```python
n = 5  # 0b0101
bit = 1
n |= (1 << bit)  # Result: 0b0111 (7)
```

### 3. Clear a bit

```python
n = 5  # 0b0101
bit = 0
n &= ~(1 << bit)  # Result: 0b0100 (4)
```

### 4. Count set bits (Hamming weight)

```python
bin(13).count('1')  # 13 = 0b1101 → 3 set bits
```

### 5. Check if a number is a power of 2

```python
n = 8
is_power_of_two = n > 0 and (n & (n - 1)) == 0
```

# 📂 File handling

## Example question | Meta -- CSV Dinosaurs:

You will be supplied with two data files in CSV format . The first file contains statistics about various dinosaurs. The second file contains additional data. Given the following formula, speed = ((STRIDE_LENGTH / LEG_LENGTH) - 1) * SQRT(LEG_LENGTH * g) Where g = 9.8 m/s^2 (gravitational constant)

Write a program to read in the data files from disk, it must then print the names of only the bipedal dinosaurs from fastest to slowest. Do not print any other information.

```
$ cat dataset1.csv
NAME,LEG_LENGTH,DIET
Hadrosaurus,1.4,herbivore
Struthiomimus,0.72,omnivore
Velociraptor,1.8,carnivore
Triceratops,0.47,herbivore
Euoplocephalus,2.6,herbivore
Stegosaurus,1.50,herbivore
Tyrannosaurus Rex,6.5,carnivore

$ cat dataset2.csv 
NAME,STRIDE_LENGTH,STANCE
Euoplocephalus,1.97,quadrupedal
Stegosaurus,1.70,quadrupedal
Tyrannosaurus Rex,4.76,bipedal
Hadrosaurus,1.3,bipedal
Deinonychus,1.11,bipedal
Struthiomimus,1.24,bipedal
Velociraptorr,2.62,bipedal
```


### Solution

```python
# Facebook | Phone Screen | CSV Dinosaurs & Split Array Equal Sum

import math
import heapq
def CSVDinosaurs(path1, path2):
    g = 9.8
    stride_map = {}
    with open(path2, 'r') as f:
        line = f.readline()
        line = f.readline()
        while line:
            tmp = line.split(",")
            name, stride, stance = tmp
            if stance.strip() == "bipedal":
                stride_map[name] = float(stride)
            line = f.readline()

    outmap = {}
    with open(path1, 'r') as f:
        line = f.readline()
        line = f.readline()
        while line:
            tmp = line.split(",")
            name, leg, diet = tmp
            leg = float(leg)
            if name in stride_map:
                stride = stride_map[name]
                outmap[name] = (stride/leg-1)*math.sqrt(leg*g)
                print(outmap[name])
            line = f.readline()

    res = []
    for name in outmap:
        res.append((-outmap[name], name))
    heapq.heapify(res)
    while res:
        speed, name = heapq.heappop(res)
        print(name + ", " + str(-speed))

path1 = "dataset1.csv"
path2 = "dataset2.csv"
CSVDinosaurs(path1, path2)
```


# Concurrency and Threading

## `yield`

[StackOverflow - Generators and Yield](https://stackoverflow.com/a/231855)



## `time.sleep(0)`

Parameter unit is **second**.

When time is set to 0, thread will be paused without taking up system time. 

## `Lock`

```python
from threading import Lock
```

[Python Documentation - Threading](https://docs.python.org/3/library/threading.html#lock-objects:~:text=It%20has%20two,Locks)

When the state is locked, acquire() blocks until a call to release() in another thread changes it to unlocked, then the acquire() call resets it to locked and returns. The release() method should only be called in the locked state

### Example

[Leetcode - 1115. Print FooBar ALternately](https://leetcode.com/problems/print-foobar-alternately?envType=company&envId=apple&favoriteSlug=apple-all)

```python
from threading import Lock
class FooBar:
    def __init__(self, n):
        self.n = n
        self.foo_lock = Lock()
        self.bar_lock = Lock()
        self.bar_lock.acquire()


    def foo(self, printFoo: 'Callable[[], None]') -> None:
        for i in range(self.n):
            self.foo_lock.acquire()
            # printFoo() outputs "foo". Do not change or remove this line.
            printFoo()
            self.bar_lock.release()


    def bar(self, printBar: 'Callable[[], None]') -> None:
        for i in range(self.n):
            self.bar_lock.acquire()
            # printBar() outputs "bar". Do not change or remove this line.
            printBar()
            self.foo_lock.release()
```