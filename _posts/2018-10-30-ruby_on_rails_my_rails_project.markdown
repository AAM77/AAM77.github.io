---
layout: post
title:      "Ruby on Rails: My Rails Project"
date:       2018-10-30 04:05:33 +0000
permalink:  ruby_on_rails_my_rails_project
---

## [ An Event Task Manager ]


## Summary:
The title is self explanatory. This post covers the Rails project I have been working on and the process I went through. It took me a long time to finish this project. It has some front-end and back-end design flaws, but I plan on ironing those out after I finish the Flatiron web development curriculum. I learned that larger projects, like this one, require more planning than I put into it. I also learned that I need to get better at going step by step to add features and improvements to my programs. My git submissions also need to be more frequent and at shorter intervals.<br>

## About the Project
The project was inspired by two sources (1) a close friend and (2) a coincidental attempt to set up game night with friends (board games). There are probably apps out there for this idea, but we were too lazy to look for them and decided to use a chat group instead. It worked well enough, but the situation gave me the idea to create an app that lets people create events and tasks that their friends can join.<br>
<br>
I am highly critical of my own work and I don't consider the project to be much in its current state. I plan on making improvements a couple of months from now.

## Planning

The bottom line here is that it took me a lot longer to complete the project than I originally anticipated. I took a different route in planning for this project and it got out of hand.<br>
<br>
I was attempting a different planning technique, but it did not work for me. I will need to tweak it.
The following includes an overview of the models and controllers in my project.<br>

### Models & Controllers

**Models**

The basic plan for models was simple enough:

```
Users can create many events
Users can create many tasks

Events can have many users
Tasks can have many users
```

The Original Models:

1. Event
2. Task
3. User
4. UserEvents
5. UserTasks


A model I added later:

6. Friendships


**Controllers**

I added the following controllers to handle the routes:
1. application_controller     ---> `contains helper methods for the controllers`
2. static_pages_controller ---> `routes to the welcome page's view`
3. sessions_controller         ---> `routes to the log in page view. Handles creating a session at log in`
4. events_controller             ---> `routes to the events' views. Handles creating and destroying events`
5. tasks_controller                ---> `routes to the tasks' views. Handles creating and destroying tasks`
6. users_controller                ---> `routes to the users' views. Handles creating and destroying accounts`
7. users_tasks_controller   ---> `Handles a user's participation (join/leave) in a task and its completion`
8. friendships_controller*   ---> `Handles creating friendships between controllers *`
<br>
*(Currently) - this controller immediately adds the users to each other's friends list.
*(Future Plans) - will also handle creating and destroying friend requests

## Difficulties I encountered implementing the plan

Before I continue, I want to express a personal opinion I currently have: I consider Rails to be very powerful and very useful. That being said, I also feel it has way too much "magic." The high level of abstraction helps to save time, but it also reduces understanding. Using gems is convenient, but I believe that one should learn to implement their features manually before using a gem.<br>
<br>
This is why I decided to avoid using Devise and many other gems. I tried to do as much as I could with as few gems as possible. Figuring out how to implement some features without gems took extra time, however.<br>


### Difficulties I Encountered in the Models

**Users Model**

The first issue I ran into had to do with the Users model. I wanted users to be split into two sub-models: Admins and Non-Admins (or Organizers and Participants).<br>
<br>
For example:<br>
<br>
Any user who creates an event is an Admin of that event.
Any user who participates in an event's task is a participant in that event and task.<br>
<br>
Having a single admin for an event was simple enough, but I wanted to allow an existing admin to add new admins to the event. Think of the admin as the organizer. An event should be able to have multiple organizers. I researched how to do accomplish this and I came across two methods: <br>
<br>
(1) polymorphic associations
(2) single-table inheritance<br>
<br>
Each had pros and cons to it, but I struggled with deciding which one to use. I felt that the decision was crucial since it might become difficult to change this later on. In the end, I did not go with either of these. I tried implementing each one, but had trouble getting them to work the way I wanted (I was doing something wrong and I was growing concerned about the time I was spending on the project. I did seek help, but I won't get into the issues I encountered there (it involved too much waiting). I had already spent two weeks playing the waiting game while seeking help and it was becoming frustrating. This would probably be less of a problem when I am working with others.<br>
<br>
I was spending more time on the project than I planned and decided to abandon the idea of multiple admins. Instead, I opted for a single admin for each event and created an "admin_id" column for the event and task columns. Then, I linked them using a relatively crude method, which involved writing methods that set the admin_id to be the user_id of the current user (the person creating the event). I also wrote a method in the tasks model that set the admin_id for the task to be the same as the user who created the event it was associated with. I  initiated these methods using callbacks such as "before create."<br>
<br>
This was not the most efficient strategy, but I had taken up more time than I wanted researching the underlying concepts of and difference between single-table inheritance and polymorphic associations. It was not ideal and I dislike inefficiency, but it got the job done, allowing me to move on.<br>


**Event & Task Models**

These models were relatively straight forward, but I had to decide on how users would interact with them. The event model has two views (one for the participant and one for an organizer) that share partials. At first, it had only one view, but it got messy and required too much logic in the view (in the form of if-else statements).<br>
<br>
The issues I had trouble dealing with when it came to events and taks included:<br>
<br>
1. How to let a user join and leave a task (and the event)
2. How to control what a user see on another user's page (friendships was the solution)
3. How to control which users could join a task (friendships was the solution)
4. How to establish friendships
5. How to end friendships
6. How to control how the completion of a task got handled


**Friendships Model**

I wanted to have a system where only friends could see everything on a user's page and join events that the user creates. To do this, I needed to find a way to come up with a way to establish friendships between them. This is where the friendships model comes in.<br>


### Difficulties I Encountered in the Controllers

**Friendships Controller**

The users controller already handles creating accounts while the sessions controller handles logging in. The friendships controller handles friendships between two users. When a user adds someone as a friend, it creates a friendship (and inverse friendship) between them. When a user unfriends someone, it deletes that friendship (and inverse friendship). As I was working on the project, I realized that it would be better if it handled creating friend requests and did not create friendships unless the recipient accepts. I will change this later on.<br>


**User_Tasks Controller**

The user_tasks controller does something similar to the friendships controller in that it creates a relationship between two objects. In this case, it creates a "participation" relationship between a user and a task. This controller is responsible for creating and destroying this relationship when a user joins or leaves a task, respectively.<br>
<br>
It also handles the completion of a task. This would have been easier to deal with if I allowed the event and task admin to complete only one task at a time. I was interested in implementing a feature that allowed one to select and complete multiple tasks, however, so I decided to learn how to do that instead. I sought help and came across some code that seemed out of my understanding. So, I refactored and replaced it with code I could understand.<br>

### Other Issues

I had some issues with implementing OmniAuth. The problem was that a user was able to switch between accounts by simply typing in the path to log in via Facebook or something similar. I created methods that checked for such paths and rerouted users if they attempted to do this. I also created methods that prevents users from visiting the login and signup pages again. It took me some time to figure out how to implement some of this, as it involved figuring out how to track the path the user was currently on. Fortuantely, I found out I could do this using `request.path`.<br>
<br>
I also got caught up trying to learn Ruby & Rails conventions and attempted to follow those. Eventually, I gave up on this in favor of getting the project done. I will learn these as I continue to work on more projects.<br>
<br>

## Current Features

In its current state, the project works well enough, but it does not yet resemble what I envision it being in its complete form. For now, even the layout and color scheme are as they are for the purposes of making it look somewhat presentable.<br>
<br>
That being said, some of the current features of the app include:<br>
<br>
1. Being able to create an account.
2. Logging in using a manually created account.
3. Logging in via an existing social media account, such as Facebook or GitHub.
4. Logging Out of an account.
5. Changing details of an account.
6. Being able to create an event.
7. Being able to join the tasks in other friends' events.
8. Being able to friend and unfriend someone.
9. Being able to view one's or a friend's friends list.
10. Allowing participants to mark a task complete.
11. Allowing event admins to confirm completion of a task.
12. Users earn points for completing tasks, though I have not yet decided on how those points can be used.

## Future Features I Plan to Implement
Some of the features I plan to implement in the future include:<br>
<br>
1. Immediate friendships will be replaced by friendship requests. Friendships between two users will be established only after the recipient accepts.
2. An appealing and viable way to use the points collected.
3. A way to prevent users from cheating the system by obtaining points through completing personal events.
4. Consider changing the layout and website's design.
<br>
Those are the only goals I have in mind for now. I might come up with more when I sit down to plan this out further.


## Final Thoughts

This project took me a long time to complete and I went overboard in the amount of features I wanted to implement. However, this is the learning stage and perhaps one of the best places to do this. As someone who has worked on team projects at my places of employment, such freedom is rarely available in a work environment, as we are often required to follow certain guidelines. I also stubbornly did things the long and hard way so that I could learn how to implement such features without gems.<br>
<br>
After reviewing some of the projects others completed, I might design simpler apps for the next two projects. Regardless of this, I will sit down and spend more time planning this out than I did this time around. I also need to get a hang of test driven development.<br>
<br>

The GitHub page for my project: [AAM Event Task Manager](https://github.com/AAM77/aam_event_task_manager)








