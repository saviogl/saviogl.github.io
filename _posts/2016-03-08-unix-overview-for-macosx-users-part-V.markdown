---
title:  "Unix overview for Mac OS X users - Part V"
date: 2016-03-08 22:16:32
description: A Unix basic overview to allow Mac OS X users understand the powerful operating system underneath Mac OS X - Input/Output Redirection and Custom Working Environment.
---

Today, we're going to explore a powerful computer programming concept which is heavily used and plays a big role in software development. Then, to wrap up this series of posts, we'll cover a little bit on how to customize your working environment and make things more personal.

# Standard Streams
> In computer programming, **standard streams** are preconnected input and output communication channels between a computer program and its environment when it begins execution. The three I/O connections are called ***standard input (stdin)***, ***standard output (stdout)*** and ***standard error (stderr)***.
> -- <cite>D. M. Ritchie, [A Stream Input-Output System](https://cseweb.ucsd.edu/classes/fa01/cse221/papers/ritchie-stream-io-belllabs84.pdf)</cite>


## Unix
On Unix, when executing a command, the **standard input** is connected to the terminal keyboard and the **standard output** and **error** are connected to the terminal screen (default behavior). However, Unix shell gives you the ability to change this preset behavior by a process called **Input/Output Redirection**. This is accomplished by making use of a few special characters, each defining a specific behavior.

### Output Redirection

You can easily change the command's **standard output (stdout)** from the default (terminal screen) to a **file** by appending a ***'>'*** followed by the name of the file '**filename**':

``` bash
$ who > users
```

Notice that no output appears on the terminal. This is because the output has been redirected from the default standard output device (the terminal) into the specified file. If you check ***users*** file you'll see all the outputted data provided by ***who***:

``` bash
$ who > users
$ cat users
saviolucena console  Mar  8 14:09
saviolucena ttys000  Mar  8 17:06
saviolucena ttys001  Mar  8 18:29
```

Be advised that the character **'>'** performs a destructive operation, hence the contents of the file are overwritten with the new data:

``` bash
$ who > users
$ cat users
saviolucena console  Mar  8 14:09
saviolucena ttys000  Mar  8 17:06
saviolucena ttys001  Mar  8 18:29
$ uptime > users
$ cat users
19:10  up  5:02, 3 users, load averages: 1.02 1.39 1.51
```

If you would like to append the new content to an existing file, switch operators, using '***\>\>***' instead of '***>***':

``` bash
$ who > users
$ cat users
saviolucena console  Mar  8 14:09
saviolucena ttys000  Mar  8 17:06
saviolucena ttys001  Mar  8 18:29
$ uptime >> users
$ cat users
saviolucena console  Mar  8 14:09
saviolucena ttys000  Mar  8 17:06
saviolucena ttys001  Mar  8 18:29
19:10  up  5:02, 3 users, load averages: 1.02 1.39 1.51
```

### Input Redirection
In the same way as you can redirect the output of a command to a file, you can redirect its input from a file. As the greater-than character ***'>'***  is used for output redirection, the less-than character ***'<'*** is used to redirect the input of a command.

To redirect **standard input** from a file instead of the keyboard:

``` bash
$ wc -l < users
	4
```

``` bash
$ sort -l < users
19:33  up  5:24, 3 users, load averages: 1.37 1.87 1.70
saviolucena console  Mar  8 14:09
saviolucena ttys000  Mar  8 17:06
saviolucena ttys001  Mar  8 18:29
```

We can see that we used the less-than character ***'<'*** to set the file as the **standard input (stdin)** for the command, and that the output was displayed on the terminal screen since we didn't make any changes (redirections) to it. We could have taken the results of the previous command and redirected its output to a specific file, as a matter of fact, let's do so.

``` bash
$ sort -l < users > sorted_users
$ cat sorted_users
19:33  up  5:24, 3 users, load averages: 1.37 1.87 1.70
saviolucena console  Mar  8 14:09
saviolucena ttys000  Mar  8 17:06
saviolucena ttys001  Mar  8 18:29
```

As you can see, a command can have both its input and output redirected.

### Discarding Output
Sometimes you might not be interested in the contents outputted from a command, and don't want to have it displayed on the terminal screen. In such cases, you can discard the output by redirecting it to the file **/dev/null**.

``` bash
$ cat sorted_users > /dev/null
```

### Pipes
Redirecting Input/Output streams is extremely powerful and this strength can be seen when used to connect multiple commands together taking the output of a command and putting it as input to another command. This is known as **piping** and is done via the **pipe (|)** character.

``` bash
$ echo “Hello World” | wc -w
	2
```

The command above takes the output of the echo command ("Hello World") and feeds it as input to the ***wc*** utility. You can pipe as many commands as you want:

``` bash
# Pass in "Hello World" as input to wc, then redirect the number of words (wc's output) to tr, which will remove all empty spaces and send the content to its default stdout (terminal screen)
$ echo “Hello World” | wc -w | tr -d ' '
2
```

# Customizing Your Working Environment
To wrap up this series of posts on Unix system, I'll talk about a little bit on how to get a few things customized to your needs in order to provide a better suited environment for development.

## Bash Login Shell
When you double-click the Mac OS X terminal application, you are telling the operating system to start a new login shell session. During that process, login shells typically read a file (or files) that does things to the current interactive shell like setting environment variables, creating alias, extending the PATH variable, etc. For a traditional bash shell the system files that are read and executed are (in order):

1.	/etc/profile *System file*
2.	~/.bash_profile, ~/.bash_login, ~/.profile, ~/.login *User-specific file*

> This second line will have only one of the files executed, the order of execution is left to right and the process is finished when a file is found

As I briefly mentioned before, this gives you a chance to customize a few things before getting the shell prompt ready to work.

> Upon logout, when a login shell exists, the bash shell will read and execute commands from ~/.bash_logout, if it exists.

## Bash Non-Login Shell
When you start a bash shell in a terminal in an existing session (a shell inside another for instance), you get an interactive non-login shell. For those cases bash shell will read and execute a configuration file named ~/.bashrc.

So, typically, your ~/.bash_profile contains the line

``` bash
if [ -f ~/.bashrc ]; then . ~/.bashrc; fi
```

after (or before) any login-specific initializations.

## Aliases
**Aliases** are nothing more than shortcuts for Unix commands. If you find yourself typing multiple times the same command which might contain an extensive list of options and arguments, you can bundle this command into a short **alias** in order to speed up your development process, and simplify your life.

You can easily set up an alias by using the following pattern:

```bash
$ alias [command]=“[command]”
```

For instance:

```bash
$ alias hello=“echo ‘Hello World’"
$ hello
Hello World
```

One common use for alias, which is heavily spread within the developers community, is to shortcut git commands. The following list represents some of the aliases set by [oh-my-zsh git plugin](https://github.com/robbyrussell/oh-my-zsh/blob/master/plugins/git/git.plugin.zsh) to facilitate daily work:

``` bash
$ alias g='git'
$ alias ga='git add'
$ alias gaa='git add --all'
$ alias gapa='git add --patch'
$ alias gb='git branch'
$ alias gba='git branch -a'
$ alias gbda='git branch --merged | command grep -vE "^(\*|\s*master\s*$)" | command xargs -n 1 git branch -d'
```

## Setting and Exporting Environment Variables
Unix allows you to set variables to be used in several ways by simply assigning a **value** to a **key**:

``` bash
$ MYNAME=John
$ echo $MYNAME
John
```

If the value contains white-space, quotations are used:

``` bash
$ MYNAME="John Doe"
$ echo $MYNAME
John Doe
```

As you can see, to reference the value of a variable you have to precede it with a **$** sign. The shell takes this to mean that it should substitute the value of the variable when it comes across this.

Variables that we set during the current bash session will disappear once we log out of that session, so in order to make them available in every bash session we could add them to the **~/.bash_profile** or **~/.bashrc** files.

Although the variables set in profile scripts will be shared amongst other bash sessions, they won't be available to child processes that bash starts for us. It will only be available in the current bash session. To indicate to bash these are environmental variables and should also be passed along to other commands, programs, and scripts (basically every time a new shell spawns) we need to use the export command.

``` bash
$ export MYNAME
```

All the environment variables can be seen with the ***printenv*** command. Let's use it to make sure that our variable has been properly exported:

``` bash
$ printenv
...
MYNAME=John Doe
...
```
Among a bunch of other variables we can verify that our variable is now an environment variable, and every time a new shell spawns, it will be available for them. If we want to completely unset a variable, either shell or environmental, we can do so with the ***unset*** command:

``` bash
$ unset MYNAME
```

> There is a good explanation on it on [this link](https://www.digitalocean.com/community/tutorials/how-to-read-and-set-environmental-and-shell-variables-on-a-linux-vps).

## Command Prompt
One common thing that is done by several developers, is the customization of the command prompt, the text that is printed every line to prompt you to type something. Bash configures its prompt by using the **PS1** and **PS2** variables.

- **PS1** defines the primary prompt which you will usually see.
- **PS2** prompt is used for multi-line commands.

	So, to change the primary command prompt, for instance, you simply change the contents of **PS1** variables.

``` bash
$ PS1='command prompt $'
command prompt $ # This has now become you command prompt
```

Bash provides a set of escape sequences which allow you to easily embed system information into the command prompt. Check the following list for a short reference:

|-----------------+---------------------------------------------------|
| Escape Sequence | Description 																			|
|-----------------|:--------------------------------------------------|
|		\u						| username																					|
|		\s						|	current shell 																		|
|		\w						|	current working directory 												|
|		\W						|	basename of current working directory 						|
|		\d						|	date in “weekday month date“ format (“Mon Jan 16”)|
|		\D{format}		|	date in strftime format (“%Y-%m-%d”) 							|
|		\A						|	time in 24-hour HH:MM format 											|
|		\t						|	time in 24-hour HH:MM:SS format 									|
|		\@						|	time in 12-hour  HH:MM am/pm format 							|
|		\T						|	time in 12-hour  HH:MM:SS am/pm format 						|
|		\H						|	hostname 																					|
|		\h						|	hostname up to first “.” 													|
|		\!						| 	history number of this command 									|
|		\$						|	when UID is 0 (root), a “#”, otherwise a “$” 			|
|		\\						| 	a literal backslash															|
|-----------------+---------------------------------------------------|

So, we could set PS1 to something like this:

``` bash
$ PS1="\u@\h:\w\$: "
saviolucena@Savios-MacBook-Pro:~/Documents$:
```

And thus, we get a nice and customized command prompt.

# Until the next post
Throughout this series of posts, we've explored a lot of Unix concepts and features to get you started on this powerful operating system. However, we're just scratching the surface and there is much more to everything that I've said here. The idea was not to turn you into a Unix expert, but to introduce you to a new world (or make you more familiar to it), which I consider extremely important, and which many times is taken for granted by many developers. 