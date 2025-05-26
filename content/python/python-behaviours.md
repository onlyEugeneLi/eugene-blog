### Boolean operators `or` & `and` behaviours

[StackOverflow: Python: justification for boolean operators (and, or) not returning booleans](https://stackoverflow.com/questions/69510472/python-justification-for-boolean-operators-and-or-not-returning-booleans#:~:text=In%20Python%20\(and%20some%20other,falsey%2C%20then%20x%20%2C%20otherwise%20y)

> `x or y`: if `x` is falsey, then `y`, otherwise `x` 

`or`: returns whichever is true-ish; if **both falsey**, return the **first** one

> `x and y` if `x` is falsey, then `x`, otherwise `y`

`and`: returns whichever is falsey; if **both true**, return the **latter** one