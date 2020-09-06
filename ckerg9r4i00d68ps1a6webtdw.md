## Git Submodules: An Ideal Workflow

Now, I admit - I'm no git expert or even close to that 🤭. But I do appreciate workflows that enable teams achieve excellent results with less effort. 
The engineers are more engaged and spirited 😁, so they work better to build great products.

However, most of us have no problem using advanced git features like rebase, squash and more. Yet, we still cannot wrap our heads around "this submodules concept" 🤦‍♂️ .

# What is Git submodules ?  

![kaleidico-26MJGnCM0Wc-unsplash.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1599417689827/eXsURjM5y.jpeg)
Git submodules is a method to organize changing projects or repos elegantly with in a main project .

*According to the  [Pro Git book](https://git-scm.com/book/en/v2/Git-Tools-Submodules#:~:text=Submodules%20allow%20you%20to%20keep,and%20keep%20your%20commits%20separate.)*

> Submodules allow you to keep a Git repository as a subdirectory of another Git repository. This lets you clone another repository into your project and keep your commits separate.

In some cases the Git submodules workflow could be the best solution  to create and manage  internal packages or manage changing micro services. 

## How do we get started with git submodules?
In this use-case, Let’s start by adding an existing Git repository as a submodule, within the current project we're working on. 

```
 git submodule add https://gitlab.com/nextwebb/my-submodule1.git
``` 
We notice we have changes added to our setup, with a sub-repo directory and a .git module file, which essentially is a configuration file that stores the mapping between the project’s URL and the local subdirectory you’ve pulled it into:

```
[submodule "my-submodule1"]
	path = my-submodule1
	url = https://gitlab.com/nextwebb/my-submodule1
``` 
Then, when we need to work within the submodule and make changes, its as simple as :

- creating a new branch

```
git branch <feature-branch>
``` 

- adding all local changes to the staging area
```
git add .
``` 
- committing changes 
```
git commit -m <"commit message">
``` 
- set and push  changes to the upstream branch. 
```
git push -u <remote> <branch>
``` 
Alternatively, you can use the “–set-upstream” option that is equivalent to the “-u” option.

```
git push --set-upstream <remote> <branch>
``` 
### When we clone a project with submodules, do we have to separately pull these sub-repos from the main project, or does the initial cloning operation take care of that recursively for us 🤔?

Generally, by default when we clone the main project,  the directories which contain submodules, contain no files within them yet:

In any case, we need to install the submodules using these commands:
``` 
git submodule init
git submodule update
```
A simpler method would be to use a recursive clone,  by  passing "--recurse-submodules" to the git clone command, it'll automatically initialize and update each submodule in the main repository, including nested submodules, if they exist. 

```
git clone  --recurse-submodules  <main repo URL>
```
We also have cases where the default branch within a submodule, might be behind the current upstream branch by a number of changes. Git provides a recursive foolproof command to initialize and update a projects submodules :

```
 git submodule update --init --recursive
```
### Publishing Submodule Changes

Now we've made some changes to our submodule directory. Some of these were brought in from the upstream repository and others local changes. 

 If we commit in the main project and push it up without pushing the submodule changes up as well, those changes will only exist locally
😑.

Git provides the "git push" command, which takes the "--recurse-submodules" argument which can be set to either “check” or “on-demand”.

```
git push --recurse-submodules=check

git push --recurse-submodules=on-demand
 ```
The “check” option will simply fail if any of the committed submodule changes, haven’t been pushed to the upstream repository. It throws an error and suggest we user the other option of “on-demand” , which will try to do this for us.

So it's a wrap for now 😀 .

Hopefully, this introduction on Git submodules shows how ideal a development workflow it  is,  inspiring you and your team to identify and streamline your workflow, so you can ship quality software.

Thanks for the audience 🤗.  connect with me on [Github](https://www.github.com/nextwebb), [linkedIn](https://www.linkedin.com/m/in/peterson-oaikhenah-102645144) && [Twitter](https://www.twitter.com/i_am_nextwebb).

### More Resources:
- [Pro Git book](https://git-scm.com/book/en/v2/Git-Tools-Submodules)






