---
layout: post
title:      "The Imposter Syndrome"
date:       2020-03-03 15:25:15 -0500
permalink:  the_imposter_syndrome
---


So this is real.  I feel like a fraud. I tell people I am a software engineering student and it sounds like bullshit. I may as well tell folks I'm an actor.

When you say things like this, people naturally respond with something that reduces down to: "Prove it!" And to a point I can.

When I speak to a non-tech individual I'm okay. They don't have any idea what I'm saying. But when trying to relate to someone in the field, I sense an unsavory energy coming of them.  I explain where I am in my journey and Object Oriented Ruby just rolls of my tongue, but, umm, uhh, the next section, uhhh, SQL! That's it, SQLite! The fumbling for the words to use, I feel them pulling away. I hear their thoughts, "Bah! You're no programmer." They say things that sound slightly condescending, like when you encourage a little kids crayon art. Sometimes, often, this feel like a half inch crack in the sidewalk tripping me, sending me to my knees.

A perfect example would be my helper method current_customer that is set in the ApplicationController.
```
def current_customer
      if session[:customer_id]
        current_customer = Customer.find(session[:customer_id])
      end
    end
```
So my current_customer varriable is set to the session[:customer_id]. And because all lower level controllers inheret the from ApplicationController I assumed I was all set to use it.

Not true.

In my PizzasController, when a user creates a new pizza, this is the method invoked:
```
 def create 
        @pizza = Pizza.new(pizza_params)
        @pizza.customer_id = session[:customer_id]
        if @pizza.valid?
            @pizza.save  
        redirect_to customer_pizza_path(current_customer, @pizza)
        else
            flash[:errors] = @pizza.errors.full_messages
            redirect_to new_customer_pizza_path(current_customer.id)   
        end
    end
```
It is only AFTER the object is validated and saved with an actual customer_id
that my helper method is usable. And of course that makes total sense now. But when things are breaking ad the head is mush...
Even talking about it here makes me feel fraudulent. Because it is such a basic principle.

But! I have my cohort. I have my cohort lead and everyone else in the program. Most of us students feel the same way. A lot of those more experienced after years still feel that way. And I'm realizing that this feeling is probably 90% or more self inflicted. We all help each other learn and rise higher and higher each day. And when I have a real connection with a tech industry insider, they explain to me that the real world work environment is much like our cohort environment. Collaborative. Everyone working together toward the same goal. This is what keeps me going and keeps me excited. I have never been in a work space like this. They all have been very competitve in an unhealthy way and full of sabotage. Maybe this is where this bogus feeling comes from? The need to make sure I'm on point enough that nobody can touch me, take my job, fire me. Working in fear of the aforementioned. Always gotta be perfect so no one questions me. Why would this be different? Someone is going to pay me a lot of money to do a job, so I BETTER be able to produce at a consistent level 8 out of 10 straight out of the gate.

But I know and have learned that when we start we will not be thrown to the wolves. We will not be left on our own. That precious few work solo. Everyone is part of a team that helps one another. Every team helps the next team. We experience problems together, and together we will code solutions to those problems.

I hope this post reaches someone feaking out like I do. I hope this settles your nerves a bit.

Code your imagination to life
