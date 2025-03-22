**Python Data Structure Methods**

### **List Methods**
| Method | Description |
|--------|-------------|
| `len(list)` | Returns the number of elements in the list |
| `list[index]` | Accesses an element by its index |
| `list[start:end]` | Retrieves a slice of the list |
| `list.append(value)` | Adds a value to the end of the list |
| `list.insert(index, value)` | Inserts a value at a specified index |
| `list.remove(value)` | Removes the first occurrence of a specified value |
| `list1 + list2` | Concatenates multiple lists |
| `[expression for item in iterable if condition]` | List comprehension for creating lists based on conditions |

### **Tuple Methods**
| Method | Description |
|--------|-------------|
| `len(tuple)` | Returns the number of elements in the tuple |
| `tuple[index]` | Accesses an element by its index |
| `tuple[start:end]` | Retrieves a slice of the tuple |
| `tuple1 + tuple2` | Concatenates multiple tuples |

### **Dictionary Methods**
| Method | Description |
|--------|-------------|
| `dict.keys()` | Returns all keys in the dictionary |
| `dict.values()` | Returns all values in the dictionary |
| `dict.items()` | Returns all key-value pairs in the dictionary |
| `dict.get(key)` | Retrieves a value using a key |
| `dict.clear()` | Removes all key-value pairs from the dictionary |
| `dict.copy()` | Creates a copy of the dictionary |
| `dict.pop(key)` | Removes and returns the value associated with a key |
| `dict.update(new_dict)` | Updates the dictionary with key-value pairs from another dictionary |
| `key in dict.keys()` | Checks if a key exists in the dictionary |

### **Set Methods**
| Method | Description |
|--------|-------------|
| `set.add(value)` | Adds a value to the set |
| `set.update(other_set)` | Adds elements from another set |
| `set.discard(value)` | Removes a value without raising an error if it does not exist |
| `set.remove(value)` | Removes a value and raises an error if it does not exist |
| `set.clear()` | Removes all elements from the set |
| `set.pop()` | Removes and returns an arbitrary element from the set |
| `del set` | Deletes the entire set |
| `set.difference(other_set)` | Returns a set containing the difference between two sets |
| `set.intersection(other_set)` | Returns a set containing common elements between sets |
| `set.union(set1, set2, …)` | Combines multiple sets into one |

This document provides a structured summary of methods for lists, tuples, dictionaries, and sets in Python.




**Comparison of Python Data Structures: List, Tuple, Dictionary, and Set**

This document provides a comprehensive comparison of Python's fundamental data structures: **List**, **Tuple**, **Dictionary**, and **Set**.

| Feature         | **List (`list`)**                     | **Tuple (`tuple`)**                  | **Dictionary (`dict`)**                          | **Set (`set`)**                        |
|---------------|--------------------------------|--------------------------------|------------------------------------------|--------------------------------|
| **Definition** | Ordered, mutable collection | Ordered, immutable collection | Key-value pairs, unordered | Unordered, unique elements |
| **Syntax** | `[1, 2, 3]` | `(1, 2, 3)` | `{"key": "value"}` | `{1, 2, 3}` |
| **Mutable?** | ✅ Yes (can change elements) | ❌ No (cannot change elements) | ✅ Yes (can modify values) | ✅ Yes (can add/remove items) |
| **Allows Duplicates?** | ✅ Yes | ✅ Yes | ❌ No (keys must be unique) | ❌ No |
| **Indexed?** | ✅ Yes (zero-based index) | ✅ Yes | ❌ No (access via keys) | ❌ No |
| **Ordered?** | ✅ Yes (maintains order) | ✅ Yes (maintains order) | ✅ Yes (from Python 3.7+) | ❌ No |
| **Declaration Example** | `my_list = [1, 2, 3]` | `my_tuple = (1, 2, 3)` | `my_dict = {"a": 1, "b": 2}` | `my_set = {1, 2, 3}` |
| **Access Elements** | `my_list[0]` → `1` | `my_tuple[0]` → `1` | `my_dict["a"]` → `1` | ❌ No direct indexing (use loops) |
| **Add Elements** | `my_list.append(4)` | ❌ Not possible (immutable) | `my_dict["c"] = 3` | `my_set.add(4)` |
| **Remove Elements** | `my_list.remove(2)` | ❌ Not possible (immutable) | `del my_dict["a"]` | `my_set.remove(2)` |
| **Modify Elements** | `my_list[1] = 99` | ❌ Not possible | `my_dict["a"] = 42` | ❌ No indexing |
| **Concatenation** | `list1 + list2` | `tuple1 + tuple2` | ❌ No direct concatenation | ❌ No direct concatenation |
| **Length** | `len(my_list)` | `len(my_tuple)` | `len(my_dict)` (keys count) | `len(my_set)` |
| **Check Existence** | `2 in my_list` | `2 in my_tuple` | `"a" in my_dict` (checks keys) | `2 in my_set` |
| **Iteration** | `for i in my_list:` | `for i in my_tuple:` | `for key, value in my_dict.items():` | `for i in my_set:` |
| **Sorting** | `sorted(my_list)` | `sorted(my_tuple)` | `sorted(my_dict.keys())` | `sorted(my_set)` |
| **Convert to Other Type** | `tuple(my_list)`, `set(my_list)`, `dict(enumerate(my_list))` | `list(my_tuple)`, `set(my_tuple)`, `dict(enumerate(my_tuple))` | `list(my_dict.keys())`, `set(my_dict.keys())`, `tuple(my_dict.keys())` | `list(my_set)`, `tuple(my_set)`, `dict(enumerate(my_set))` |
| **Copy** | `my_list.copy()` | `tuple(my_tuple)` | `my_dict.copy()` | `my_set.copy()` |
| **Clear All Items** | `my_list.clear()` | ❌ Not possible | `my_dict.clear()` | `my_set.clear()` |
| **Set Operations (Union, Intersection, Difference)** | ❌ Not supported | ❌ Not supported | ❌ Not supported | `set1.union(set2)`, `set1.intersection(set2)`, `set1.difference(set2)` |
| **When to Use?** | When order and mutability are needed | When order and immutability are needed | When key-value pairs are needed | When uniqueness is needed |

This table provides a **quick and complete** reference for working with Python **lists, tuples, dictionaries, and sets**. 🚀

