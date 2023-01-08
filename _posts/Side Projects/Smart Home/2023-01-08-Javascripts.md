---
layout: post
title: "[JavaScript for Alexa Skill Development] Objects"
date: 2023-01-08 15:40:00 +0800
categories: [Side Projects, Smart Home]
tags: [Alexa Developers, JavaScript, Asynchronization]
---

## 1. Introduction to Objects
An object is a composite value: it aggregates multiple value(primitive values or other objects) and allows you to store and retrieve those values by name. The methods of an object are typically inherited properties, and this "prototypal inheritance" is a key feature of JavaScript.

Objects are mutable and manipulated by reference rather than by value. If the variable x refers to an object and the code <span style="color:#3ababa">let y = x;</span> is executed, the variable y holds a reference to the same object, not a copy of that object. Any modifications to the object through variable y are also visible through the variable x.


## 2. Creating Objects
Objects can be created with <span style="color:#3ababa">object literals</span>, with the <span style="color:#3ababa">new keyword</span>, and with the <span style="color:#3ababa">Object.create()</span> function.

### Object Literals
The easiest way to create an object is to include an object literal in your JavaScript.

<img src="/assets/img/JavaScript/object_0.PNG" alt="promise example" width="500"/> 

### Creating Objects with new
The new keyword must be followed by a function invocation. A function used in this way is called a constructor and serves to initialize a newly created object.

```
let o = new Object(); // Create an empty object: same as {}.
let a = new Array();  // Create an empty array: same as [].
let d = new Date();   // Create a Date object representing the current time
let r = new Map();    // Create a Map object for key/value mapping
```


