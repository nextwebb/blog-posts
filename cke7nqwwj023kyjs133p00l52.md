## Python techniques: Introduction to Python list comprehension




## WHY LOOPS 🙄?
Loops essentially allow you to repeat a process over and over without having you write the same expression or condition, each time you want the program to perform a specific task.

![tine-ivanic-u2d0BPZFXOY-unsplash.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1598219027319/mpKdRkMIV.jpeg)
*<span>Photo by <a href="https://unsplash.com/@tine999?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Tine Ivanič</a> on <a href="https://unsplash.com/s/photos/loop?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Unsplash</a></span>*

Suppose, we want to separate the letters of the string "programming" and add the letters as items of a new list. The first thing that comes to mind, might be writing a 
"for loop" 😀 for this operation. 

**Example 1: Iterating through a string Using for Loop**

```
p_letters = []
word = 'programming'
for letter in word:
    p_letters.append(letter)

print(p_letters)
``` 
When we run the program, the output will be:
```
> ['p', 'r', 'o', 'g', 'r', 'a', 'm', 'm', 'i', 'n', 'g']
 ```

However, Python has an easier way to solve this issue using "List Comprehension". 

List comprehension is an elegant and concise way to define and create lists, based on existing lists, tuples, or strings 😎. It's powerful because it can auto-detect the type of iterable passed to it, which can be string, tuple, or list.  
> Syntax of List Comprehension

```
[expression for an item in list]
``` 
Let’s see how the above program can be written using list comprehensions.

**Example 2: Iterating through a string using list comprehension technique **
 
```
p_letters = [ letter for letter in 'programming' ]
print( p_letters)
``` 
When we run the program, the output will be the same as example 1. 

In the above example, a new list is assigned to variable p_letters, as indicated by the square brackets. this new list contains the items of the iterable and in this case, it's a  string 'programming'. Then We call a print() function to show the output.

 ### Another use-case that prompts some form of iteration, might be getting all the sub-strings of a word and appending them as the items of a new list 🤔. 

Implementing this operation in python code using a rather traditional technique, while working with alterable, can grow to be quite verbose and less readable 😬.

Let’s see how the above program can be written using list comprehension.

**Example 2: Get all sub-strings of the given string, using list comprehension + string slicing  **

```
word = "programming"
subs_list=[ word[i: j] 
	for i in range(len(word)) 
    	     for j in range(i + 1, len(word)+1) ] 
print(subs_list)
``` 
In the above example, a new list is assigned to variable subs_list, as indicated by the square brackets. the expression here is a string slicing method fitted with changing starting and ending indexes, to get all possibilities of the sub-strings from the word "programming".
Then We call the print() function to show the output.

True, you can certainly do all of these operations using traditional loops 🤷‍♂️. However, not every loop can be rewritten as a list comprehension. 
But list comprehensions, 
- 
Can replace verbose loops and unreadable code with maintainable logic.
- 
 With its elegant syntax, it can increase performance as opposed to normal functions that create lists.
- 
 It can also be used in combination with normal loops, but avoid making your list comprehension become too large.



I hope you write clean and maintainable python code with this technique at your disposal 😁.  
Thanks for the audience 🤗 and connect with me on [Github](https://github.com/nextwebb), [linkedIn](https://www.linkedin.com/in/peterson-oaikhenah-102645144/) and [Twitter](https://twitter.com/i_am_nextwebb).  







