## Boolean operators

[StackOverflow: Python: justification for boolean operators (and, or) not returning booleans](https://stackoverflow.com/questions/69510472/python-justification-for-boolean-operators-and-or-not-returning-booleans#:~:text=In%20Python%20\(and%20some%20other,falsey%2C%20then%20x%20%2C%20otherwise%20y)

### `or` behaviours

> `x or y`: if `x` is falsey, then `y`, otherwise `x` 

`or`: returns whichever is true-ish; if **both falsey**, return the **first** one

### `and` behaviours

> `x and y` if `x` is falsey, then `x`, otherwise `y`

`and`: returns whichever is falsey; if **both true**, return the **latter** one

## Arithmetic

### Ceiling Division

$$
ceil(\frac{x}{y}) = \frac{x + y - 1}{y}
$$


## Funtions

### `sort()`

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