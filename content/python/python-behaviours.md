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

```
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

```
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

```
diff = ord('B') - ord('A')
print(diff)
```
> \> 1

## `bisect()`

This function returns the position where a number should be inserted to keep the list sorted. If the number already exists, it returns the rightmost insertion point.

```
bisect.bisect(list, num, beg=0, end=len(list))
```

## String 

### `.startswith()`

`startswith()` method in Python checks whether a given string starts with a specific prefix. 

```
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
```0