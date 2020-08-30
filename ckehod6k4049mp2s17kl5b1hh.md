## How to find the longest substring without duplicates in characters - Python solution


![sebastian-herrmann-oMpknr7yi7g-unsplash.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1598825976956/UzRwboHQd.jpeg)
*<span>Photo by <a href="https://unsplash.com/@officestock?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Sebastian Herrmann</a> on <a href="https://unsplash.com/s/photos/problems?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Unsplash</a></span>*

Coding challenges can be head-scratching most of the time 😏. Well, for the most part, they require thorough understanding of the problem.  This might involve formulating a use case and manually working through the problem with some sets of sample data.

** The problem above says we should find the length of the longest sub-string without repeating characters or duplicates 🤔 .** 

So, if we have "pwwkew", the non-repeating sub-strings include "pw", "wke" and "w", but the longest sub-string without duplicating characters is "wke". This is because the  moment we move to the next character after "e", we hit another "w" and remember, we already have hit a previous w. 

We thus need to keep track of the length of a sub-string, when it hits a duplicate and continually update the starting point of other sub-strings.

## Psuedo Code
- Define a variable that tracks the start index of the current character in our input string
- Define a variable that holds the length of a current sub-string 
- Define a variable that holds longest length of a sub-string.
- Traverse the string and for each already visited character, store its last occurrence in a  hash table with the key as character and value as its last position.
- While traversing the string, check whether the current character is present in hash table or not.
-  If the current index is in the hash table  change the start index to the index after the current iteration. 
- If it is not present, then store current character in hash table with value as current index
- Increment the current length by one because we updated the hash table
- Check if the current length is greater than the longest sub-string length
- And return the longest sub-string length.

Let's implement this in code 🤓:

```
class Solution(object):
   def lengthOfLongestSubstring(self, string: str) -> int:
        sub  = {} #define a hashmap track every char and its current index
        cur_sub_start = 0 #this variable keeps track of the current starting index of a sub-string
        cur_len = 0 #when the hashmap is updated or the current sub-string meets a duplicate, the current length of the substring is updated
        longest = 0 #this variable records the max length of the cur_len of a sub-string

         #this loop traverses the input string , checking if a char exists in the hash table
        for i, letter in enumerate(string):
          if letter in sub and sub[letter] >= cur_sub_start:   
            #this condition checks if the char is in the hash table
             #and if the char exists in the current substring
             
              cur_sub_start = sub[letter] + 1  
               # increment the current substring starting index, because it's met a duplicate
               
              cur_len = i - sub[letter] #the current index - the index in the hash table, 
                #gives updates the current length of the substring
              sub[letter] = i
              # update the hash table with the current index of the duplicate char

          else:
            sub[letter] = i
              # if the char is not in the hash table add it.

            cur_len += 1 #increment the current sub-string length since it was updated 

            if cur_len > longest:
             # if the current sub-string length is greater than the previous length, update it

              longest = cur_len
          
        return(longest) #returns the longest string
``` 
## Conclusion
With our solution we eliminate the need to write nested loops, or to traverse the starting and ending sub-string indexes. We don't also need to first get all possible sub-strings 😑, before implementing our criteria in code. 

*According to leetcode*:

*[Longest Substring Without Repeating Characters.](https://leetcode.com/problems/longest-substring-without-repeating-characters/submissions/)*

> Runtime: 48 ms, faster than 96.97% of Python3 online submissions for Longest Substring Without Repeating Characters.
Memory Usage: 14.1 MB, less than 17.66% of Python3 online submissions for Longest Substring Without Repeating Characters.

Is this the most optimized solution? Certainly not, there are other approaches to this problem . If you have a better solution or you'll like to improve my code, it's [here](https://gist.github.com/nextwebb/7afd02e33694119e68a1c54efa271a55) 😉 . Please feel free to leave a comment, like and share. 

Thanks for the audience 🤗.  connect with me on [Github](https://www.github.com/nextwebb), [linkedIn](https://www.linkedin.com/m/in/peterson-oaikhenah-102645144) && [Twitter](https://www.twitter.com/i_am_nextwebb).
















