## How to Automate tasks in Linux

## Introduction

Many of us love the  Linux kernel 💙. It uses the Unix based terminal. it's super customizable. We thus might choose to run Linux without a desktop environment like GNOME.  
Linux works magic ✨ on older hardware and it's lightweight. We don't sell or advertise on Linux or have to struggle with a ton of desktop bloatware and its a great platform for backend engineers.

Another productivity tip I leaned about, was setting up  Cron Jobs to schedule time-based operations, to be executed automatically on my computer. These jobs can be for a variety of tasks, like backups of data or, in my special case,  change the location of global tools like ```dotnet-ef ```  by adding 
 a line to my shell's configuration: 
```
export PATH=$PATH:/snap/bin
```

## Prerequisites
- Installing the cron daemon
- The Job to be done

Cron is a time-based job scheduling daemon found in Unix-like operating systems, including Linux distributions. Almost every Linux distribution has some form of Cron installed by default. However, if you have a Debian Linux distro without Cron preinstalled, you can install it using the APT package manager.

Before installing any package on a Debian machine, update the computer’s local package database:

```
 $ sudo apt update
```

Then install cron with the following command:

``` 
$ sudo systemctl enable cron 
```

``` 
Output

 Synchronizing state of cron.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable cron

 ```

Next, we write the script of the job we wish to automate in a bash file. below is the text of my script I use to change the location of ```dotnet-ef``` global tool in the shell configuration. 
```
#! /bin/bash
export PATH=$PATH:/snap/bin
```
This file can be created in the text editor of your choice and saved as ```filename.sh``` in the root of your home folder. (In my case this is /home/nextwebb/Desktop). This script can be directly run in the terminal with ```sh filename.sh ``` and it works correctly 😃. 



### Scheduling tasks using Crontabs

This exercise is done on the terminal using the ``` crontab ``` command with two flags,  ```-l (list)``` and ```-e (edit) ``` .
Type the command ```crontab -e``` and choose from a number [1-4] to select the text editor of your choice, with your personal Cron file and write your Cron job. I'll select 1 because my default editor is  Nano. 

```
crontab -e 
```

![Screenshot from 2020-10-11 12-40-22.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1602419083172/oOFvjKqU0.png)

The Terminal enters into Editor mode with your personal Crontab file opened.  As you can see, all the commands are shown at the bottom of the Terminal Window.  
``` 
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
 m h  dom mon dow   command

```

**Some Special Syntax**

These are essentially shortcuts for the equivalent numeric schedule specified:

```@hourly  -> 0 * * * *```

```@daily  ->	0 0 * * *```

```@weekly  ->	0 0 * * 0 ```

```@monthly  ->	0 0 1 * * ```

``` @yearly  ->	0 0 1 1 *```

``` @reboot``` ->  This shorthand will run whatever command follows it any time the server starts up 

Returning back to the subject in question, in my case, I want the bash file ``` filename.sh``` to be executed after a startup, within the crontab editor I type:

```
 @reboot  /home/nextwebb/Desktop/filename.script 
```

Once happy with your entry, you need to save it (Ctrl+s) and now press (Ctrl+x) and so exit back from the Editor to the Terminal.
Keying in will now show your new Cron Job and you can confirm if all looks okay, or not.
```
crontab -l
```


![Screenshot from 2020-10-11 13-21-32.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1602419070168/8KHzf6mzE.png)


It's a super-simple way to run scripts on boot using Cron jobs.

Thanks for the audience and I hope you found this article helpful 🤗. feel free to reach out to [Github](https://github.com/nextwebb), [Twitter](https://twitter.com/i_am_nextwebb) and [LinkedIn](https://www.linkedin.com/in/peterson-oaikhenah-102645144/).
Do drop a like, comment, and share 😌.

 For a detailed description of the Cron Job Syntax and Automated setups, have a look at   [Crontab Syntax Tutorial](https://community.linuxmint.com/tutorial/view/1072)

**Happy Scheduling 😉**.


