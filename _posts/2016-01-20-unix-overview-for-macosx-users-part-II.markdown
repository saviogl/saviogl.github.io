---
title:  "Unix overview for Mac OS X users - Part II"
date: 2016-01-20 15:46:00
description: A Unix basic overview to allow Mac OS X users understand the powerfull operating system underneath Mac OS X - Walking through Unix filesystem (cd, touch, cat, mkdir, mv, rm, cp, etc.)
---

Picking up from where we left off in the previous post, it's time to start manipulating files and directories through the unix filesystem.

#Changing Directories
The first thing that we have to learn before getting down to file and directory manipulation is how to walk through the Unix filesystem. To do so you make use of the ***cd*** (aka chdir - Change Directory) utility. Executing this command will change the current working directory to whatever path argument you pass to it.

{% highlight bash %}
$ cd test # This will walk you to the 'test' folder and make it the current directory
{% endhighlight %}

To walk out of a directory you execute the ***cd*** command with the '***..***'' argument like this:
{% highlight bash %}
$ cd .. # This will walk you out to the parent directory of the current tree
{% endhighlight %}

There are a couple of default arguments that are used as shortcuts to change directories. ***cd ~*** will take you to the user's directory, and ***cd -*** will toggle you back and forth to the last working directory.

#Creating Files
In a Unix based system you have three simple ways for creating files from the terminal's command line:

* Unix text editors

* Direct output to a file

* The ***touch*** command

##Unix Text Editors
A text editor basically lets you add, change, and rearrange text easily through the terminal's command line. There are some options included with the Mac OS X:

###vi (Visual Editing Mode), vim (Improved version of vi)
![Visual Editing Mode](/assets/images/vim.png)
It's been around for a long time and it's many people's favorite, but faces some user-friendliness problems. Although the enhanced version of it (vim) offers extensions to add extras functionalities; e.g., *syntax highlighting*, *new editing commands*, and even *mouse support*, to mitigate that problem.
###emacs (Editor Macros)
![Editor Macros](/assets/images/emacs.png)
Probably the most powerful text editor out there. It's referred to as the swiss army knife by many due to its extensive capabilities and the ability to create macros to automate recurrent tasks.
###pico (Pine Composer), nano (Improved version of pico)
![Nano](/assets/images/nano.png)
It has all the basic editing features, and it's probably the easiest to learn. nano, the enhanced version of pico, is my personal favorite to do simple and basic text editing due to its simplicity and limited commands.

These are just a few options that come out of the box with your operating system, if you would like to check them all you could go over to [this](https://en.wikipedia.org/wiki/List_of_text_editors) Wikipedia page.

##Direct output to a file (Output Redirection)
We're going to dig deeper into this topic in another post in the future, but for now you only need to know that you can also create a file by redirecting the output of a command, using the ***'>'*** operator.

Check following **who** command which redirects the complete output of the command to a file named users in the current directory.
{% highlight bash %}
$ who > users
$
{% endhighlight %}

If we list the files, we'll find a users file that holds all the output produced by the ***who*** command.
{% highlight bash %}
$ ls -l
total 16
drwxr-xr-x  8 owner  staff   272 Nov 20 00:05 images
-rw-r--r--  1 owner  staff  1102 Nov 24 19:02 index.html
-rw-r--r--  1 owner  staff   280 Jan 19 17:11 users

$ cat users
owner console  Jan  4 20:12
owner ttys000  Jan 13 15:56
owner ttys001  Jan  5 10:32
owner ttys002  Jan  7 11:03
owner ttys003  Jan 19 12:11
owner ttys004  Jan 19 16:57
owner ttys005  Jan 19 08:36
owner ttys007  Jan 19 16:04
$
{% endhighlight %}

Note that if a command has its output redirected to a file that already contains data, that data will be lost.

{% highlight bash %}
$ echo line 1 > users
$ cat users
line 1
$
{% endhighlight %}

You can use the ***'>>'*** operator to append the output in an existing file as follows:
{% highlight bash %}
$ echo line 2 >> users
$ cat users
line 1
line 2
$
{% endhighlight %}

##The ***touch*** command
The *touch* command is the easiest way to create new, empty files. It's also widely used as a way to update the ***timestamps*** (i.e., dates and times of the most recent access and modification) on existing files and directories.

To create a file simply execute the *touch* command followed by a file's name:
{% highlight bash %}
$ touch new_file
{% endhighlight %}
{% highlight bash %}
$ ls -l
-rw-r--r--  1 owner  staff     0 Jan 19 17:27 new_file
-rw-r--r--  1 owner  staff   280 Jan 19 17:11 users
$
{% endhighlight %}

If you run the same command again the ***timestamp*** of that file will be updated with the current date and time:
{% highlight bash %}
$ touch new_file
{% endhighlight %}
{% highlight bash %}
$ ls -l
-rw-r--r--  1 owner  staff     0 Jan 19 17:28 new_file
-rw-r--r--  1 owner  staff   280 Jan 19 17:11 users
$
{% endhighlight %}

#Reading Files
The most basic and simple way of reading files on Unix is through the use of the ***cat*** command. The ***cat*** command reads one or more files and prints them to standard output. Let's take the previous created **'users'** file and try to run the ***cat*** command with it.
{% highlight bash %}
$ cat users
userone console  Jan  4 20:12
userone ttys000  Jan 13 15:56
userone ttys001  Jan  5 10:32
userone ttys002  Jan  7 11:03
userone ttys003  Jan 19 12:11
userone ttys004  Jan 19 16:57
userone ttys005  Jan 19 08:36
userone ttys007  Jan 19 16:04
$
{% endhighlight %}
All the contents of the file have been printed out to the standard output.

Although ***cat*** comes in handy a lot of times it might not be the best way for reading long text files, since it outputs all the contents to the screen at once and you might lose track of what you're looking for. To better handle the examination of those files you can use either ***more*** or ***less*** command.

Both these commands perform similar functions as they allow you to see the text file one "page" at a time (Paginated output) i.e., "page" being the amount of content that fits the current terminal window screen. You use the **'spacebar'** to continue paging, **'enter'** key will move down one line, and **'q'** to quit. ***less*** was developed later and has more features than ***more***. The most useful feature is that it can scroll backwards **'(crtl + b)'** and can jump between the beginning **'(g)'** and the end **'(shift + g)'** of the document.

![Less](/assets/images/less.png)

In the Darwin Unix system the command ***more*** actually calls ***less*** behind the scenes since it has more features and better memory usage.

##Reading Portions of Files
Two useful commands to examine portions of files are ***head*** and ***tail*** where one reads the first few lines (***head***), and the other reads the final (***tail***) few lines of any text given to it as input and write them to the standard output.

{% highlight bash %}
#The head command by default prints the first 10 lines of the document
# but you can change this number with the -n option
#Prints the first two lines of the users file
$ head -n 2 users
userone console  Jan  4 20:12
userone ttys000  Jan 13 15:56
$
{% endhighlight %}

{% highlight bash %}
#The tail command by default prints the last 10 lines of the document
# but you can change this number with the -n option
#Prints the last two lines of the users file
$ tail -n 2 users
userone ttys005  Jan 19 08:36
userone ttys007  Jan 19 16:04
$
{% endhighlight %}

There is one useful option about the ***tail*** command that I consider worth noting here which is ***'-f'***. Passing this option to the ***tail*** command makes it hold the prompt in order to output newly created lines, thus becoming a live file inspector.

{% highlight bash %}
#Print last lines and keep watching for newly created ones
$ tail -f users
userone console  Jan  4 20:12
userone ttys000  Jan 13 15:56
userone ttys001  Jan  5 10:32
userone ttys002  Jan  7 11:03
userone ttys003  Jan 19 12:11
userone ttys004  Jan 19 16:57
userone ttys005  Jan 19 08:36
userone ttys007  Jan 19 16:04

#To redeem the prompt command just hit (crtl + c)
{% endhighlight %}

#Making Directories
To create new directories on Unix you make use of the ***mkdir*** utility.

{% highlight bash %}
$ mkdir new_folder
$ ls -l
total 16
drwxr-xr-x  8 userone  staff   272 Nov 20 00:05 images
-rw-r--r--  1 userone  staff  1102 Nov 24 19:02 index.html
-rw-r--r--  1 userone  staff     0 Jan 19 17:28 new_file
drwxr-xr-x  2 userone  staff    68 Jan 19 21:47 new_folder # New folder created
-rw-r--r--  1 userone  staff   280 Jan 19 17:11 users
$
{% endhighlight %}


***mkdir*** will prompt an error message if you try to create nested folders where any of the parent directories do not exist.

{% highlight bash %}
$ mkdir new_folder/nested1/nested2
mkdir: new_folder/nested1: No such file or directory
$
{% endhighlight %}

To enable creation of the necessary parent directories just include the ***-p*** option

{% highlight bash %}
$ mkdir -p new_folder/nested1/nested2
$
{% endhighlight %}

#Moving and Renaming Files and Directories
To accomplish both operations the same Unix command is used, ***mv***. Both operations follow the same pattern:

{% highlight bash %}
$ mv [SOURCE] [DEST]
{% endhighlight %}

***mv*** will decide to perform one operation or another based on the [DEST] argument. If [DEST] is an existing directory the utility will move all files/folders from [SOURCE] to [DEST], if not it'll simply rename the [SOURCE] operand to its new name.

{% highlight bash %}
#Renaming file
$ mv users new_users
$ ls -l
total 16
drwxr-xr-x  8 userone  staff   272 Nov 20 00:05 images
-rw-r--r--  1 userone  staff  1102 Nov 24 19:02 index.html
-rw-r--r--  1 userone  staff     0 Jan 19 17:28 new_file
drwxr-xr-x  3 userone  staff   102 Jan 19 21:55 new_folder
-rw-r--r--  1 userone  staff   280 Jan 19 17:11 new_users # users was renamed to new_users
{% endhighlight %}

{% highlight bash %}
#Moving file
$ mv new_users new_folder
$ ls -l new_folder
total 8
drwxr-xr-x  3 userone  staff  102 Jan 19 21:55 nested1
-rw-r--r--  1 userone  staff  280 Jan 19 17:11 new_users # new_users has been moved inside new_folder
{% endhighlight %}

> ***mv*** is set by default to overwrite (***-f***) files/folders, to prevent overwriting use the (***-n***) option, or for interactive overwriting (asks for permission) use the (***-i***) option.

#Copying Files and Directories
***cp*** command is the one to use to copy files or directories. For instance, let's say that you want to make a copy of a file called **new_file** and name it **cp_new_file**. You would run the command:

{% highlight bash %}
#Copying file
$ cp new_file cp_new_file
$ ls -l
total 24
-rw-r--r--   1 userone  staff     5B Jan 19 22:30 cp_new_file #new file, copied from new_file
drwxr-xr-x   8 userone  staff   272B Nov 20 00:05 images
-rw-r--r--   1 userone  staff   1.1K Nov 24 19:02 index.html
-rw-r--r--   1 userone  staff     5B Jan 19 22:25 new_file
drwxr-xr-x   4 userone  staff   136B Jan 19 22:14 new_folder
$
{% endhighlight %}

To copy entire directory structures from one place to another you have to make use of the ***-R(-r)*** option which indicates recursive operation.

{% highlight bash %}
#Copying directory
$ cp -R new_folder cp_new_folder
$ ls -l
total 24
-rw-r--r--  1 userone  staff     5 Jan 19 22:30 cp_new_file
drwxr-xr-x  4 userone  staff   136 Jan 19 22:38 cp_new_folder
drwxr-xr-x  8 userone  staff   272 Nov 20 00:05 images
-rw-r--r--  1 userone  staff  1102 Nov 24 19:02 index.html
-rw-r--r--  1 userone  staff     5 Jan 19 22:25 new_file
drwxr-xr-x  4 userone  staff   136 Jan 19 22:14 new_folder
$
{% endhighlight %}

> ***cp*** is set by default to overwrite (***-f***) files/folders, to prevent overwriting use the (***-n***) option, or for interactive overwriting (asks for permission) use the (***-i***) option.

#Removing Files and Directories
To remove unwanted files on a Unix system use the ***rm/unlink*** command. Let's say that we don't want that **new_file** copy created in the previous topic, we can remove the file by runnnig the following command:

{% highlight bash %}
#Removing file
$ rm cp_new_file
{% endhighlight %}

To remove a directory and all its files, run the same command using the ***-R(-r)*** option which indicates recursive operation.

{% highlight bash %}
#Removing non empty directory
$ rm -R cp_new_folder
{% endhighlight %}

There is another command that you could use to remove directories, ***rmdir***, but it will only go through with the operation if the target directory is empty.

#Hard Links
When performing a listing in a directory on Unix the listing is actually a list of references that map to an [inode](https://en.wikipedia.org/wiki/Inode) (i.e., a data structure that represents a filesystem object which contains attributes and disk block location(s) of the object's data). When a hard link is created it becomes yet another reference to that same inode as the original file, and you end up with two exact files without having to duplicate the data on disk.

Taking that into account a hard link does not break if the original file is moved nor even if it's deleted since it references the file in the actual hard drive. However, take note that if you are modifying the hard link you are in turn modifying the original file as well (and vice-versa) since both reference the same inode.

{% highlight bash %}
#Creating a hard link
$ ln new_file hard_link_to_new_file
$ ls -l
total 24
-rw-r--r--   2 userone  staff     5B Jan 19 22:25 hard_link_to_new_file # New hardlink which references new_files's content in disk
drwxr-xr-x   8 userone  staff   272B Nov 20 00:05 images
-rw-r--r--   1 userone  staff   1.1K Nov 24 19:02 index.html
-rw-r--r--   2 userone  staff     5B Jan 19 22:25 new_file
drwxr-xr-x   4 userone  staff   136B Jan 19 22:14 new_folder
{% endhighlight %}

>When a hard link is created it's listed as a regular file ***(-)***, not a reference to it.

#Symbolic Links (aka Symlinks)
A symbolic link is similar to a hard link in that it is used to link to an already existing file, however it differs greatly in its implementation. Whilst hard links reference to an inode, symbolic links just reference to a file or directory path acting as a pointer to the original file.

{% highlight bash %}
#Creating a symlink
$ ln -s new_file symlink_to_new_file
$ ls -l
total 32
-rw-r--r--  2 userone  staff     5 Jan 19 22:25 hard_link_to_new_file
drwxr-xr-x  8 userone  staff   272 Nov 20 00:05 images
-rw-r--r--  1 userone  staff  1102 Nov 24 19:02 index.html
-rw-r--r--  2 userone  staff     5 Jan 19 22:25 new_file
drwxr-xr-x  4 userone  staff   136 Jan 19 22:14 new_folder
lrwxr-xr-x  1 userone  staff     8 Jan 20 14:08 symlink_to_new_file -> new_file
{% endhighlight %}

Different from hard links, symbolic links break if the original file or path is moved or deleted.

>When a symbolic link is created it's listed as an l file ***(l)***, not a regular file ***(-)***.

#Searching for Files and Directories
When it comes to finding files or directories in the terminal's command line ***find*** is the one to rely on. It's a very powerful utility and can search the entire filesystem to find files and directories according to a specific search criteria.

{% highlight bash %}
#Basic find command pattern
$ find [path] [expression]
$ find . -name new_file # Find all files and directories named 'new_file' within the current directory's tree
./new_file # File found
$ find . -type f # Find all files (-type f) within the current directory's tree
./hard_link_to_new_file
./index.html
./new_file
./new_folder/new_users
$ find . -name "new*" # Find all files and directories within the current directory's tree that start with "new"
./new_file
./new_folder
./new_folder/new_users
{% endhighlight %}

The operator ***\**** works as a wildcard meaning zero or more characters. Besides the ***\**** there are two other wildcards:

- **?** | any one character
-	**[]** | any character in the brackets

<br/>
As I've said before, ***find*** is an extremely powerful tool for Unix users and you'll definitely want to check the ***man*** page for that utility to harness its full power. However there is one other useful feature of ***find*** that I think is worth mentioning here, the ***-exec*** operator. You see, besides listing the files that you are searching for, ***find*** allows you to execute commands (***grep***, ***rm***, ***mv***, etc.) on them without having to ***pipe**** its output.

{% highlight bash %}
#Find and -exec
$ find . -name new_file -type f -exec rm {} ';' # Find the file named 'new_file', and remove it from the filesystem
{% endhighlight %}

> ****pipe*** (|) is an operator that allows you to take the output of a command and insert it as the input of other command. We're going to cover it in a future post when we'll talk more about commands and programs, and directing input and output.

#Until the next post
In this post we explored several utilities for working on the Unix filesystem. Those utilities are pretty much used on a daily basis for most developers that own a Unix machine, or anyone that has to work their way around Unix based servers, therefore mastering them will help you grow professionally and will allow you to better understand your work environment.