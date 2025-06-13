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

## `sort()`

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