---
layout: post
title:      "Ruby: What the Heck are Blocks?"
date:       2018-08-13 18:10:58 -0400
permalink:  lets_talk_about_ruby_what_the_heck_are_blocks
---

Note: Please excuse the poor formatting. The blog appears properly formatted when I preview it, but it messes up once it goes live. I will try to correct this.

## { Summary }

This post talks about **blocks**, what they are, and some of the ways there are used. Before I get started, note that other programming languages may refer to **blocks** as *closures*.<br>
<br>
**Blocks** in Ruby are those bits of code that can be placed within curly braces  **`{ }`**  or between  **`do ... end`**  and passed to method--yes, sort of like how some methods accept arguments. We often see this when using common enumerators like *<a href='https://ruby-doc.org/core-2.4.0/Integer.html#method-i-times' target='_blank'>#times</a>*, *<a href='https://ruby-doc.org/core-2.5.0/Array.html#method-i-each' target='_blank'>#each</a>*, or *<a href='https://ruby-doc.org/core-2.5.0/Array.html#method-i-map' target='_blank'>#map</a>* (same thing as *<a href='https://ruby-doc.org/core-2.5.0/Enumerable.html#method-i-collect' target='_blank'>#collect</a>*). All that stuff we put inside of curly braces and `do ... end` after calling the enumerator method are actually blocks: chunks of code we're passing to the enumerator that gives it specific instructions on what to do with each item it iterates over.<br>
 <br>
 One of the coolest ways **blocks** can be used are for *yield*. I provide examples of this, but I do not go into a lot of detail regarding that in this post.
 <br>
 <br>
 
 ## { The Long version }
 
For Ruby beginners, it may be difficult to grasp what **blocks**  are, so I will try to guide you through it.
<br>
 
### Methods and Arguments (a review)

To understand a little bit about blocks, let's talk do a quick review of methods and arguments.<br>
Normally, when we create (define) methods, we can choose to give it an argument, or we can choose not to.

**Example 1: (no arguments)**

This method does not have a parameter list, so it does not accept any arguments:

```
def say_hello
 puts 'Hello!'
end
```

When we call this by typing `say_hello`, we get the following output:
<br>
<br>
`Hello!`
<br>
<br>
Here, we were able to call the method without providing any methods
<br>
<br>

**Example 2: (with arguments)**


The following method, however, has a parameter list that accepts two arguments:

```
def add(num1, num2)
 sum = num1 + num2
end
```

When we call this by typing `add(5, 10)`, we get the following output:
<br>
<br>
`15`

Here, **5** is **num1** and **10** is **num2**. The method plugs these arguments into the respective local variables (*num1* and *num2*), adds them, and returns the sum.
<br>
<br>
Simple enough, right?
<br>

Now, let's see how **blocks** work when called by a method.
<br>
<br>

### Blocks

#### [1] Defining  Blocks:
<br>
Blocks are pieces of code that are often passed to methods in a way similar to arguments.<br>

The difference, however, is that a block can span a single line or multiple lines. It might also have variables of its own as well as conditional statements.

If a block is short and fits nicely on a single line, we often write it inside of **curly braces** **`{  }`**.<br>
If the block spans multiple lines, however, we often write it inside of **`do ... end`**.
<br>
<br>

#### [2] Blocks in Action : *(without variables)*

<br>
<br>

**Example 1: (curly braces - no variables)**

Here is an example of a method accepting a simple block that we are passing it on a single line without a variable.<br>
(Note: the part with the curly braces and everything inside it is the block).--` { puts "Hello" } `
 
```
5.times { puts "Hello" }
```

The output is:

```
Hello
Hello
Hello
Hello
Hello
```
 
We called a ruby method here named *<a href='https://ruby-doc.org/core-2.4.0/Integer.html#method-i-times' target='_blank'>#times</a>*.  This built-in Ruby method accepts a block and repeats that action of the number of times specified (by an integer). Here, we essentially told it to output 'Hello' five (5) times.
<br>
<br>

**Example 2: (`do ... end` - no variables)**

Let's look at the same example using **`do ... end`**.<br>
(Note: the part within and including the **do** and **end** is the block)
 
```
5.times do
 puts "Hello"
end
```

The output is:

```
Hello
Hello
Hello
Hello
Hello
```

Note that this is the exact same method as above. Both of them used the Ruby method *<a href='https://ruby-doc.org/core-2.4.0/Integer.html#method-i-times' target='_blank'>#times</a>*, which accepted a block to output 'Hello' and outputted it five (5) times. The difference here is that we placed the block inside of **`do ... end`** instead of curly braces **`{  }`**.<br>
<br>
Note: Since the block is single line, the curly braces are preferred as it takes up less space, but **`do ... end`** would still be acceptable (with grumbles from some Rubyists).
<br>
<br>

**Example 3: (Doing the same things using arguments)**

Now, let's see how we would do this if we wanted to do it using a method that accepts arguments.

```
def repeat (string, num_repetitions)
 until num_repetitions == 0
  puts string
	num_repetitions -= 1
	end
end
```

This is a contrived and very simplified example, but it does pretty much what we were doing in the previous examples: outputting a string a specified number of times.

If we typed, `repeat("Hello", 5)`, we would get the following output:
```
Hello
Hello
Hello
Hello
Hello
```


One primary thing to note here is that we cannot enter a customized block. The only things we are allowed to enter are a *string* and a number for the amount of times we want the action repeated.<br>
<br>
<br>
What if we wanted to customize the block we put in? This can be done with a feature of Ruby called **yield**. I will focus on **yield** in my next post. For now, note the following example where I can pass a custom method a block using **yield**.
<br>
<br>
Let's use the same method we created before:
<br>
<br>

```
def repeat (num_repetitions)
 until num_repetitions == 0
  yield
	num_repetitions -=1
 end
end
```

<br>

Now, when I call it, I can pass both an argument and a custom block to it. In this scenario, you can think of **yield** as a special kind of variable that gets assigned the block. So, the method will replaces **yield** wherever **yield** appears inside the method. Each time we call the method, we can pass a different block.
<br>
<br>
<br>
**Example A: (passing a simple custom block)**

If we type `repeat(3) { puts 'I am passing this block in for example 1' }`, we get the following output:

```
I am passing this block in for example 1
I am passing this block in for example 1
I am passing this block in for example 1
```
<br>

**Example B: (passing a simple custom block)**

Now, if we type `repeat(3) { puts 'Block I passed in for example 2' }`, we get the following output:

```
Block I passed in for example 2
Block I passed in for example 2
Block I passed in for example 2
```

Note how the output changed based on what we passed in even though everything else about the method is exactly the same.
<br>
<br>
<br>

#### [3] Blocks in Action : *(with variables)* 

<br>
*Note: I use an enumerator within a custom method, but it is unnecessary. Calling the *<a href='https://ruby-doc.org/core-2.5.0/Array.html#method-i-each' target='_blank'>#each</a>* method directly on the array would have been enough.* 
<br>
<br>


One of the ruby methods wecommonly pass blocks to is the *<a href='https://ruby-doc.org/core-2.5.0/Array.html#method-i-each' target='_blank'>#each</a>* method, which iterates over a collection (like an array).


Let's assume we have an array of integers represented by the variable *numbers*:
<br>
<br>
`  numbers = [1, 3, 5, 7, 11, 13, 17]`.
<br>
<br>

**Example 1: (Passing a block with a variable**
<br>
We can iterate over these numbers using a custom method that accepts an array as an arguement and then iterates over it using *<a href='https://ruby-doc.org/core-2.5.0/Array.html#method-i-each' target='_blank'>#each</a>*. We tell the *#each* method what to do with each item in the array by passing a block to it.

```
def more_less(numbers)
 numbers.each do |x|
  if x> 5
	 puts "#{x} is more than 5"
  elsif x == 5
	 puts  "#{x} is exactly 5!"
  else
	 puts "#{x} is 5 or less"
  end
 end
end
```

The block we are passing in is the `if ... else` statement enclosed within the `do ... end`. Since this is a multiline conditional, we place the block within the `do ... end` instead of the curly braces `{  }`.
<br>
<br>

If we type `more_less(numbers)` we get the following as our output:

```
1 is 5 or less
3 is 5 or less
5 is exactly 5!
7 is more than 5
11 is more than 5
13 is more than 5
17 is more than 5
```


Note on a variable's scope within a block: The scope of the variable we pass in to the block is local to that block. The variable has no meaning for the program outside of it (or, if assigned outside the block, it will not have the same meaning). So, even if we assign 'x' a value inside of the block, it would not have that value outside of it. A discussion of scopes related to variables within blocks likely merits its own post, so I will not go into further detail here.
<br>
<br>

**Example 2: (passing in a custom block)**
<br>
<br>
What if we wanted to do something different to this method? We can use **yield** to do this. *Remember: the block we pass into a block takes the place of any yield within the method.*

```
def more_less
 yield
end
```

We can assign our method the same functionality as we did before by passing the appropriate block. *Note that this is slightly hairy since the block we are passing is also accepting a block*<br>
So, here, we would type the following:
<br>

```
more_less do
 numbers.each do |x|
  if x> 5
	 puts "#{x} is more than 5"
  elsif x == 5
	 puts  "#{x} is exactly 5!"
  else
	 puts "#{x} is 5 or less"
  end
 end
end
```

One thing to note here is that we have enclosed the `numbers.each do |x| ... end` block we are passing to the method `more_less` within the additional set of **`do ... end`**. We could have also passed in the block by placing it within curly braces, as follows :

```
more_less { 
 numbers.each do |x|
  if x> 5
	 puts "#{x} is more than 5"
  elsif x == 5
	 puts  "#{x} is exactly 5!"
  else
	 puts "#{x} is 5 or less"
  end
 end
 }
```

That looks slightly more readable, in my opinion. You might have noticed that the method does not accept an argument. We simply pass in the block as the argument to take the place of **yield**. The output will be the same regardless of which syntax we use to pass the block:

```
1 is 5 or less
3 is 5 or less
5 is exactly 5!
7 is more than 5
11 is more than 5
13 is more than 5
17 is more than 5
```

<br>
<br>
**Example 3: (passing in a different custom block)**
<br>
<br>
Let's use the same method as before:

```
def more_less
 yield
end
```

This time, let's assume the program we are working on has a method that needs to do something different with the numbers array from earlier. Let's pass in a different block to it.

```
more_less { numbers.each { |x| puts x*11 } }
```

Here, the block we are passing is a single and also accepts another block that is also on a single line. As a result, we place each block in its own set of curly braces `{  }`. We could have used a combination of `do ... end` and/or `{  }` to accomplish the same thing.
<br>
<br>

Regardless of the syntax we use, the output would be:

```
11
33
55
77
121
143
187
```

Well, that's about all I have to say about the basics behind blocks, what they are, how to recognize them, and a few ways we use them. I hope that clarifies some of confusion behind what blocks are.














 
