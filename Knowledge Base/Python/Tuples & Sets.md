### Tuples
A Tuple is a collection which is ordered and **unchangeable**. It **allows duplicate** members.

#### Creating a Tuple
```py
numbers = (2, 5, 1, 2)

# using a constructor
numbers2 = (2, 5, 1, 2)

print(numbers, numbers2)  # (2, 5, 1, 2) (2, 5, 1, 2)
```


> [!NOTE] Tuples with single value
> When creating a tuple with a single value, add a trailing comma otherwise Python will create a `str` or `int` etc. depending on the value.]
> ```py
> numbers3 = (1,)
> ```

#### Getting a value
```py
t = (10, 20, 30)

print(t[1]) # 20
```

> [!Important]  The value of a tuple item cannot be changed. We will get a type error if we try to reassign.

#### Tuple Methods
###### Length of Tuple

`len()` can be used to get the number of elements in a tuple.

```py
t = (1, 2, 3, 4)

print(len(t)) # 4
```

###### Count instances of a value

`count()` returns the number of times a specified value appears in the tuple.
```py
t = (1, 2, 2, 3, 4, 2)

print(t.count(2)) # 3
```

###### Find index of a value

`index()` returns the first index of the specified value. If the value is not found, it raises an error.
```py
t = (1, 2, 3, 4, 5)

print(t.index(3)) # 2
```

###### Check if element exists

You can use the `in` operator to check if an element exists in a tuple.
```py
t = (1, 2, 3, 4)

print(3 in t) # True
```

###### Concatenate two tuples

You can use the `+` operator to join two tuples.
```py
t1 = (1, 2)
t2 = (3, 4)

print(t1 + t2) # (1, 2, 3, 4)
```

###### Repeat a tuple

You can use the `*` operator to repeat the elements in a tuple.
```py
t = (1, 2)

print(t * 3) # (1, 2, 1, 2, 1, 2)
```

### Sets
A Set is a collection which is unordered and unindexed. There can be **no duplicate members**.

#### Creating a set
```py
fruits = {"Apple", "Pear", "Mango"}

# using a constructor
fruits2 = set(("Apple", "Pear", "Mango"))
```

#### Set Methods
###### Length of Set

`len()` can be used to get the number of elements in a set.
```py
s = {1, 2, 3, 4}

print(len(s)) # 4
```

###### Add an element to a set

`add()` adds a single element to a set.
```py
s = {1, 2, 3}

s.add(4)
print(s) # {1, 2, 3, 4}
```

**If we try to add duplicate elements, nothing will change. There will be no error.**
###### Remove an element from a set

`remove()` removes the specified element from the set. If the element does not exist, it raises an error.
```py
s = {1, 2, 3}

s.remove(2)
print(s) # {1, 3}
```

###### Discard an element

`discard()` removes an element if it exists, and does nothing if it doesn't (no error is raised).
```py
s = {1, 2, 3}

s.discard(4)
print(s) # {1, 2, 3}
```

###### Pop a random element

`pop()` removes and returns an arbitrary element from the set. If the set is empty, it raises an error.
```py
s = {1, 2, 3}

print(s.pop()) # Might return 1, 2, or 3
```

###### Clear all elements from the set

`clear()` removes all elements from the set, leaving it empty.
```py
s = {1, 2, 3}

s.clear()
print(s) # set()
```

###### Union of sets

`union()` returns a set containing all elements from both sets (duplicates are removed).
```py
s1 = {1, 2, 3}
s2 = {3, 4, 5}

print(s1.union(s2)) # {1, 2, 3, 4, 5}
```

###### Intersection of sets

`intersection()` returns a set containing only the elements that are common to both sets.
```py
s1 = {1, 2, 3}
s2 = {2, 3, 4}

print(s1.intersection(s2)) # {2, 3}
```

###### Difference between sets

`difference()` returns a set containing elements in the first set that are not in the second set.
```py
s1 = {1, 2, 3}
s2 = {2, 3, 4}

print(s1.difference(s2)) # {1}
```

Check if subset

issubset() checks if one set is a subset of another.
```py
s1 = {1, 2}
s2 = {1, 2, 3, 4}

print(s1.issubset(s2)) # True
```

###### Check if superset

`issuperset()` checks if one set is a superset of another.
```py
s1 = {1, 2, 3, 4}
s2 = {2, 3}

print(s1.issuperset(s2)) # True
```