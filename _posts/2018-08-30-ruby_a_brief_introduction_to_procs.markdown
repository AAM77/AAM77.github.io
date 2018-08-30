---
layout: post
title:      "Ruby: A Brief Introduction to Procs"
date:       2018-08-30 20:58:07 +0000
permalink:  ruby_a_brief_introduction_to_procs
---



## **{ Summary }**

In a previous post, I provided an introduction to <a href="https://adeelstechbeat.net/lets_talk_about_ruby_what_the_heck_are_blocks" target="_blank">blocks</a>. I also briefly introduced *yield* there. This post offers a relatively more in-depth introduction to Ruby's <a href="https://adeelstechbeat.net/ruby_an_introduction_to_yield_and_how_to_use_it" target="_blank">*yield* keyword</a>, what it is, and how to use it.
<br><br>

In this post, I provide a relatively brief introduction to procs in Ruby.
<br><br>
<br><br>


## **{ Main Content }**
 
### **Covering the Basics of *Procs* **

Okay, so we know that everything in Ruby is an object, right? Well, a *proc* is an object as well. When we want to create a *proc*, we have to type `Proc.new`. Besides being an object, *procs* are blocks of code that get assigned to a variable.

**For example:**

```
x = 2
#=> 'x' is a local variable assigned the value of two



some_flower_colors = Proc.new { puts 'Blue, Green, Orange, Pink, Red, White, and Yellow' }

#=> 'some_flower_colors' is a local variable that gets assigned a proc
#=> The proc is gets assigned accepts a block of code that outputs some colors.
```
<br>
Anytime we want to create a *proc*, we need to write out `Proc.new` and assign it a new block using either `{  }` curly braces or `do ... end`.
<br><br>

*But, wait... how do we **call a proc**?*
<br><br>

Whenever we want to call a proc, we use the #call method (append `.call` to the end of the variable's name) or adding `[]` brackets to the end of the variable's name.
<br><br><br>
**For example, if we want to call the proc we created a little while ago we type:**

```
some_flower_colors.call

some_flower_colors[]
```
<br>
**This outputs:**
``` 
blue, green, orange, pink, red, white, and yellow.

blue, green, orange, pink, red, white, and yellow.
```

Both the `.call` and `[]` tell Ruby to read and run the contents of the proc that the variable represents. Without them, Ruby thinks we want to know the literal value of the object and returns us the **id** of the *proc* instead of running the code inside of it.

So, if we had tried to call the previous proc by typing ```some_flower_colors``` without the `.call` or `[]`, we would have ended up with something like: ``` #<Proc:0x00007fecb60e82d8@(irb):1>```
<br><br><br>
**Here is one last example before we move on:**

```
some_flower_colors
some_flower_colors
some_flower_colors

some_flower_colors.call
some_flower_colors.call
some_flower_colors.call

some_flower_colors[]
some_flower_colors[]
some_flower_colors[]
```
<br>
**This outputs:**

```
#<Proc:0x00007fecb60e82d8@(irb):1>
#<Proc:0x00007fecb60e82d8@(irb):1>
#<Proc:0x00007fecb60e82d8@(irb):1>

blue, green, orange, pink, red, white, and yellow.
blue, green, orange, pink, red, white, and yellow.
blue, green, orange, pink, red, white, and yellow.

blue, green, orange, pink, red, white, and yellow.
blue, green, orange, pink, red, white, and yellow.
blue, green, orange, pink, red, white, and yellow.
```


**The bottom line here is:** If you want your proc to run the block of code inside of it, you call the proc by using the #call method (appending `.call` to the end of the variable name) or the `[ ]` brackets. I suggest using `.call` for better readability.
<br>

### **Using *procs* with Methods**

You should practice creating and calling a block in something like <a href="https://www.digitalocean.com/community/tutorials/how-to-use-irb-to-explore-ruby" target="_blank">IRB</a> before moving on to this section.

So, I think of a *proc* as being something like a customized *yield*, in the sense that you can pass only one *yield* to a method, but you can pass multiple (different) *procs* to a method. Using *procs* lets us insert different blocks of code in multiple places inside of a method.

Keep in mind, though, that a *proc* and *yield* are **different**. Benchmarking by various Ruby users reveals that *yield* is more efficient than *procs*. They found that *procs* are approximately two times slower than *yield*. It would be interesting to find out how much of a difference this makes in a larger application, but that is outside the scope of this post. (Source: <a href="https://stackoverflow.com/questions/1410160/ruby-proccall-vs-yield" target="_blank">Stack Overflow: Ruby proc call vs yield</a>).

So, basically, you can use procs with methods. I wrote this section out, but it needs some editing and I do not want to post it without cleaning it up a bit. I also want to post one blog post per week, so I will post this with some resources you can look at for a more in depth look at how to use *procs*. I will come back and edit it myself later.

Resources:

1. <a href="https://blog.udemy.com/ruby-proc/" target="_blank">Udemy:  How to Use the Ruby proc</a>
2. <a href="https://pine.fm/LearnToProgram/chap_10.html" target="_blank">Learn to Program:  Blocks and Procs</a>


