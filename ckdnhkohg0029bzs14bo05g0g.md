# Javacript techniques: Fallbacks when destructuring nullable objects




JavaScript is a loosely typed language and thus your programs are prone to runtime errors. A runtime error is an error that occurs during the execution of the program, also known as the exceptions. In the example that is given below the program is syntactically correct , but at runtime it is trying to destructure a null object and throws an Error!  ☹.

```
    let {name, age, favColour} = player1;
    // where player1 could be null 
``` 
If we must  do conditional destructuring and  assignment with null-checks, then we might need to write-up boilerplate code to reduce the frequency of this kind of errors.

Let's see an example:

*This code snippet checks if a variable is defined before assigning it to a class property . it thus prevent errors*
```
if (file == undefined) {file = false} //prevents errors
  if (file) {
    this.file = file
  }
``` 
## Back story
So this happened to me a while back. It took me a while to debug my code and find the culprit 😈 . Then I figured I returned a promise that made a function call and the data that was being passed was undefined 
🤦‍♂️. So whenever you write a function that  performs destructuring assignment. RULE OF THUMB;  I mean always make sure you set a fallback and error checkers.

### What is the most elegant way of using it in cases like that or should we use destructuring assignments only for guaranteed non-null objects?

You can use an empty object as fallback, and if object is null or undefined the assigned variables will be undefined.

*Sample code snippet employing  "empty object" as a fallback strategy*
```
function printAge(player1 = {}) {
  const { age } = player1;
}

printAge(); // ✅ , no errors 👌
``` 

*Another implementation of "empty object" as a fallback strategy*

```
const player1 = null;
const { age, name, favColor } = player1 || {};

console.log(age, name, favColor);
``` 

Thanks guys for the audience 🤓! Connect with me on [Twitter](https://twitter.com/i_am_nextwebb), [linkedIn](https://www.linkedin.com/in/peterson-oaikhenah-102645144/) and [Github](https://github.com/nextwebb)
And do drop your comments and reviews.




  