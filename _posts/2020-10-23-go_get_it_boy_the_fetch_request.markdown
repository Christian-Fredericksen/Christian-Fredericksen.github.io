---
layout: post
title:      "Go get it, boy! The Fetch Request"
date:       2020-10-23 21:34:53 +0000
permalink:  go_get_it_boy_the_fetch_request
---


Something I have learned about recently is the JavaScrip fetch() method. This is really a demystifying method. As a user you may wonder what happens when you log into your Facebook account and you add a post, or like a comment or visit your timeline. How does all that information get displayed? Where does it come from? How does it happen so quickly and without refreshing the page. The JS fetch() method is one heavy lifting work horse of a method. 
Let's see if we can dissect a simple fetch() method like submitting new user data and creating and logging them in.

And I am probably going to be super remedial about it. Lol.

```
    fetch(USERS_URL, {
        method: "POST",
        headers: {
            "Content-Type": "application/json",
            Accept: "application/json"
        },
        body: JSON.stringify({
            user: {
                email: signupInputs[0].value,
                password: signupInputs[1].value
            }
        })
    })
    .then(res => res.json())
    .then(function(object){
        if (object.message) {
            alert(object.message)
        }
        else {
        loggedInUser(object)
        }
    }
    )

```

So this first part
```
 fetch(USERS_URL, {
        method: "POST",
```

We are calling the `fetch()` method into play.  We pass in a URL or a  function that represents a URL in this case we have `fetch(USERS_URL)`. Then because fetch defaults as a GET request we need to tell it that this is a POST request. We are not asking for data, we are sending data to the server, we are POSTing data. So that brings us to:

```
 fetch(USERS_URL, {
        method: "POST",
```

Next we need to tell the server what it is receiving. So in a headers{ } We explain that the "Content..." of this POST request is a JSON application and that we will "Accept" responses from the server the same way.

```
headers: {
            "Content-Type": "application/json",
            Accept: "application/json"
        },
```

Now the meat of it. The "body" We take that JSON object and we "stringify" it! We have to  turn into an HTML string so the server know what it is looking at. `JSON.stringify` takes the "user" object that elements of "email" and "password" and each of those have "input" "values" from a signup form and turns it all into a long string and sends it off to the server.

```
 body: JSON.stringify({
            user: {
                email: signupInputs[0].value,
                password: signupInputs[1].value
            }
        })
```

".then" we wait for a "*res*"-ponse from the server where the process is basically reversed. The HTML string that the server sends back is converted into a JSON object with this line` .then(res => res.json())` 

".then" we pass that object into an annonymous function and do a little "work" to it. "If" something is wrong with the object and `(object.message)` is triggered, the alert() function will run and return the value of `(object.message)`.

otherwise move on to the ` loggedInUser()` function *described elsewhere* and pass in this object we just received.

The  ` loggedInUser()` function then renders that objects data into what we see on the screen.

```
 .then(res => res.json())
    .then(function(object){
        if (object.message) {
            alert(object.message)
        }
        else {
        loggedInUser(object)
        }
    }
    )		

```

So, putting it all together, a new user will fill out a couple fields of data, click a submit button, the user's side then executes the above explained, communicating with the server. The server answers and if all is well the anwer is then displayed for the user. All without refreshing the screen. All in an instant.

I really like fetch().

Code your imagination into reality.
