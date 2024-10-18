Strings in python are surrounded by either single or double quotation marks. Let's look at string formatting and some string methods.

#### String Formatting
##### Arguments by Position
```py
name = "John Cena"
age = 37

print("My name is {name} and I am {a} years old.".format(name=name, a=age))
# My name is John Cena and I am 37 years old.
```

##### F-Strings
Similar to string template literals in JavaScript.
```py
name = "John Cena"
age = 37

print(f"My name is {name} and I am {age} years old.")
# My name is John Cena and I am 37 years old.
```

#### String Methods
###### Capitalize String
```py
s = "hello world"

print(s.capitalize()) # Hello world
```

###### Upper/Lower case
```py
s = "hello world"

print(s.upper()) # HELLO WORLD

print(s.lower()) # hello world
```

###### Swap case
The _swapcase_() method returns a string where all the upper case letters are lower case and vice versa
```py
s = "HELLO world"

print(s.swapcase())  # hello WORLD
```

###### Length of String
*len()* can be used to get length of strings, lists and other data structures.
```py
s = "HELLO world"

print(len(s)) # 11
```

###### Replace substring
```py
s = "HELLO world"

print(s.replace("world", "everyone")) # HELLO everyone
```

###### Count instances of substring
```py
s = "Hello Cello Yellow"

print(s.count("ello")) # 3
```

###### Starts with substring
`startswith()` checks if a string starts with a specified prefix.
```py
s = "HELLO world"

print(s.startswith("HELLO")) # True
```

###### Ends with substring
`endswith()` checks if a string ends with a specified suffix.
```py
s = "HELLO world"

print(s.endswith("world")) # True
```

###### Split into list
`split()` breaks a string into a list, using a specified separator. If no separator is specified, it separates at white spaces.
```py
s = "HELLO world"

print(s.split()) # ['HELLO', 'world']
```

###### Is alphanumeric
`isalnum()` checks if all characters in the string are alphanumeric (letters and numbers).
```py
s = "Hello123"

print(s.isalnum()) # True
```
Similary, `isalpha()` checks if all characters in the string are alphabetic (only letters) and `isnumeric()` checks if all characters in the string are numeric (only numbers).