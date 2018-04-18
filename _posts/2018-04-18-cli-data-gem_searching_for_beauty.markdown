---
layout: post
title:      "CLI-Data-Gem: Searching for Beauty"
date:       2018-04-18 19:15:29 -0400
permalink:  cli-data-gem_searching_for_beauty
---


**Quick Note:**
I intend on continuing with my Bash series, but not until after I submit CLI Data Gem Project and pass the review.

The idea for my app came from a personal desire to search for items, not just by their product name, but by whether they contained a specific ingredient or not. Sometimes it had to do with the material the item was made from. The point is, I wanted to search by an item's specification, not just by its product name. So, it seemed like a good idea for this project, but there were somet things to consider first.


**THE TCoS & ROBOTS.TXT** <br />
Scraping a website for its data was not a major issue years ago. This was not because the ethical and moral dilemmas were not present---they were. Rather, it was because few individuals, if any, had addressed the legality of such issues. Today, however, it is good practice to not only read the website's  Terms and Conditions of Service (i.e. ToS, TCoS, ToU), but also its Robots.txt. The TCoS explains whether the site's owner allows or not and---if allowed---what constitutes a breach of the site's TCoS. Pulling up the site's TCoS and reading through is always recommended, but it is sometimes possible to search through the document using  Cmd-F (Mac) and Ctrl-F (on Windows). Searching for keywords like "scrape" and "data" may help to pull up the relevant parts faster. The Robots.txt, on the other hand, indicates what the scraper is allowed or not allowed to scrape. I usually search for it by typing in the web address I am interested in and then "Robots.txt" after it (e.g. www.amazon.com   Robots.txt). This will turn it into a Google search and pull up the page you are looking for. Be careful, however, because even though most websites' Robot.txt allows Google---one of the largest scrapers on the web---to scrape their pages, this does not mean it allows any user to scrape it. This is why it is important for programmers to consult both the TCoS and the site's Robot.txt before interacting with it.

The site's owners have mechanisms in place to detect scraping. Therefore, even if someone  does not care about the moral or ethical impliations, he or she should care about the legality of it all---it could come back to haunt them. In many cases, the site will simply block the user's IP, ranging from a couple or minutes to a permanent ban. But, if one's scraper is particularly intrusive (or the person is simply unlucky), the site's owners may choose to take legal action against the scraper's user. Please note that I am not a lawyer and this is not legal advice, but rather a friendly word of caution from a fellow coder. Respect the site owner's wishes.

**THE LIMITATIONS OF NOKOGIRI** <br />
So, with all of that in mind, I set out to search for a website and decided to go scrape Sephor for product details. Unfortunately, as I was working on its scraper, I learned that Nokogiri was not capable of scraping everything I needed. This was because the website used Javascript. Since Nokogiri parses only HTML and XML, it meant I had to either find a different website to scrape or learn how to scrape the Javascript portion. I spent a decent amount of time trying to learn how to do this, but it turned out to be too time costly and the information I was finding was not particularly helpful. Much of it was outdated and the commands they provided were deprecated. I did not have time to read through the Gems' manuals and learn through trial and error---I was more interested in completing the project in a timely fashion. So, I decided to scrape a different website and settled for Cult Beauty. I named my scraper ***Beauty_Product*** (*I know: ingenious, but Beauty_Scraper seemed in poor taste*) and got to work.

**BOTTLENECKING ISSUES**
<br />
When I finished the first version of the scraper file for Cult Beauty, it was working fine, but there was a problem: it took too long to run---long enough that the session would timeout. So, what did I do? I went looking for ways to speed it up. In the end, I learned that bottlenecking was inevitable with web scrapers. My computer might be able to parse through thousands of local files relatively quickly, but this was not the case with files stored on a foreign server: the exchange rate across the internet simply was not fast enough. In addition, Ruby may be pretty fast for scraping, but it does not appear to be the fastest overall (it is ranked somewhere closer to the middle in terms of speed). After learning this and evaluating my options, I settled for scraping the site's Sale's page. It took roughly *32 seconds* to scrape the data for all of the items on sale, which was far more feasible than waiting several minutes. This change also forced me to simplify my plans for the project. I had planned on allowing the user far more options, but getting it to work with only a few options was already taking long enough.


**WHAT I LEARNED** <br />
This project demonstrated the love-hate relationship coders have with programming: the process can be frustrating and exciting almost simultaneously. I learned a lot while working on this project and have become much more comfortable with many of the concepts covered in the bootcamp so far. I did not push my changes to my Github repository as regularly as I should, but I am gradually developing a good habit of doing so. It's like developing a healthy habit of saving a game several times: no one likes having to go through hours of gameplay due to an ill-fated crash to desktop (CTD). I also got better at creating a new branch at each stage of development and pushing changes there before merging to the master.

One more thing I learned was how to balance planning, working in stages, and coding with potential future functionality in mind. I tend to plan extensively before starting any project, but overplanning is not always time efficient. Plans change and often need to be modified. Too little planning, on the otherhand, can lead to aimlessly coding for hours for beginningers, in particular. Personally, planning out an outline and the CLI's major features in the beginning helping me get going. Creating stubs to get it to work right kept things progressing at a steady rate and made it easier to test if the  program was working correctly. Adding functionality in stages helped break up overwhelming tasks into smaller parts and allowed me to be more flexible in my planning. If something went wrong, I only had to refactor part of the code, not the entire thing. As a final note, more than anything, I realized that it is okay to write imperfect code to get the program to work. I can improve its efficiency and readibility during the refactoring (proofreading/editing) stage. So, in many ways, coding is very much like writing an essay.


**SOME NOTES ABOUT MY APP** <br />
As it stands, the scraper's CLI allows the user to:

(1) Print a numbered list of sale products by alphabetical order
(2) Print a numbered list of sale products by alphabetical order
(3) Search by product name *(searching by a keyword or substring works)*
(4) Search for products that contain a user-specified ingredient *(searching by a keyword or substring works)*
(5) Search for products that do not contain a user-specified ingredient *(searching by a keyword or substring works)*

(Typing 'exit' terminates the program).

Choosing (1) and (2) returns to the main menu after executing.
Choosing (3), (4), and (5) gives the user the option to run the same search again, 
The scraper could be improved in at least a few ways:
(A) - It could provide a more unique list of ingredients.
         At present, the ingredients list does not remove extraneous text from the end of the ingredients. Since this includes
				 notes that the product's manufacturer or web site's developer decided to add in, it can get rather difficult to filter it
				 all out properly. Creative use of Ruby code plus regex may help me accomplish this, however.
				 
(B) - It could provide more item information.
         At present, it scrapes the site for the product's description and usage instructions, but does not display it to the
				 user. I chose to exclude these features for the time being due to time constraints.
				 
(C) - It could allow the user to create a username and populate a favorite's list, both of which are saved as persistent data.
         I really wanted to add this feature in, but it is unnecessary for the project's requirements. I hope to learn more about
				 how to do this and add it in as a pet project for my personal learning experience.
				 
(D) - A universal sub-menu would be great.
         In its current state, the scraper has a separate method for options (3), (4), and (5). The problem is that the code is nearly identical. The only difference is a single line that recursively calls the same respective method. I looked up how to remedy this by passing in a method name as a parameter. It would have looked as so:

				 
				 ```
				 
				 def sub_menu(method_name)
				        puts "\nWhat now?"
								puts "1. Search again"
								puts "2. Return to main menu"
								
				        user_input = gets.downcase.strip
								
								case user_input
								when "1"
								     send(method_name)
							  when "2"
								     main_menu
								else
								     puts "\nINVALID CHOICE"
								     send(__method__) #=> This runs the current method again
								end
								
								```
								
The idea was to place this method inside of each sub_menu.
Example:
```

   def match_product
	      puts "Enter product name: "
				user_input = gets.downcase.strip
				
				...(other code)...
				
				sub_menu(**:**match_product)
				
  end # match_product
   
```

and run that method again if the user entered "1." And, it worked beautifully---until I entered an "invalid choice," that is. Then it gave me an error indicating I did not provide enough arguments (the 'else' portion has no arguments).

I tried using defauly parameters `def sub_menu(method_name = nil) `  and  `def sub_menu(*method_name)`.
The former gave me an error when I encountered the 'else' in the case statement. The latter gave me an error when I selected "Search again," stating that it cannot call **[ ]**. I became frustrated at the amount of time I was spending trying to look up information to figure it out, so I decided to skip it for the time being. I am probably overlooking something and will find a solution soon enough.


That is about all I have to say on the matter, but you can check out my code at: https://github.com/AAM77/beautyproduct-cli-app

Suggestions and **constructive** feedback is always more than welcome.


So, remember the part where I said it's like writing an essay?
I am going to leave this as is and proofread/edit it later.

