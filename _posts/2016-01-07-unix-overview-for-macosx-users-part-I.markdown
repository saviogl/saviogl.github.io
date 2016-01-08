---
title:  "Unix overview for Mac OS X users - Part I"
date:   2016-01-07 21:00:00
description: A Unix basic overview to allow Mac OS X users understand the powerfull operating system underneath Mac OS X
---

#Motivation

Working as a web developer I found myself typing a lot of Unix commands, either to install a new cli tool, or to scaffold a new project, or to override Unix file's permissions and owenership, etc... Anyway the list goes on and on when it comes to the use of Unix commands on a web developer's work day. 

At some point I realized that I was dropping a bunch of commands on the terminal, and for some of then I wasn't even understanding what I was doing, I just knew that I had to type it in order to make something work. That bothered me. Bothered me a LOT. Being an Engineer I don't few confortable not knowing things, specially things that are part of my daily routine, part of my job.

Decided to fill that knowledge gap and to get more familiar with the system that operates my computer I resorted to the Internet to achieve that goal, and this series of posts aim to share with you a little bit from what I've learned and hopefully help you better understand what you're doing.

#Darwin

Unix is a multitask, multi-user operating system derived from AT&T Unix, developed in the 70s at the Bell Labs. Mac OS X is built on top of the [BSD](https://en.wikipedia.org/wiki/Berkeley_Software_Distribution)(Berkeley Software Distribution), a Unix distribution with some added developments from [NeXTSTEP](https://en.wikipedia.org/wiki/NeXTSTEP), and Apple itself. That customised version of Unix is called [Darwin](https://en.wikipedia.org/wiki/Darwin_(operating_system)), Apple’s Operating System.

Unix based operation systems are made up of two parts (Kernel and Shell), besides the core programs:

### Kernel
Is the core of the operating system. Responsible for allocating time and memory to programs and handling the filestore and communications in response to system calls.

### Shell
Is the outer layer of the operating system and acts as an interface between the user and the kernel, sending all user's requests to it.

Mac OS X comes set up with the [Bash](https://en.wikipedia.org/wiki/Bash_(Unix_shell)) shell, but includes other choices.

##Terminal
On the Mac OS X the default application that allows you to interact with the operating system is called "**Terminal**". It does so by providing a text-based command line interface running in conjuction with the Bash shell.

![Terminal screenshot](/assets/images/terminal.png)

### Prompt
This is the beginning of the command line. It usually provides some contextual information like who you are, where you are and other useful info. It typically ends in a $ . After the prompt is where you will be typing commands.

### Cursor
Indicates where in the line you are typing in. Provided with some text, you can move around the cursor as desired.

## Running a command
![Basic command](/assets/images/basic-command.png)

A Unix command contains the following basic structure:

{% highlight bash %}
command [option(s)] [argument(s)]
{% endhighlight %}

* *command* must be comprised of lowercase characters and digits
* *options* usually have a single dash followed by a letter (-v) or double dash followed by a word (—version)
* multiple *options* are allowed and you can squeeze then together:
{% highlight bash %}
ls -a  -l -h = ls -lah
{% endhighlight %}
* sometimes *options* require its own arguments so those will come right next to its option and may confuse the basic/standard structure, therefore it's common to see options’s arguments being passed with no spaces in between:
{% highlight bash %}
banner -w 50 ‘Hello World’ = banner -w50 ‘Hello World’
{% endhighlight %}
* multiple *arguments* allowed
* you can use semicolon or && to execute multiple commands all at once in one line (Those are different commands, semicolon indicates the end of the command):
{% highlight bash %}
echo ‘Hello’; echo ‘World’
{% endhighlight %}

## Unix Manual Page - man pages
The Unix Manual Page is a powerful learning tool for any developer. It allows you to quickly access the documentation of any Unix command through a *terminal pager* program such as more or less.

{% highlight bash %}
man <command_name>
{% endhighlight %}

For instance, if you want to understand what the Unix command *apropos* does:

{% highlight bash %}
man apropos
{% endhighlight %}

You would get

![Apropos](/assets/images/man-apropos.png)

## File System Organization

The filesystem appears as a single rooted tree of directories. Instead of addressing separate volumes such as *disk partitions*, *removable media*, and *network shares* as separate trees (as done in MS-DOS and Windows: each "drive" has a drive letter that denotes the root of its file system tree), such volumes can be "mounted" on a directory, causing the volume's file system tree to appear as that directory in the larger tree. The root of the entire tree is denoted /.

A typical Unix directory structure is shown below:

| Directory        														| Description           
| ------------------------------------------- |:---------------------------------------------:
| /      																			| Root
| /bin      																	| Binaries, programs      
| /sbin  																			| System binaries, system programs
| /dev  																			| Devices: hard drives, keyboard, mouse, etc.
| /etc  																			| System configuration
| /home  																			| User home directories
| /lib  																			| Libraries of code
| /tmp  																			| Temporary files
| /var  																			| Various (mostly files the system uses)
| /usr;/usr/bin;/usr/etc;/usr/lib;/usr/local  | User programs, tools and libraries (not files)

# Until next post

On the next post we will explore more of Unix file system, understand its representation and manipulate several files. 