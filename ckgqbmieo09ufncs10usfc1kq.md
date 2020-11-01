## Python Techniques - How to Check If Multiple Keys Exists In A Dictionary?

## Introduction

In Python, dictionaries are written with curly braces ```{}``` and they have one or more unordered key-value pairs. The values of a dictionary can be of any data type and can be duplicated, but the keys or names can’t be repeated and must be immutable. 
The implementation of Hash tables or hash maps in python is through the built-in dictionary data-type, as key-value pairs. 


**Example  **

*Create and print a dictionary:*
```
person =	{
  "name": "nextwebb"
  "favColor": "red",
  "hobby": "Gaming"
}
print(person)
```

*Accessing the keys of a dictionary in python*

```
x = person["name"]
 >>>  nextwebb
```

Let’s discuss the subject matter,  which is how to check multiple keys in a dictionary in a single pass?  :

**Method #1  Using sets: **

*According to the [doc.python.org](https://docs.python.org/3/library/stdtypes.html#set-types-set-frozenset)*

> A set object is an unordered collection of distinct hashable objects. Common uses include membership testing, removing duplicates from a sequence, and computing mathematical operations such as intersection, union, difference, and symmetric difference.

In our case, we can make the input values or arguments into a set object, and check if the given set,  is a subset or superset of another set. The resulting output will be  ```True or False ``` depending on the elements present in the sets.

*here's an example:* 
```
# dictionary or hashmap in python
person =	{
  "name": "nextwebb",
  "favColor": "red",
  "hobby": "Gaming"
}

if set(("name", "favColor")) <= set(person):
   pass 

```
*Alternatively*

```
if set(("name", "favColor")).issubset(person):
   pass 
```

**Method #2  Using if, all statement: **

In this method, we will check if all the key elements that we want to compare can be found in the above dictionary or not.

```
if all (key in person for key in ("name", "favColor")):
    pass

```

There you go, with two elegant methods to check for multiple keys in a dictionary or hashtable in python 😀 .

I'd love to see the awesome solutions you come up with  👇 👇.  


Thanks for the audience and I hope you found this article helpful 🤗. Feel free to reach out to me on  [Github](https://github.com/nextwebb), [Twitter](https://twitter.com/i_am_nextwebb) and [LinkedIn](https://www.linkedin.com/in/peterson-oaikhenah-102645144/).
Do drop a like, comment, and share 😌.
 