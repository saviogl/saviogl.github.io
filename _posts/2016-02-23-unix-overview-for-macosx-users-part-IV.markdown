---
title:  "Unix overview for Mac OS X users - Part IV"
date: 2016-02-23 15:16:00
description: A Unix basic overview to allow Mac OS X users understand the powerful operating system underneath Mac OS X - Commands and Programs.
---

We've seen that in order to interact with the kernel of the operating system and actually get things done we (the user) type a set of words in the shell which is in charge of interpreting these words and sending them along to the kernel acting as an interface between the two parties. These set of words are called **commands** and they follow a simple and straightforward pattern:

``` bash
command [option(s)] [argument(s)]
```

For instance:

``` bash
$ echo "Hello World"
Hello World
```

But, what is this command keyword? On Unix, to execute a file, one just needs to type the filename. For instance, the command *echo "Hello World"* can be executed as **/bin/echo "Hello World"** and that is because ***echo*** resides in that file path (/etc/echo):

``` bash
$ /bin/echo "Hello World"
Hello World
```

So to answer the previous question, **commands** are essentially a shorthand and a more convenient way for going to these files and executing them.

Some basic and useful information regarding commands:

- Common options: **-v**, **--version**, **-h**, **--help**

	> Usually all the major installable commands have these options built-in. -v and --version print to the standard output the current version of the command/program, whereas -h and --help print some usage information on how to proper use it, as well as some of its options

- Exit command: **q**, **x**, **ctrl + q**, or **ESC**

	> These are the most common keys implemented by commands that need to hold the shell prompt.

- Semicolons **(;)** or double Ampersand **(&&)** between commands

	> Either of these two characters typed between different commands allows one to concatenate commands execution one after the other, for instance, ***$ echo "Hello"; echo "World"*** prints "Hello" in one line and "World" in another before giving the shell prompt back to us. These characters are treated as command separators, just like pressing the ENTER key

- **whereis**, **which** and **whatis**

	> These are useful commands to get some basic information about other commands. **wheris** and **which** search and locate the executable file of a certain command and print out the file path to the standard output (There is a difference between these two, check the man pages for both of them). **whatis** searches a set of database files of system commands for keywords and displays the result on the standard output

## The PATH variable

So if ***commands*** are essentially a shorthand for tracking files and executing them what is the logic that the shell uses to locate them? It would take forever if it would search the entire file tree, so Unix system has an environment variable set up called **PATH** which is a comma separated list of files paths that it uses to locate commands to execute.

``` bash
$ echo $PATH
/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
```

The early entries take precedence over the later ones, so it starts from the left and works all the way to the to right until it finds what it's looking for.

In **BASH** to change the **PATH** variable for the current shell session just use:

``` bash
$ PATH = '/usr/custom/bin:/usr/local/bin:/usr/bin:/bin'
```

## System Information Commands

- **date** - Prints out the system date

``` bash
$ date
Tue Feb 23 13:13:04 BRT 2016
```

- **uptime** - Prints out how long the system has been running

``` bash
$ uptime
13:14  up  5:08, 3 users, load averages: 2.31 2.01 1.91
```

- **uname** - Prints out how the operating system name

``` bash
$ uname
Darwin
```

- **hostname** - Prints out the host system name

``` bash
$ hostname
Savio-Lucenas-MacBook-Pro.local
```

## Disk Information Commands

- **df** - Report file system disk space usage

``` bash
$ df -h
Filesystem                          Size   Used  Avail Capacity  iused   ifree %iused  Mounted on
/dev/disk1                         297Gi  282Gi   15Gi    96% 73964740 3877530   95%   /
devfs                              186Ki  186Ki    0Bi   100%      644       0  100%   /dev
map -hosts                           0Bi    0Bi    0Bi   100%        0       0  100%   /net
map auto_home                        0Bi    0Bi    0Bi   100%        0       0  100%   /home
localhost:/ATcy2p79wkpv6kfRwDmi4C  297Gi  297Gi    0Bi   100%        0       0  100%   /Volumes/MobileBackups
```

- **du** - Display disk usage statistics

> *The **du** utility displays the file system block usage for each file argument and for each directory in the file hierarchy rooted in each directory argument.  If no file is specified, the block usage of the hierarchy rooted in the current directory is displayed.* (Taken from the **man** page)

``` bash
$ du -hs Documents
1.4G	Documents
```

## View Process Information

- **ps** (process status) - Report a snapshot of the current processes

``` bash
$ ps aux
```

> **a**: list all foreground processes, owned by anyone; **u**: show a column with the process user starter; **x**: show background processes. Most common way to use it.

## Monitoring Process Information

- **top** - Display Linux tasks, live monitor processes

``` bash
$ top
```

Common Options:
**-n** = number of processes to show; e.g. -n 10
**-o** = sort order; e.g. -o cpu
**-s** = refresh time; e.g. -s 3
**-U** = user of the process; e.g. -U saviolucena

Putting it all together:

``` bash
$ top -n 10 -o cpu -s 3 -U saviolucena
```

## Stopping Processes

- **ctrl+c** - Only works if you have control of the process
- **kill** (pid) - Send a signal to a process by its PID

``` bash
$ ps c
ps c
  PID TTY           TIME CMD
  727 ttys000    0:02.15 -zsh
24754 ttys000    1:02.56 ruby
24761 ttys000    0:00.09 fsevent_watch
26055 ttys001    0:01.56 -zsh
$ kill 24754
```

The default behavior of the **kill** command is to send a SIGTERM signal to the running process. This gives the opportunity to the process to catch this event and perform its shutdown procedures to terminate itself cleanly by closing all log files, connections, etc. If this does not work, you could send a **kill** command with the **-9** option, which forces the process to shutdown without giving the opportunity to handle anything prior to it.

## Text File Helpers

- **wc** - Word count

> Outputs three columns and the file name. ***First column***: number of lines (\n) in the file. ***Second column***: number of words in the file. ***Third*** column: number of characters in the file.

``` bash
$ wc dump.rdb
3      11   11450 dump.rdb
```

- **sort** - Sort lines

> Capital letters are treated differently from lower case letters. So if you want it them to be sorted as the same values, pass in the **-f** option.

``` bash
$ sort fruit.txt
apple
banana
banana
peach
pear
pineapple
plum
```

- **uniq** - Filter in or out adjacent repeated lines

**-d** = return the duplicated lines of the file
**-u** = return the unique line occurrences of the file

``` bash
$ uniq fruit.txt
apple
banana
peach
pear
pineapple
plum
```

``` bash
$ uniq -d fruit.txt
banana
```

> If you want to remove duplicates that are not adjacent (sequential), you have to first sort the file.


## Command History

The Unix system has an interesting and useful history tool implemented to keep track of every command typed on a terminal. It does two things:

- It keeps a ~/.bash_history (if you are using BASH) log file containing all the commands typed but only updated when the bash shell is about to quit its session.

``` bash
$ tail -n 3 .bash_history
cd tmp
cd ~
ls -l
```

- Provides a **history** utility which prints out a live list of all the commands executed by the user

``` bash
$ history
...
10487  echo Kevin >> people.txt
10488  cat people.txt
10489  uniq people.txt
10490  uniq fruit.txt
10491  uniq -d fruit.txt
```

> The reference numbers shown on the left (**hid**), can be used to call that command instead of using the up arrow key until you find the command you are looking for.

### History Command

To work with the history command there are a few shortcuts, and all of them start with an exclamation point **(!)**.

|-----------------+---------------------------------------------------|
| Command 				| Description 																			|
|-----------------|:--------------------------------------------------|
| !3 							|References history command #3 (**hid**)  					|
| !-2     				|References 2 commands-back in the history          |
| !cat      			|References most recent command beginning with “cat”|
| !! 							|References previous command 												|
| !$ 							|References previous command arguments  						|
|-----------------+---------------------------------------------------|
