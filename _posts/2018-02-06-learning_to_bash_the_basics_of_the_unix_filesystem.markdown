---
layout: post
title:      "Learning to Bash: The Basics of the Unix Filesystem"
date:       2018-02-06 13:18:21 -0500
permalink:  learning_to_bash_the_basics_of_the_unix_filesystem
---

Note:* I will  add proper images once I have more time. For now, I am using the blog's "code snippet" feature to represent the terminal.*

Last time, I introduced Bash, how to open the terminal, and the anatomy of the terminal. This time, I will discuss the file system of Unix-based operating systems. Understanding a little bit about the file system will help in navigating the directories using the terminal.

When you first open the terminal, you will most likely be in your home directory, which follows the generic path of */Users/Name*.

For example, if your name was Joefoo, the path would be:

```
Fufus-MBP:~ JoeFoo$ pwd

Users\Joefoo
```

But, don't take my word for it. Type *pwd* (present working directory) into the command line and hit enter to see for yourself. This will display the complete path to the folder you are in. The name after the last back slash ( \ ) is the folder you are currently in. You can then type *ls* (list) and press enter to list the contents of the folder. An example of this is:

<figure>
<img src="https://i.imgur.com/bAv6Jsz.png?1" title="source: Adeel M." style="height:300px" />
<figcaption style="font-family: arial; font-size:14px; color:gray;">An example displaying the results of the *ls* command</figcaption>
</figure>

If you want to change directories and move into a different folder, you need to use the *cd* (change directory) command. The one thing you need to keep in mind about using *cd* is that its use is dependent on the file tree. Before I go into how to change directories or create new folders, you should know the basic file structure in Unix based systems, since you must move along the path of the tree to go from one directory to another.

A typical file tree on a unix based system looks as follows:

<figure>
<img src="https://i.imgur.com/qnD2okO.png" title="source: imgur.com"  style="height:400px" />
<figcaption style="font-family: arial; font-size:14px; color:gray;">An Example of a File Tree on Unix-based Systems</figcaption>
</figure>

<br/>

For this explanation, unless stated otherwise, assume that I we are logged in as the user **Joe**.

Here, each folder contains the items attached to the main branch immediately below it. For example, the **/** (root) folder contains all of the items in the second row (i.e. **/bin**, **/dev**, **/etc**, **/usr**, **/tmp**, **/public**, and **/Users**), which can be considered its child directories. Another example would be the **/user** folder, which contains the directories **/Foofoo** and **/Joe**, which are also the usernames for the accounts on this particular Mac computer.

Just to avoid confusion, know that the number and names of the folders inside your root directory will be slightly different than here. If you are interested in creating your own filesystem tree, you can see which folders are in your second row (below the **/** root directory) by opening the terminal and typing *cd /*, hitting enter, and then listing the folders there, by typing *cd ls*:



To go from one directory to another, you must move along the path of the tree. If you are in the same directory as the folder you wish to move into, then it is a simple matter of moving into it by typing *cd* and the name of the folder you wish to move into. So, assuming we are logged in with **Joe** as our account or user name, if we are in the **/Users** directory and we want to move into **/Foofoo**, we would type *cd Foofoo*:

```
MacName:users Joe$ cd Foofoo
```

As soon as you hit the Enter key, the area immediately after "**MacName:**" should change from "**Users**" to "**Foofoo**":

```
MacName:Foofoo Joe$
```

Note how the "**~**" has been replaced by "**Foofoo**". But, what if we were in the **/Users** directory and we wanted to move into the **/Joe** directory while logged in with **Joe** as our account or user name? Well, we would simply type *cd Joe*:

```
MacName:users Joe$ cd Joe
```

which yields:

```
MacName:Joe Joe$
```

That's simple enough, isn't it? But, what if we wanted to move back into the **/Users** folder (the parent directory of **Foofoo** and **Joe**)? In Bash, the command to move into the parent directory of the current folder your are in is "*..*". So, to move back into the **/Users** directory, we would type *cd ..*:

```
MacName:Foofoo Joe$ cd ..
```

Which yields:

```
MacName:users Joe$ 
```

We can confirm we have successfully changed directories by typing *pwd* to display the path to our current directory. Go on. Try it out for yourself.

Now, what if we are in the **/Foofoo** directory and we want to move back into the **/lib** directory, which is in the **/usr** directory, which itself is in the **/** (root) directory?

Well, based on what we have discussed so far, which would need to move alone the tree by first moving back into the **/users directory** and then moving into the **/** (root) directory). After we are in the root directory, we would then need to move into **/usr** before finally moving into **/lib**.

*Ych! That sounds like hassle, doesn't it?*

Well, it doesn't have to be. Demonstrated as a flow diagram, this would be:

**/Foofoo** --> **/Users** --> **/** --> **/usr** --> **/lib**

Typed out as a command, this would be: *cd ../../usr/lib*:

```
MacName:Foofoo Joe$ cd ../../usr/lib
```

This yields:

```
MacName:lib Joe$
```

Recall that "*..*" allows us to move back into the parent directory. So, the first set of "*..*" moves us from **/Foofoo** to **/Users** while the second set of "*..*" moves us from **/Users** to **/** (root) and then we move follow a forward path by moving into **/usr** and then its child directory, **/lib**.

<br/>

**SHORTCUTS**

Now that you have a basic idea of the file tree system and how to move along it, you should be able to change directories without much difficulty. This can, however, become troublesome if you are currently in a directory that is deeply nested inside of a main folder.

For example, what if you were inside of a folder called "**/candyStash**" that had the following path:
*/users/Joe/Desktop/Fun_and_Games/Folder1/Folder2/Folder3/Folder4/Folder5/Folder6/Folder7/Folder8/Folder9/Folder10/Folder11/Folder12/Folder13/Folder14/Folder15/Folder16/Folder17/candyStash*

Trying to navigate from this folder into the previously discussed **/lib** directory or even back to the **/Desktop** directory would be a minor nightmare. Fortunately, there are a couple of shortcuts:

First, let us discuss the **/Users** directory again, which has the directories **/Foofoo** and **/Joe** in it. Well, these are 

Wait, why did **Users** change to "**~**" and not **Joe**? This is because "**~**" here means the home directory and, since we are logged in with **Joe** as our account or user name, we are in the home directory for **Joe**. If we were logged in as **Foofoo**, then typing *cd Joe* while in the **/Users** directory would have changed **Users** to **Joe** and not "**~**".

```
MacName:Joe Foofoo$
```

I am running out of time, so I will stop here. Stay tuned for further edits later today.

