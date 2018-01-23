---
layout: post
title:      "Learning to Bash - An Introduction"
date:       2018-01-23 04:18:25 -0500
permalink:  learning_to_bash_-_the_basics_part_1
---


<figure>
<img id="terminal_image" src="https://i.imgur.com/bAv6Jsz.png?1" title="source: imgur.com" style="height:300px" />
<figcaption style="font-family: arial; font-size:14px; color:gray;">An edited image of the Terminal on my Mac OS X</figcaption>
</figure>

<br/>

<h2 style="font-style: bold; color:blue;">Introducing Bash:</h2>

*This post introduces the basics of what a Terminal is, how to open it on a Mac OS X, setting up your first session, and how to customize the color theme.*

If you use OS X (Mac) or Ubuntu (Linux), there is a good chance you have heard of the terminal, which is also referred to as the console, command line, or shell. These may be foreign terms to beginners and learning about them can be confusing, as they are sometimes used interchangeable even through they (technically) mean different things. Even the "Terminal" app on Mac OS X is really a terminal emulator. For our purposes, however, I will be referring to it as the terminal. Just know that the terminal refers to a wrapper program, which runs the shell. The shell provides a Unix-like command line interface to communicate with the operating system.

*Eh, let me try to simplify that explanation...*

Put simply, the terminal is an application or program that you type different keywords into, called commands, which tell the computer what to do. There a variety of different shells available for use, but the most commonly used shell is Bash, which is the default shell for Ubuntu (Linux) and OS X (Mac). When you open the terminal on either of these systems, know that you are using the Bash shell and the commands you type in are actually Bash commands. The command line of the Learn IDE used by Flatiron also uses the Bash shell. Before I go further, please note that, although I am focusing on the Bash shell, the command prompt on Windows is also a command-line interface used to speak with the operating system, but has a different set of commands than Bash. And, even when the commands are similarly named, they may not yield the same result.

Today, most operating systems provide a graphical user interface (GUI) consisting of icons and various accompanying applications (apps) to help more user-friendly (and pleasing to look at). As a result, Bash is less likely to be used by the average user. This does not, however, mean that knowing Bash is useless. Rather, the Bash Terminal—or any command line interface—is a powerful tool that can make life a lot easier for users, particularly for aspiring software or web developers. In some instances, it is almost necessary. Therefore, I feel that every developer, regardless of level of expertise, should know at least the basics of Bash and some Bash commands.

<br/>

<h2 style="font-style: bold; color:blue;">The Basics</h2>

**Opening the Shell or Terminal**

To open the Terminal in Mac OS X, press and hold the command / ⌘ key and the spacebar. This should bring up a search bar. Alternatively, you can click on the maginifying glass in the top right corner of the screen. Type "Terminal" into the search bar (without the quotation marks) and double click the terminal application that appears in the results.

<figure>
<img id="terminal_image" src="https://i.imgur.com/hXT2F69.png" title="source: by Adeel M." style="height:100px" />
<figcaption style="font-family: arial; font-size:14px; color:gray;">The Terminal icon</figcaption>
</figure>

Or, you can go to the utilities folder in applications and double click on the icon named "Terminal."
Shortform: Finder -> Applications -> Utilities -> Terminal

Unlike Mac OS X, Linux offers more choices when it comes to terminal emulators, but my instructions deal with the native Terminal app already present on the Ubuntu operating system. To open it, open the Dash (Linux's start menu) by clicking on the Ubuntu icon in the upper-left corner. Type in "Terminal" without the quotation marks and double click the Terminal app that appears in the results.


**Anatomy of the Terminal**

Once you have the terminal open, one of the first things you see should be a line of text with your computer's name and the username followed by a **$**. That is, it should look something like this (yours may be black text with a white background):

<figure>
<img id="terminal_image" src="https://i.imgur.com/DkaYwk6.png?1" title="source: by Adeel M." style="height:300px" />
<figcaption style="font-family: arial; font-size:14px; color:gray;">A blank Terminal screen on my OS X laptop. I edited it to serve as an example.</figcaption>
</figure>


Although it looks simplistic, there are different parts to the terminal. The following image helps summarize this:

<figure>
<img id="terminal_image" src="https://i.imgur.com/4lkeeZ8.png" title="source: by Adeel M." style="height:400px" />
<figcaption style="font-family: arial; font-size:14px; color:gray;">The Anatomy of a Terminal</figcaption>
</figure>


Most of the labeled sumarries may not make sense or seem relevant until I go over how to move around the file system. For now, know that the area after the **$** is the  command line. This is where you will type in the commands to tell your computer what to do. Also note that the presence of the **$** indicates the command line is ready for you to type in. If the dollar sign is not present, entering commands will not have the desired effect.

The default color theme for the Terminal does not suit everyone. Fortunately, you can customize the colors to your liking. To do this on the Mac OS X, go to the top left of your screen, click on "Terminal" to view the drop down menu, and select "Preferences" from it. Alternatively, you can use the shortcut, command + , ( ⌘ , ) . In case you cannot tell, that's a command + comma (⌘ comma).

Once you have the preferences window open, click on the "Profile" tab at the top of the window.

You should see something like this:

<figure>
<img id="terminal_image" src="https://i.imgur.com/jINXKY0.png" title="source: by Adeel M." style="height:400px" />
<figcaption style="font-family: arial; font-size:14px; color:gray;">Terminal Preferences: Color Theme Selection</figcaption>
</figure>

You can now choose from a handful of pre-defined themes or create a new theme with custom colors by clicking the "+" symbol neat the bottom left. You may also change the font if you prefer. Once you have done this and are satisfied with the result, select your color theme's profile and click on the "Default" button near the bottom left of the window (near that "+" sign I mentioned earlier). Doing this will automatically load that theme each time you open a new session of the Terminal.

Well, that's it for this post. I hope it was helpful.
I will begin covering how to move around in the file system in my next post.

<h3 style="font-style:bold; color:rgb(0,153,153);">Next up:</h3>
<p style="font-style:italic; color:rgb(0,153,153);">Learning to Bash: The Basics of the Unix Filesystem</p>

