---
title:  "Unix overview for Mac OS X users - Part III"
date:   2016-02-06 22:43:00
description: A Unix basic overview to allow Mac OS X users understand the powerful operating system underneath Mac OS X - Ownership, Permissions and the ROOT user.
---

So, today I'm going to cover some important topics about Unix. I'll talk about a few things that you are probably dealing with on a daily basis, without fully understanding its concept or how it works, but you just suck it up "follow" the instructions to get by and achieve your end goal. I'll talk about Ownership, Permissions and the **root** user.

Before we dive into those topics I think it's important to cover some fundamental concepts.

#Users
>*Users are an abstraction that denotes a logical entity for assignment of ownership and operation privileges over the system. A user may correspond to a real-world person, but also a type of system operation. So, in my system, I have user 'nick' that corresponds to me, but I also have user 'www' which corresponds to the privileges necessary to operate the local webserver. Unix doesn't care about what the user means for me. It just knows what belongs to any given user and what each user is allowed to do with any given thing (file, program, device, etc) on the system. Unix identifies each user by a User ID (UID) and the username (or login) such as 'nick' and 'www' is just an alias to the UID that makes humans more comfortable.*

In a basic Unix environment all the users created are given a "home" directory under **/home/** as in /home/[username], i.e, a file system directory containing files for a given user. In Darwin (Mac OSx version of Unix), instead of **/home/**, a user's file system directory is created under **/Users/**.

![Darwin home directory](/assets/images/user_home_directory.png)

#Groups
>*Users can be organized in groups. A user may belong to one or more groups of users. The concept of groups serves the purpose of assigning sets of privileges for a given resource and sharing them among many users that need to have them (perhaps because they are all members of a project working team and they all need access to some common project files). So, on my system user 'nick' and user 'www' both belong to the group 'perlfect'. This way, they can have some shared privileges over the files for this site. User 'nick' needs them to edit the site, and user 'www' needs them to manage the webserver that will be publishing the site.*

The utility ***groups*** will list all the groups to which you (or the optionally specified user) belong.

{% highlight bash %}
$ ## groups [user]
$ groups
$ staff com.apple.sharepoint.group.1 wheel everyone localaccounts _appserverusr admin _appserveradm _lpadmin com.apple.access_screensharing-disabled _appstore _lpoperator _developer com.apple.access_ftp com.apple.access_ssh
{% endhighlight %}

#Ownership
Ok, now that we've gone through those concepts we are ready to kick things off. So, every file/directory on Unix has an **owner user** and an **owner group** mapped to it.

![Ownership listing command](/assets/images/user_home_directory_marked.png)

As you can see on the image above, executing the command ***ls*** with the ***-l*** option, you're given the listing of the current active directory with some extended details. For each file listed by ***ls*** you can see, on the third and fourth column, that there is a specific user and group attached to it, each representing an ownership relation. For any file or directory in the system, a given user may have one of the following ownership relations:

- 'user' **owns the file**,
- 'user' is **a member of the group that owns the file**, and
- 'user' is neither the user, nor belongs to the group that owns a file;

<br/>

##Changing Ownership
In order to change ownership of a given file or directory you can rely on ***chown***, to change both user and group ownership relation.
{% highlight bash %}
$ chown [options] [user]:[group] filename
{% endhighlight %}

Given the following directory listing, let's change the owner user from ***saviolucena*** to ***newuser***:
{% highlight bash %}
$ chown newuser example
$ l
total 0
drwxr-xr-x   3 saviolucena  staff   102B Feb  6 17:21 .
drwxr-xr-x  24 saviolucena  staff   816B Feb  6 17:21 ..
-rw-r--r--   1 newuser      staff     0B Feb  6 17:21 example
{% endhighlight %}

Now let's add ***everyone*** as the owner group for this file:
{% highlight bash %}
$ chown :everyone example
$ l
total 0
drwxr-xr-x   3 saviolucena  staff    102B Feb  6 17:21 .
drwxr-xr-x  24 saviolucena  staff    816B Feb  6 17:21 ..
-rw-r--r--   1 newuser      everyone   0B Feb  6 17:21 example
{% endhighlight %}

***chown*** allow you to do both operations at one, by passing the desired ***user*** and ***group*** joined by a colon:
{% highlight bash %}
$ chown newuser:everyone example
{% endhighlight %}

For directories this command will only change the directory ownership, to affect all the file hierarchy rooted in the directory you need to execute with the option ***-R(-r)***
> There's another utility to change groups ownership called ***chgrp***, but as ***chown*** takes care of both cases it's the preferred option.

#Permissions
Every file on the system has associated with it a set of permissions. Permissions tell Unix what can be done with that file and by whom. There are three things you can (or can't) do with a given file:

- **read** it,
- **write** (modify) it, and
- **execute** it. (For directories think of this as a traverse permission, allowing you to search inside the directory and change into it)

<br/>

Unix permissions specify which of the above operations can be performed for any ownership relation with respect to the file. In simpler terms, what the owner can do, what the owner group can do, and what everybody else can do with the file. For each ownership relation we'll need a set of three bits to specify access permission: read(**r**), write(**w**), and execute(**x**). To map all 3 possible relations we'll need a triplet for each, hence nine bits. To mark a set bit we use its corresponding letter (**r**, **w** or **x**), and to mark a clear bit we use a dash (-). Putting it all together in one row gives us the complete permissions of a file.

![Permission](/assets/images/permissions.png)
> The colors are mapping which permission set correspond to which ownership relation

So, in the above example, the owner user has the read and write permissions and the owner group, as well as other users, have only the read permission.

##Changing Permissions
To set/modify a file or directory permissions you need to use the **chmod** program. Of course, only the owner of a file may use **chmod** to alter a file's permissions.
{% highlight bash %}
$ chmod [options] mode filename
{% endhighlight %}

The remove part specifies how the permissions are going to be changed. It's composed of two parts, the first stating which user's permission should be changed and the second stating which access type should be changed. Let's say for example:
{% highlight bash %}
$ chmod ugo=rwx example
{% endhighlight %}

This means that the owner's permission **user(u)**, **group(g)** and **other(o)** should be given **read(r)**, **write(w)** and **execute(x)** permissions. This can also be set on a user basis like:
{% highlight bash %}
$ chmod u=rw, g=rw, o=r example
{% endhighlight %}

You can compress **u**, **g** and **o** into **a** (all users) to simplify the command call:
{% highlight bash %}
$ chmod a=rwx example # Is the same as ugo=rwx
{% endhighlight %}

Using **'='** you override all the access permissions with the new ones for the given relation (**u**, **g**, **o** or **a**). By switching to **+**, or **-** you can keep the permissions as they are and just **add(+)** or **remove(-)** the indicated permission.
{% highlight bash %}
$ chmod ug+w example # Add write permission to owner users and owner group
{% endhighlight %}
{% highlight bash %}
$ chmod o-w example # Remove write permission from others (everyone else)
{% endhighlight %}

###Octal Notation
You might have encountered things like ***chmod 755 somefile*** and of course you will be wondering what this is. The thing is, that you can change the entire permission pattern of a file in one go using one number like the one in this example by making use of the octal notation.
####Octal value mapping (r=4, w=2 and x=1)
|         				| user | group | other
| --------------- |:-----|:------|:------
| read    (r) 		| 4    | 4     | 4
| write   (w)  		| 2    | 2     | 0
| execute (x)			| 1    | 0     | 0
|         				| 7    | 6     | 4

So, the above mapping table give us the value of 764, thus executing the command will set the following permissions:
{% highlight bash %}
$ chmod 764 example
$ ls -l
$ -rwxrw-r--   1 saviolucena  everyone     0B Feb  6 17:21 example
{% endhighlight %}
- u: rwx => 4 + 2 + 1 = 7
- g: rw- => 4 + 2 + 0 = 6
- o: r-- => 4 + 0 + 0 = 4

That's probably the most preferred way for developers to set permissions on Unix.

#The *root* user
**root** is the user name or account that by default has access to all commands and files on a Unix-like operating system. It is also referred to as the ***root account***, ***root user*** and the ***superuser***. On a Mac OS X the root user is disabled by default. It's an important Unix concept and you probably have heard or will hear a lot of references to it. Remote Unix servers provided nowadays by several cloud infrastructure services usually have the ***root*** user enabled and your first access to them is through this user.
>Attention! It's not recommended to run your services under the ***root*** user, it's a better practice to run them on a user level and leave administrative tasks to the root user, and only on a per-need basis.

## SUDO
Sudo (substitute user and do) allows a system administrator to give certain users (or group of users) the ability to run some (or all) commands as root, or any other user while logging all commands and arguments. Sudo operates on a per-command basis, to make use of it just prefix a command with sudo. For instance:
{% highlight bash %}
$ sudo ls -la
{% endhighlight %}

You'll be asked to type in your password to verify your account:
{% highlight bash %}
$ sudo ls -la
Password: 
{% endhighlight %}

After you have verified your credentials the command is executed and you are granted a "*ticket*" for 5 minutes (default value but it's configurable) in which you can use the ***sudo*** prefix without having to reenter your password. Each subsequent ***sudo*** command updates the "*ticket*" for another 5 minutes. At any time you can expire this ticket by executing: 
{% highlight bash %}
$ sudo -k
{% endhighlight %}

This "*ticketing*" system prevents the problem of leaving a root shell where others can physically get to your keyboard. Some other features of ***sudo***:

- The ability to restrict what commands a user may run on a per-host basis.
- Sudo does copious logging of each command, providing a clear audit trail of who did what.
- Sudo's configuration file, the sudoers file, is setup in such a way that the same sudoers file may be used on many machines. This allows for central administration while keeping the flexibility to define a user's privileges on a per-host basis.

#Until the next post
Today, we've covered some fundamental concepts that orchestrate and support a Unix based system. I consider those concepts base and required knowledge for anyone who works with web development nowadays given the fact that Unix based systems are by far the most preferred servers to deploy modern applications.