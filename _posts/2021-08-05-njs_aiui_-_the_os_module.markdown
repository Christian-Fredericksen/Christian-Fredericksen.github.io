---
layout: post
title:      "NJS:AIUI - THE OS MODULE "
date:       2021-08-06 00:41:03 +0000
permalink:  njs_aiui_-_the_os_module
---

* THESE NEXT FEW POSTS ARE TO GIVE A GENERAL UNDERSTANDING OF BUILT IN MODULES, THEIR FUNCTIONS, AND
A HANDFUL OF OPTIONS TO INVOKE THEM.

The OS Module is a built in module that provides many properties and methods for interacting with the operating system and the server. The general set up to use these built in modules looks something like this:
```
const os = require('os')  // remember our global variable require() from the last post? It's a function to use modules.
                          // set a var equal to "require('module_name')". Because it's built in there is no need
                          // to use the './'directory notation.
                          // this gives us use of 'os.builtInFunctions()' For example:
```
We can get information about the current user:
```
const user = os.userInfo() // <--- var assignment is one way to invoke methods of built in modules.
console.log(user)
```
If we run the app in the command line we will be returned a user object{} with attributes and values.
It should look something like this:
```
[16:05:35]  Node.js practice (main)
// ♥  node 8-os-module
{
  uid: 1000,
  gid: 1000,
  username: 'dragon_on_a_unicycle',
  homedir: '/home/dragon_on_a_unicycle',
  shell: '/bin/bash'
}
```
Let's invoke another function through a console log.
```
console.log(`The System Uptime is ${os.uptime()} seconds.`)  // Let's use template string here and interpolate 
                                                             // our invocation. Don't forget the backticks `.
```
Running the app now would give us the same user object as before as well as a message telling you how long the system has been running in seconds. Like so:
```
[16:56:22]  Node.js practice (main)
// ♥  node 8-os-module
{
  uid: 1000,
  gid: 1000,
  username: 'dragon_on_a_unicycle',     
  homedir: '/home/dragon_on_a_unicycle',
  shell: '/bin/bash'
}
The System Uptime is 39295 seconds.
```
Let's try invoking a bunch of methods in an object:
```
const currentOS = {         // <---here we set a var and create an {} that contains a few
    name: os.type(),        // <---( properties: ) set to ( module.function() )
    release: os.release(),  // <---( properties: ) set to ( module.function() )
    totalMem: os.totalmem(),// <---( properties: ) set to ( module.function() )
    freeMem: os.freemem(),  // <---( properties: ) set to ( module.function() )
}
console.log(currentOS)
```
Run the app in the command line and we get all the previous data and the object we just created.
```
[17:14:15]  Node.js practice (main)
// ♥  node 8-os-module
{
  uid: 1000,
  gid: 1000,
  username: 'dragon_on_a_unicycle',     
  homedir: '/home/dragon_on_a_unicycle',
  shell: '/bin/bash'
}
The System Uptime is 40366 seconds.
{
  name: 'Linux',
  release: '4.4.0-19041-Microsoft',
  totalMem: 8416038912,
  freeMem: 3076472832
}
```
Here we have explored, just a little, the OS Module that comes built into node.js. This is just
scratching the surface of what we can do with this module to interact with the operating system.
For a complete list of OS Module methods visit the offficial doc page at (https://nodejs.org/api/os.html)

Let's move on and see how node.js allows us to interact with the file system. Something we can't
do with JavaScript in the browser. 

Next up the Path Module.

