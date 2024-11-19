A list is a collection which is ordered and changeable. It alllows duplicate members. They are the equivalent to arrays in other languages.

#### Creating a list
```py
numbers = [1, 2, 3, 4, 5]
```

**You can also use the list constructor:**
```py
numbers2 = list((1, 2, 3, 4, 5))
```

#### Accessing a value
```py
fruits = ["Apple", "Mango", "Pear", "Orange"]

print(fruits[1])  # Mango

fruits[1] = "Pineapple"
```

#### Getting length
```py
len(fruits)
```

#### List Methods
###### Appending values
```py
fruits.append("Mango")
```

###### Removing from list
The `remove()` method removes the first matching element (which is passed as an argument) from the list.
```py
fruits.remove("Pear")
```

###### Insert into position
The `insert()` method inserts an element to the list at the specified index.
```py
fruits.insert(2, "Strawberry")
```

###### Removing from position
The list `pop()` method removes the item at the specified index. The method also returns the removed item.
- The `pop()` method takes a single argument (index).
- The argument passed to the method is optional. If not passed, the default index **-1** is passed as an argument (index of the last item).
```py
fruits.pop(2, "Strawberry")
```

###### Reversing the list
```py
fruits.reverse()
```

###### Sorting the list
The `sort()` method can take two optional keyword arguments:

- **reverse** - By default `False`. If `True` is passed, the list is sorted in descending order.
- **key** - Comparison is based on this function.
```py
fruits.sort()

fruits.sort(reverse = True)
```

**Ex) Sorting according to length of strings**
```py
fruits = ["Apple", "Mango", "Pear", "Fig", "Watermelon"]

fruits.sort(key=len) 

print(fruits)  # ['Fig', 'Pear', 'Apple', 'Mango', 'Watermelon']
```