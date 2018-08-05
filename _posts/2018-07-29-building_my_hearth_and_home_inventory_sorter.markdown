---
layout: post
title:      "Building My Hearth & Home || Inventory Sorter"
date:       2018-07-29 16:53:41 -0400
permalink:  building_my_hearth_and_home_inventory_sorter
---


##### *[WIP]*

You can find the app here:
https://github.com/AAM77/hearth_and_home_inventory


### ** [ The TL;DR version ] **

(I plan on updating this blog post with links to other relevant posts and videos I create. I just finished working on this and took far longer than I had planned, so I don't want to spend too much time on a detailed blog post, which would be too long anyway. I will proofread and update this as I have time).

 I had so much fun working on this project, but it had its challenges. The bumps in the road taught me a lot, including, but not limited to: (1) how to write and use procs, (2) what erb partials are and how to use them, (3) how to use yield properly, (4) a better understanding of ActiveRecord associations, (5) a better understanding of how RESTful routes work, (6) how validations work and how to skip validations using #save(validate: false) under certain conditions, (7) improved familiarity using bootstrap.

I also learned that using ‘unless’ with negations is preferred, as is using ‘single quotes’ except for cases involving interpolation.

Also, I don't go into too much detail in this blog. I am going to make a video that outlines everything. It was a long process and I encountered several headache inducing issues that I eventually figured out. I will link to the video at a future date.

The link to web-app on heroku is: https://hh-inventorysorter-sinatra.herokuapp.com/

My next step is to work on the app again after graduation to improve it.


### ** [ The Long Version ] **

<br>
#### **Description**

The project is built on a relatively simple concept. A user registers and logs in. (S)he makes folders and categories. (S)he adds items to folders and categories. Moreover, users exist independently of one another—they cannot see each other’s content.

In a sense, it is a catch-all for organizing and making an inventory of whatever the user desires. The inspiration for, it however, came from a discussion surrounding the usefulness of keeping an inventory of one’s possessions for insurance purposes. Now, the first thing that came to mind was allowing users to upload evidence in as image and videos.” This, however, requires a lot more storage than text alone. As a result, I decided to opt for text only for this project, especially since I planned on deploying it to Heroku.


<br>
#### **The Beginning (Planning)**

I started by planning out what I needed to do. I did this by talking out what I wanted the program to do. Then, I wrote out what objects my program would need: (1) user, (2) items, (3) folders, (4) categories. I then wrote out a tree diagram that described how I wanted each of them to be associated.




```
      User   -----   can have many items (has_many)
      User   -----   can have many folders (has_many)
      User   -----   can have many categories (has_many)
  A Folder   -----   belongs to one user (belongs_to)
   Folders   -----   can have many items (has_many)
A Category   -----   belongs to one user (belongs_to)
Categories   -----   can have many items (has_many)
      Item   -----   belongs to one user (belongs_to)
      Item   -----   belongs to one folder (belongs_to)
      Item   -----   belongs to one category (belongs_to)
```

I changed the item’s associations in the end so that any unique item can belong to multiple categories and folders. The reason for this change was because I was thinking of “work” and “home” folders. But, then quickly realized that I might want to add that same item (with a unique id) to a category or multiple categories. Maybe someone wants to create a “books” folder and wants to add the same book to both folders.

So, the final associations for the Item’s relationship with folders and categories to be:

```
       Item   -----    can have many folders (has_many)
       Item   -----    can have many categories (has_many)	
```

I now had a has_many on folders and items, as well as on categories and items. This created a many_to_many relationship between them. Well, I made join tables to address this, which I wont’s go into detail here. You can check a blog post I wrote on how to set these up in the models (http://adeelstechbeat.net/the_many-to-many_relationship_in_activerecord).

I then wrote out a basic MVC file structure on how I wanted it to look:

```

CONTROLLERS:

1.	Application Controller
2.	User Controller
3.	Item Controller
4.	Category Controller
5.	Folders Controller

MODELS:

1.	user.rb
2.	item.rb
3.	category.rb
4.	folder.rb



VIEWS:

1.	layout.erb
2.	welcome_index.erb
3.	users folder
         create_user.erb
         login_user.erb
         show_user.erb
         edit_user_info.erb
         delete.erb

4.	items folder
         items_index.erb (lists all of the items)
         create_item.erb
         show_item.erb
         edit_item.erb
         delete_item.erb

5.	categories folder
         categories_index.erb
         create_category.erb
         show_category.erb
         edit_category.erb
         delete_category.erb
 
6.	 Folders folder
         folders_index.erb
         create_folder.erb
         show_folder.erb
         edit_folder.erb
         delete_folder.erb

```


Beyond this, though, I had nothing set up and did not know where to begin. I decided to look online for a gem that would create a basic skeleton structure for Sinatra apps. Fortunately, I came across Corneal (https://github.com/thebrianemory/corneal). Corneal was developed by Brian Emory, who was a former Learn Verified student at Flatiron. The gem created a file structure similar to what I had been working with so far in Flatiron. It also provided a basic erb test page that told me everything was running as intended. This made getting started much easier.

I had basic features in mind, but I went through a process of adding and removing features as I worked on the website’s design. 


### **Some Challenges**
<br>
#### **Some initial issues**

**[1] Tracking the Current User**

I wanted to make sure that a user could login and logout. I needed a way for my program to recorgnize and keep track of who was logged in at any given instance.

I addressed this by:
First, setting up a route for sign up and login. After passing a series of tests, the sign up route creates a new user and a new session hash ``` session[user_id]``` . The login does the same thing, except that it finds the user's entered username among existing users in User.all. The logged in user's id is then assigned to session[user_id].

Like any key value pair in the params hash, this session hash's key-value pair is created for each user and exists until it is replaced or cleared. This is why we create it only once on login and clear it on log out.

I used the session[user_id]'s value (if it exists) to track the current user easily and check if the user is logged in.

I created two method to handle this:
(1) current_user, which locates the user by find_by(id: ) using the session[user_id]'s value for the id and assigns it to an instance variable @current_user

(2) logged_in?, which checks if the instance variable @current_user exists. If it does, it returns a boolean of true.


**[2] Validations**

I wanted to make sure that a user could not be created and saved to the database unless it had a username, password, and email. I addressed this by adding the following line of code to my user model:

```
validates_presence_of :username, :email, :password
```

I also wanted to make sure items, categories, and folders did not get created without a name, so I added the following line of code to the respective models:

```
validates_presence_of :name
```

I did have some issues with saving (updating) edits to my user because of the validations. It kept asking for a password.
In hindsight, requesting a password as confirmation before hitting edit or delete would be a good feature, but I was able to get around this for the time-being by using

```
@user.save(validate: false)
```

This skip validations for that particular save and it allowed me to save edits to the user's info without requiring the user to enter a password.


**[3] Creating a Secure Encrypted Password**

The next thing I needed to address was to make sure the user's password is secure. I did this by using bcrypt gem and adding the following line of code to my user model:

```
has_secure_password
```

Bcrypt is a great gem in that it takes the input password and creates a hash using predefined rules. Now, hashes generally have keys that are able to make sense of them. The hash is created in the same way for each password, so two passwords would have the same hash. This allows admins (and hackers) to retrieve the passwords so long as they have the pattern/key that was used to generate the hash. Bcrypt does something extra, however: it salts the hash---that is it adds a completely randomly generated set of characters to each hash. This way, even two same passwords would have different hashes. It stores these hashes in password_digest.

This means, of course, that the password can no longer be retrieved. If a user wants to change a password or forgets a passwords, the admin will need to generate a new random password for that user and send it to them using secure means. The user will then be able to use this to reset their password. It's a hassle, yes, but security comes with a price.

Of course, this does not means hackers cannot find the password. It would just take them a very long time.

***I don't think I did a good job explaining it, so:***

You can read more about bcrypt at:
https://github.com/codahale/bcrypt-ruby
https://www.youtube.com/watch?v=O6cmuiTBZVs

You can also learn about password security at:
https://www.youtube.com/watch?v=8ZtInClXe1Q


**[4] Object Names**

Another issue I ran into was  a concern over how to deal with names for the folders, objects, and items. I wanted to, for example, make it so each user had a folder for 'work' and 'home' and I wasn't sure at first if this would be an issue. Fortuantely, not. This is because each new folder would have a unique object id, different from every other object, even if the names are the same. Also, each object has a user_id associated with it, so I would be able to find the folder, category, or item, by using the object's id or by using the user's id and object's name.

This was slightly trickier for the item, as an item with the same name could belong to multiple users, but also to multiple folders and categories. So, to address this, I needed to also take the folder's or category's id into account.

**[5] Misc.**

I ran into other issues as well, but I will not go into detail here, so I will just mention that I did try to include tests, but I veered away from it. This was unfortunate, as test driven development (TDD) is the best way to approach something like this. I did not have enough experience writing tests, so I was afraid it would take me much longer to complete the project if I went this route. I plan on learning TDD as I work my way through the next section of the curriculum.


#### **Refactoring**

The refactoring stage was, perhaps, the longest stage of development.
Now, I should point out that I was done with this project ONE WEEK AGO. It was very simple in terms of visuals, but it behaved exactly as I wanted. The problem was, I could not get myself to hand it in with at least attempting to 'DRY' it.

I had two models (folder and category) that behaved identically and had almost identical routes, that it added a lot of repetition to my code.

(1) I had helper methods with different names and variables doing the exact same thing.
(2) I had class and instance methods in each of the models doing the exact same thing.
(3) I had almost identical views for folders and categories.

**[1] application_controller.rb : Helper Methods**

Now, this would have been simple enough if they had the same exact variables, etc, but they didn't. Also, different parts of my program needed the entire method, but needed to insert some line of code into a specific spot.

Fortunately, I was able to address the latter using

```yield if block given?```

I will cover in a separate blog post within the next two weeks and update this part by linking to it here.


Now, there were parts of my method that needed a single line of code in multiple places and they needed it each time. The problem was that different parts of the methods did the same thing, but they referenced either the Item class or Folder class or Category class. Also, I could not use multiple yields, as each yield can handle only one block.

One of the other (bigger) problems I had was that the methods had an iterator variable. You know the one. I'm sure you've seen it. They look like this:

```
apples.each do | apple |
       puts "Nom nom nom. Yummy apple."
end

```

You see that 'apple' between the | pipes | ? Well, that was throwing me off. Long story short, after a long trial and error session, I learned about procs and lambdas, which I will cover in another blog post and post here. The bottom line is that procs are like mini methods that cannot operate on their own. They're kind of like insertable methods that can be assigned variables. The procs can also be assigned to a variable.

Here, is what a proc would look like: 

```
find_category = proc { |category_id| current_user.categories.find_by_id(category_id) }
```

Here's another example:

```
item_to_new_category = proc { |new_category, selected_item| new_category.items << selected_item }
```

And, I can have a method accept the proc and use it by assigning it in the parameters and using the #call method, as such (it is what one of my final methods ended up looking like:

```
def add_selected_item_folder_category(ifc_ids:, new_object:, find_proc:, add_item_proc:)
      if ifc_ids
        yield if block_given?

        ifc_ids.each do |ifc_id|
          selected_ifc = find_proc.call(ifc_id)

          if !selected_ifc.nil?
            add_item_proc.call(new_object, selected_ifc)
          end
        end
      end
    end
```

I also learned (after installing and reading up on rubocop (https://github.com/rubocop-hq/rubocop) that there are some conventions I violated (heavily) while making this app. Two of them include (1) whenever possible, it is better to use single quotes '   ' instead of "  " unless we need to do something like interpolation, (2) when using negations, like ```  if !selected ``` it is better to use unless. There are others, but I will keep these in mind from now on. I spent way too long on this project already (even though I thoroughly enjoyed it).


By the way, those named arguments (e.g. ifc_ids: , new_object: , etc.) are awesome. I kept forgetting the order I needed to place in the arguments, but those helped a ton.


**[2] Class & Instance Methods in Models**

I addressed this by creating a concerns folder and creating modules for each relevant model with "ClassMethods" and "InstanceMethods" modules nested inside each of them. I then added the in-common methods to the respective modules. Afterwards, I provided the respective models access to these modules by adding "extend" and "include" for class methods and instance methods, respectively, to each of them.

The issue I ran into, though, was that the 'require_all app'  in my ./config/environment.rb was not recognizing the modules. To address this, I simply added 'require_relative [ path to relevant module file ]' to the top of the respective model files.


**[3] Identical Views for Models and Categories**

 I fixed this issue by using something called partials . Partials are erb files that have content inside them, just like any other erb file. We can call erb files into other erb files by using the following pattern:
 
 ```
 <%= erb(:"[ path to the erb file]") %>
 ```
 
 If you have variables you are using inside an erb you are calling, it will raise an error.
 Fortuantely, there is a way to address this as well. You can assign values to the erb variables in the following way:
 
 ```
 <%= erb(:"categories_folders/category_folder_edit_partial", :locals => { :erb_variable1 => assigned_value1, :erb_variable2 => assigned_value2} %>
 ```
 
 The assigned values can also be variables. You can also chain erbs in this way, but I won't get into it here. I will write a separate blog about it.
 
 Here is an example of how I called an erb in one of my views:
 
 ```
 <%= erb(:"categories_folders/category_folder_edit_partial", :locals => {
  :object => @folder, :object_string => "folder",
  :edit_path => "/"+current_user.slug+"/folders/"+@folder.slug+"/edit"
  }) %>
	```
	
	So, essentially, I created joint view partials called:
	```
	category_folder_create_ partial.erb
	category_folder_edit_ partial.erb
	category_folder_index_ partial.erb
	category_folder_show_ partial.erb
	```
	
	These contained the template for the folders and categories, with general variables that I assigned once I called the erb.
	
After this, I went a little overboard and made partials for anything else that I had an issue with.
I also learned a neat trick with partials. I do not need to have complete opening and closing tags in each partial. I can put the opening html tags in one partial and the closing tags in another. I can then join them together by calling the relevant erb.

Here are examples:

In the first erb, I had the top portion:
``` 
<div class="accordion" id="accordionExample">
  <div class="card">
    <div class="card-header" id="<%= heading_num %>">
      <h5 class="mb-0">
        <button class="btn btn-info btn-lg btn-block" type="button" data-toggle="collapse" data-target="#<%= collapse_num %>" aria-expanded="false" aria-controls="<%= collapse_num %>">
```

In another, I had the middle portion:
```
    </button>
  </h5>
</div>

<div id="<%= collapse_num %>" class="collapse show" aria-labelledby="<%= heading_num %>" data-parent="#accordionExample">
  <div class="card-body">
````

And, in yet another, I had the bottom portion:
```

      </div>
    </div>
  </div>
</div>

```

I called them in a separate erb (along with other html and erbs I inserted in between) to get my layout to look and behave the way I wanted it to.

This allowed me to address an issue when I needed the same style and behavior for collapsible divs, but couldn't get it to work correctly because I didn't need some of the variables. Now that I think of it, there was probably a better way, but it was a neat trick to learn, regardless.



#### **Issues & Future Plans**

I overused some techniques (like partials). Sometimes, readability is more important than making an app DRY. Partials are great for inserting content into layouts, though.

I could not get the validation messages to work correctly, so I manually created checks and flash messages in the controllers.

My future plans include:

 - (1) Upload to Heroku - [ DONE ]
(2) Add a password reset feature
(3) Add a "Log in with Facebook/Google" feature, so that they can handle all of the username/password headaches.
(4) Add in the ability to upload images and videos (as time permits).
(5) Clean up the design using new techniques and knowledge I learn.
(6) Read up on good habits and conventions to follow when coding.
(7) Implement these good habits and conventions.
(8) Write blogs about the useful techniques I learned

#### **The Link to the App on Heroku**

You can try out the app on Heroku by visiting: https://hh-inventorysorter-sinatra.herokuapp.com/

