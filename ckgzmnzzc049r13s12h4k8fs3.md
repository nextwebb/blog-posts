# Introduction To Linked-list In Python

## Introduction
There are situations when the allocation of memory to store data cannot be in a contiguous block of memory or stored sequentially in memory,
such situations include When :
- You need constant insertions and deletions from the list (such as in real-time computing where time predictability is absolutely critical).
-  You don't know how many items will be in the list.
- You don't need random access to any elements
- You want to be able to insert items in the middle of the list (such as a priority queue) and Et cetera.

We implement data structures such as ``` linked list ``` with nodes. A node is simply a data element which consists of two parts: *data & pointer to the next node.*

It can consist of address data, geographical data, geometric data, routing information, or transaction details, etc. 
So nodes help us to know the address of the next data element from the values of the current data element. 

![python-linked-lists-1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1604261380071/tVOUfIaiX.png)


*Figure 1: A sample implementation of a Single-linked list*

 These data elements called nodes are linked using pointers, hence called a ```linked list``` In Python.



## Creation of Nodes for a Single-Linked List

Nodes are the foundations on which various other data structures ```linked lists``` and ```trees``` can be handled in Python.
The nodes are created by implementing a class that will hold the pointers along with the data element. 

In the example below, we created a class named "ListNode" to hold the name of  Countries. 
The "next" pointer is initialized to null and three nodes and initialized with values as shown.

The next pointer of node1 points to node3 while the next pointer of node3 points to node2.

```
class Countries(object):
    """
    the default dataval is set to NIL
    """
    def __init__(self, data=None):
        self.data = data
        self.next = None

"""
 Instantiation of nodes
and setting the data part of the node
to the input value and the next pointer to NIL
"""
node1 = Countries('Nigeria')
node2 = Countries('USA')
node3 = Countries('Kenya')

"""
 Here we're setting the next pointer of node1 to point to node3, and the next pointer of node3 to point to node2
"""
node1.next = node3
node3.next = node2

```

## Traversing the Node Elements

We can traverse the data elements/node of the ```linked list ``` created above by creating a variable and assigning the first node to it. Then we use a while loop and the ``` next``` pointer to print out all the node elements.

*** Note that we have one more additional data element ``` node4``` and the ```next``` pointers are properly arranged to get the output as countries orderly.***

```
class Countries(object):
    """
    the default dataval is set to NIL
    """
    def __init__(self, data=None):
        self.data = data
        self.next = None

"""
 Instantiation of nodes
and setting the data part of the node
to the input value and the next pointer to NIL
"""
node1 = Countries('Nigeria')
node2 = Countries('USA')
node3 = Countries('Kenya')
node4 = Countries('Canada')

"""
 Here we're setting the next pointer of node1 to point to node3,
 and the next pointer of node3 to point to node2
"""
node1.next = node3
node3.next = node2
node2.next = node4


thisvalue = node1

while thisvalue:
        print(thisvalue.data)
        thisvalue = thisvalue.next


```
When the above code is executed, the resulting output is given below:
```
Nigeria
Kenya
USA
Canada
```

Additional operations like Adding Nodes, Removing Nodes, Searching the list and double-linked lists can be done by implementing appropriate methods using these node containers in the general data structures like linked lists and trees.

There you go with a practical introduction to linked-lists in Python 
😀.

Thanks for the audience and I hope you found this article helpful 🤗. Feel free to reach out to me on  [Github](https://github.com/nextwebb), [Twitter](https://twitter.com/i_am_nextwebb) and [LinkedIn](https://www.linkedin.com/in/peterson-oaikhenah-102645144/).
Do drop a like, comment, and share 😌.

