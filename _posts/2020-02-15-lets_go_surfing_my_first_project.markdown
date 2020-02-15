---
layout: post
title:      "Let's Go Surfing: My First Project"
date:       2020-02-15 20:31:59 +0000
permalink:  lets_go_surfing_my_first_project
---

Let's Go Surfing: My First Project

WOW!! What an adventure! Very much like surfing. This project was by far the most challenging and difficult thing I have ever created. I think my emotional journey went something like this:

1. Alright, I've been taught a bunch. I'm not sure how much has stuck but, let's dive in and find out.
2. WHAT AM I LOOKING AT? *Where* do I start?? What are *these* files? I need them for *what?* What did Avi say in that video again? For 3 days I did what he did and I isn't working!
3. Okay. Set. Time to code.
4. Hahaaa!  Look at this. I'm getting somewhere.
5. WTF!!  What happened?
6. Repeat steps 4 and 5 two dozen times.
7. TOTAL CODE FAILURE!! (and it looks like a mess) Completely buried and way behind schedule. Now I really am entertaining the idea of quitting. But I truly do not have that option. I am FORCED to keep pushing. 
8. Heavy, disappointed sigh of relief. Back up and running. But only for the grace and help of others. Feeling kind of meh about the whole thing.
9. MORE trouble. Out of time. Out of ideas. 
10. Problems solved. CODE WORKS WITHOUT BREAKING! I'm done. Happy tears. Almost nauseated with relief. The best feeling ever! *I love hyperbole*


Yeah this is what I call an exciting time. And I am looking forward to the next one.

So what I created is a surf themed cli code. The user is greeted with some surf lingo briefly explaining what to expect. The user is then prompted to enter a selection based on the provided options. Ask for a list of surf spots and get a list. Ask to exit the code and exit. Ask to see details of a beach and get those details. The elements of the list and it's details are data that is scraped from the internet. NO HARD CODING.  Super simple, right? Hahahaha.

I had LOADS of trouble. It took every bit of the 2 weeks allotted for this project to get it working and be able to sit down and write this. 

Two related technical troubles I had would be worth sharing here.

The fist one is with scraping the website *only* for the content I want. A lot of other content came with the scraping that had to be removed. Specifically four extra elements. 

```
def self.scraped_data
    html = open('https://wanderwisdom.com/travel-destinations/A-Locals-Guide-to-Orange-County-Beaches')
    doc = Nokogiri::HTML(html)
    container = doc.css("div.full.module.moduleText")
    container.each do |b|
      name = b.css("h2.subtitle").text.strip
      info = b.css("p").text.strip
       Surf.new(name, info)
    end
```


```
def list_beaches
    puts "\n\nCheck out this list!\nFrom catching 'the drop'\nto getting 'worked',\nthese are the sets to catch!"
    puts "(in no particular order)\n\n"
      @beaches = Surf.all     
      @beaches.each.with_index(1) do |beach, i|
        puts "#{i}. #{beach.name}"
      end
      puts "\nWanna 'get wet'?\nEnter a digit, dude.\nOr 'later' to bail.\n\n"
      surf_entry
  end 
```

Between these two methods my Surf.all array had an extra `name` and `info` element stored at index[0] and an empty `name` and `info` element in the final index positions.

So the solution was to add a .shift  and a .pop command in the list_beaches method.
Like so:

```
def list_beaches
    puts "\n\nCheck out this list!\nFrom catching 'the drop'\nto getting 'worked',\nthese are the sets to catch!"
    puts "(in no particular order)\n\n"
      @beaches = Surf.all
      @beaches.shift
      @beaches.pop      
      @beaches.each.with_index(1) do |beach, i|
        puts "#{i}. #{beach.name}"
      end
      puts "\nWanna 'get wet'?\nEnter a digit, dude.\nOr 'later' to bail.\n\n"
      surf_entry
  end 
```

This fixed that problem. However, it created another. I'm still not sure *exactly* how but, when the user would ask to see the list again, this method would run against the previously returned list, .shift and .pop and return the list with two missing elements. A list of 10 became a list of 8. If list was called again the list lost two more elements and became a list of 6.

What to do? 

I decided to set the results of `.shift` and `.pop` to a class variable `@@list`. Then I created another method called `re_list`  that performs the necessary routine whithout eating the list up. This is how the `list_beachs` method looks in the end with the `re_list` method.

```
def list_beaches
    puts "\n\nCheck out this list!\nFrom catching 'the drop'\nto getting 'worked',\nthese are the sets to catch!"
    puts "(in no particular order)\n\n"
      @beaches = Surf.all
      @beaches.shift
      @beaches.pop
      @@list = @beaches
      @beaches.each.with_index(1) do |beach, i|
        puts "#{i}. #{beach.name}"
      end
      puts "\nWanna 'get wet'?\nEnter a digit, dude.\nOr 'later' to bail.\n\n"
      surf_entry
  end 
```


```
def re_list
        @@list.each.with_index(1) do |beach, i|
        puts "#{i}. #{beach.name}"
     end
     puts "\nWanna 'get wet'?\nEnter a digit, dude.\nOr 'later' to bail.\n\n"
      surf_entry
   end 
```

So after the initial request anytime the list is called the `re_list` method kicks in and everything is gnarly, dude.

I hope this blog entertained and educated a little bit.

Code your imagination to life.




