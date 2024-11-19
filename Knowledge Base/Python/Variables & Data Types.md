#### Variable Rules
- Variable names are case sensitive (name and NAME are different variables)
- Must start with a letter or an underscore
- Can have numbers but can not start with one

#### Data Types
**1. int**
```py
x = 1 # int
```

**2. float**
```py
y = 2.5 #float
```

**3. str (string)**
```py
name = "John" #str 
```

**4. bool (boolean)**
```py
is_cool = True #bool
```

> [!NOTE] Capitalised Boolean
> `True` and `False` are recognised by Python.
> `true` and `false` are not reserved keywords in Python.


#### Multiple Assignment
It is possible to do assignments of multiple variables at once even if they have different data types.
```py
x, y, name, is_cool = (1, 2.5, "John", True)
```

#### Type Casting
We can override the default type given to a variable by type casting them.
```py
x = 1  # int
x = str(x)
print(type(x), x)  # <class 'str'> 1

y = 2.5 #float
y = int(y)
print(type(y), y)  # <class 'int'> 2
```