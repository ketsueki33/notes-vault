***Map of Content***
- [[#Introduction]]
- [[Variables & Data Types]]
- [[Strings]]
- [[Lists]]
- [[Tuples & Sets]]

TODO:-
- Dictionaries 
- Functions
- Conditionals 
- Loops
- Modules 
- Classes
- Files
- JSON 

**Reference Video -** [video](https://youtu.be/JJmcL1N2KQs)

### Introduction

#### Difference from other languages
**1. There are no semi-colons**

**2. There are no keywords to declare variables**
*Example)* The correct way to declare a variable to store a number is:
```py 
x = 1 # not `var x = 1` 
```

**3. Python typecasts the variables according to the value we assign by default:**
Here, name will be given the type of `str` by Python.
```py
name = "John"
```

#### Printing to Console
`print` function is used to print anything to the console
```py
print("Hello")  # Hello

x, y, name, is_cool = (1, 2.5, "John", True)

print(x, y, name, is_cool)  # 1 2.5 John True
```
#### Comments in Python
**Single line comments**
`#` is used at the beginning for a single line comment
```py
# This is a single line comment
```

**Multi line comment**
They are also called *docstring*. Three single or double quotes are used ( `'''` or `"""`) to wrap the lines we want to comment out.
```py
'''
This is a 
multiline comment
or docstring (used to define a functions purpose)
can be single or double quotes
'''
```

