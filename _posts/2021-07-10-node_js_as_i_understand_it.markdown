---
layout: post
title:      "Node.js:  As I Understand It"
date:       2021-07-10 23:14:02 +0000
permalink:  node_js_as_i_understand_it
---

I recently decided to teach myself Node.js using this YouTube full course tutorial from freeCodeCamp.org 
[]https://youtu.be/Oe421EPjeBE 

This is a really good course tutorial covering the fundaments of Node.js and assumes the student has at least some fundamental coding experience going in. 

This will be an ongoing blog as I advance through learning this back end language. Im certain these posts will become more comprehensive as we get deeper. So follow along and let's learn Node.js...as I understand it.

In our introduction we are reminded of some basic JavaScript syntax...

```
const amount = 3;

if(amount < 10){
    console.log("The amount is less than 10")
}else if(amount == 10){
    console.log("The amount is 10")
}else {
    console.log("The amount is more than 10")
}

console.log("This is my first app")
```

We then discuss *some* of the global variables...

```
//  __dirname  - path to current directory
//  __filename - file name
//  require    - function to use modules (CommonJS)
//  module     - info about current module (file)
//  process    - info about environment where the program is being executed

```

We invoke this console.log
```
console.log(global)

```

And are returned some built in functions of Node.js:
```
<ref *1> Object [global] {
  global: [Circular *1],
  clearInterval: [Function: clearInterval],
  clearTimeout: [Function: clearTimeout],
  setInterval: [Function: setInterval],
  setTimeout: [Function: setTimeout] {
    [Symbol(nodejs.util.promisify.custom)]: [Function (anonymous)]
  },
  queueMicrotask: [Function: queueMicrotask],
  clearImmediate: [Function: clearImmediate],
  setImmediate: [Function: setImmediate] {
    [Symbol(nodejs.util.promisify.custom)]: [Function (anonymous)]
  }
}
```
Let's write a little setInterval function code:
```
setInterval(() => {
console.log('hello world')
}, 1000)
```
We should see 'hello world' print to the console once every second until we exit the app by holding `ctrl + c`

Wow! This all seems kind of familiar. Hmmm 🤔

Next up, we dive into Modules.

Node is written in CommonJS so a lot of the syntax will be familiar. In Node.js all files are, by default, modules. And Modules are 'encapsulated code' and should share only a minimum of content.  Modules are seperated code files that keep our related data organized and isolated from corruption.

Let's say our app.js file looks like:
```
const owner = 'Cody McCoderson'
const pets = ['cat', 'dog']
const cat = 'Fluffy'
const dog = 'Fido'

const animalIntro = (pet, name) => {
    console.log(`My ${pet}'s name is ${name}!`)
}

animalIntro(pets[0], cat)
animalIntro(pets[1], dog)
```
This is just the beginning of our app and it's already getting convoluted. We have (3) vars, a function and (2) function calls. Plus all of our variables are subject to malicious change. Lets start seperating this data into their own files/modules. This way we can follow the rules of separating and isolating our data and be selective about what data our app has access to.

Create a file/module called `petsData.js` and copy all this data into it:
```
const owner = 'Cody McCoderson'
const pets = ['cat', 'dog']
const cat = 'Fluffy'
const dog = 'Fido'
``` 
And do the same thing with the function:
```
const animalIntro = (pet, name) => {
    console.log(`My ${pet}'s name is ${name}!`)
}
```
into a module called `functions.js`

In the interest of expediting this blog let's make the `petsData.js` and `functions.js` look like this:
```
const owner = 'Cody McCoderson'
const pets = ['cat', 'dog']
const cat = 'Fluffy'
const dog = 'Fido'

module.exports = {pets, cat, dog}
```

```
const animalIntro = (pet, name) => {
    console.log(`My ${pet}'s name is ${name}!`)
}

module.exports = animalIntro
```
As we can see our node.js `module`s have an `exports` property and that property is an object that can be filled with all sorts of data. In one case we have filled it with the `pets` array, the `cat` var and the `dog` var. Notice we left out the `owner` var. That's to demonstrate how we can be selective about the data we wish to share.

In the other case we have just the one function. Notice that we do not need to use { } if we only have one thing to export.

In node.js we contain our data in modules to keep it organized and safe from corruption. To use that data we place it in the exports{} obj and then we `require` it in our app.  

Let's have our app look like this:
```
const pets = require('./petsData')
const animalIntro = require('./functions')

animalIntro(pets[0], cat)
animalIntro(pets[1], dog)

console.log(pets)

```

So what's happening in the first two lines? We have declared a `const var` and set it equal `=` to the golabal function `require()` and passed in the path`'./petsData'` of where the shared data lives. At this point if you were to run `node app.js` you would see:

```
My cat's name is Fluffy!
My dog's name is Fido!
{ pets: [ 'cat', 'dog' ], cat: 'Fluffy', dog: 'Fido' }
```

Proof that we were able to share, receive and use the data from one module in another while protecting it from being compromised.

And there you have it. An introduction to

**Node.js:  As I understand it**


















