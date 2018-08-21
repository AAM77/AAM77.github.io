---
layout: post
title:      "Ruby: An Introduction to 'yield' and How to Use it"
date:       2018-08-21 07:15:26 +0000
permalink:  ruby_an_introduction_to_yield_and_how_to_use_it
---



## **{ Summary }**

In a <a href="http://adeelstechbeat.net/lets_talk_about_ruby_what_the_heck_are_blocks">previous post</a>, I provided an introduction to blocks. I also briefly introduced *yield* there. This post offers a relatively more in-depth introduction to Ruby's *yield* keyword, what it is, and how to use it.
<br><br>
**[ When to Use Yield ]**
<br><br>
My experience so far has taught me that *yield* in Ruby is a good option when:
<br><br>
(1) trying to make my code DRY
<br>
(2) adding custom functionality to a part of my method without having to re-write it (which is similar to 1).
<br><br>
**[ How to Use Yield ]**
<br><br>
*yield* can be used even if the method has no arguments. You can insert yield almost anywhere into the method.<br>
This is how it would look when you insert it:
```
def method_name
 yield
end
```
<br>
After inserting a *yield* into the method, you will need to pass it a block, which would having the following format:<br>
```
method_name { puts 'Hello' }
```

The output for this would be `Hello`. 
<br>

*yield* is unnecessary in the very simple example above, but it still demonstrates the basics of using *yield* and passing it a block when calling the method. Also, *yield* is useful for accepting a custom block, but a method using *yield* in multiple spots can still accept only one block of code. Each *yield* will be substituted with the same block of code. If you need to pass two or more custom blocks to your method based on various conditions, you should look into ***lambdas*** and ***procs***.
<br><br>
I cover come of the nuances of using *yield* in the longer version of this post (below).



## **{ The Long Version }**
 
### **What is *yield* in Ruby?**

Before we start, make note that the *yield* keyword means something different in Ruby than it does in many other programming languages. In Ruby, *yield* is a special keyword that allows Ruby developers to pass in a block (chunk of code) to a method. The block gets inserted into the method and takes the place of the *yield* keyword.
<br><br>
For example, if our method looks like this:

```
def method_1
 yield
 puts "This is already inside the method."
end
```

If we decide to call this method and pass it a block:  **`method_1 { puts "I passed in this chunk of code." }`**, for the duration of this method call, the code of the method above  is temporarily "transformed" to look something like:

```
def method_1
 puts "I passed in this chunk of code."
 puts "This is already inside the method."
end
```

Once it finishes running, the method's code transforms back into:

```
def method_1
 yield
 puts "This is already inside the method."
end
```

Passing a block to a method with the *yield* keyword inside it is sometimes referred to as *yielding* to blocks. This functionality of *yielding* to blocks provides a Ruby programmer a lot of flexibility. It allows us to, for example, keep repetition to a minimum by letting us "inject" code into our method (wherever we place *yield*).


### **How to use *yield* in its simplest form**

What I mean by "simplest" form is: *yield* without any arguments or something like **`block_given?`** and without passing it any blocks.
<br><br>
When we are interested in passing a custom block to a method (and we know that we will always pass it a block), we can simply add *yield* inside the method where we want to insert (inject) the code we pass in via our block.
```
def method_2
 yield
 puts "Second line"
end
```

Then, when we call this method, we must pass a block to the method. If we fail to provide a block in the above scenario, we will get an error because the method reads the line with the *yield* keyword and **expects** to receive a block to insert into that spot.
<br><br>

We use one of the following ways to pass a block to our method:<br>
(1) <br>
`method_2 { puts "First line }`<br>

(2)
```
method_2 do
 puts "First line"
end
```

Both of these will produce the same output:
```
First line
Second line
```
<br>
***Reminder:***<br><br>
Curly braces **`{ }`** are preferred for singles line blocks.<br>
**`do ... end`** is preferred for multi-line blocks.<br>


### **How to Make *yielding* to a Block (i.e. Passing a Block to a method) Optional**

The *yield* keyword is great, but it can be a major inconvenience if we want to use a method with *yield* inside it without passing a block to it. Fortunately, Ruby accounts for this by providing us with the ***block_given?*** method. By adding **`if block_given?`** after the *yield* keyword, we make passing in a block optional.<br>

Let's test this out using our previous example:
```
def method_2
 yield if block_given?
 puts "Second line"
end
```

Now, when we called the block we have the option of passing it a block or not passing it one.
If we pass it a block, we type something like: **`method_2 { puts "PASSED IN THIS BLOCK" }`**.

This will produce the output:
```
PASSED IN THIS BLOCK
Second line
```

What do you think will happen if we just called the method without passing it a block? Think about this for a moment before continuing.
<br><br>
***Ready?***
<br><br>
When we call this method by typing `method_2`, we get the following output:

```
Second line
```

What happened here? Why didn't we get an error this time? Think about this for a moment before continuing.
<br><br>
***Ready?***
<br><br>
The reason we didn't get an error this time is because when our code was being parsed (read), Ruby came to the line **`yield if block_given?`** and evaluated it. By writting this in, we are explicitly telling Ruby that we want it to expect a block and replace *yield* with it **ONLY** if we pass a block to the method. When it sees that we did not pass a block to the method, Ruby continues on to the next line of code without issue. In the case of our example, the next line told it to output *Second line* to the screen.


### **How to use *yield* with Arguments**

The *yield* keyword can also accept arguments. These arguments are then used by the block.

Take a look at the following method:

```
def method_3
 yield 7
end
```

When we call it by passing a block: **`method_3 { |x| x + 5 }`**
<br>

We get the following output:
```
12
```

To understand what happened, let's break this down.

**First:**
<br><br>
*yield 7* in our method accepts the integer '**7**' as its argument.<br>
(Note: another acceptable way to pass in a single argument is *yield(7)*)
<br><br>

**Second:**
<br><br>
When we pass in a block, we need to provide the block with the same number of variables as the number of arguments we passed to *yield*. If we pass three arguments to *yield*, we need to provide the block with three variables (one for each argument). For our example above, when we call the method by typing **`method_3 { |x| x + 5 }`**, we provide it with a variable '**x**' by placing it inside the pipes (vertical bars/lines) **`||`**. 

**Third:**
<br><br>
Each variable we provide the block with takes on the value of each of the arguments we passed to *yield*. In our example, the '**x**' is assigned the value of the argument we passed in. So, in our example, the  '**x**' is assigned the value of '**7**'.

**Fourth**
<br><br>
When we call our method above using the block in our example: **`method_3 { |x| x + 5 }`**, the code of our method temporarily looks something like:

```
def method_3
 x = 7
 x + 5
end
```

As we saw previously, this  outputs `12`.

#### ***Passing Multiple Arguments to yield***

Passing multiple arguments to yield would look something like the following:

```
def method_4
 yield 7, 3, 8, "cats"
end
```

We are passing **four** arguments to *yield* in this example #=> **7**, **3**, **8**, and **"cats"**. Three of the arguments are numbers. One of the arguments (the last one) is a string. *Note: When passing multiple arguments to yield, I had to list the arguments without parentheses.*
<br><br>
Let's call this method by passing a block:<br><br>
`method_4 { |x, y, z, animals| puts "They had #{animals} that were of ages #{y}, #{x}, and #{z}." }`
<br><br>
When assigning variables, the order of the arguments and variables is important. The first argument in *yield* is assigned to the first variable of the block; the second argument in *yield* is assigned to the second variable of the block; etc.
<br><br>
That being said:
<br>
x = 7<br>
y = 3<br>
z = 8<br>
animals = "cats"<br>
<br><br>
Once you assign the variables in the correct order, you can use them however you see fit in the rest of the block. In our current example, I use them in a string for string interpolation and place them out of order to demonstrate this point.
<br><br>
The "temporary" code generated when calling this method with the block from our example would look something like:
```
def method_4
 x = 7
 y = 3
 z = 8
 animals = "cats"
 
 puts "They had #{animals} that were of ages #{y}, #{x}, and #{z}." 
end
```

The output for our example is:
```
They had cats that were pf ages 3, 7, and 8.
```

After our coding finishes running, the code in our method above would return to looking like:
```
def method_4
 yield 7, 3, 8, "cats"
end
```

#### ***Passing an Existing Variable as an Argument to yield***

This should not come as a surprise, but we can pass a variable as an argument to *yield*

It would look something like:

```
def method_5
 w = "Hello Universe!"
 yield w
end
```

This would work the same way as the previous examples covering passing arguments to *yield*.


**Also:**
<br>

*yield* with arguments follows the same rule as "basic" *yield*  (without arguments): we **must** pass the method a block unless the *yield* keyword is followed by the `if block_given?` condition.
<br><br><br>
**FINAL NOTE:**
<br>
As a final note, know that even though you can place yield in multiple spots within a method, all of the yields accept the SAME block. What I mean by this is that you can pass only one block. If you wanted to, for example, customize your method by passing it two separate (different) blocks, you will not be able to do this with *yield*.
<br><br>
You will need to use ***lambdas*** or ***procs*** instead.

*That is all I have to say on yield. If I come across one or more clever uses of yield, I will make a post about it. The same hold true if I comes across potential pitfalls of using yield.*
















 
