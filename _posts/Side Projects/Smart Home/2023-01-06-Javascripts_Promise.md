---
layout: post
title: "[JavaScript for Alexa Skill Development] Chapter 13: Asynchronous features"
date: 2023-01-06 15:40:00 +0800
categories: [Side Projects, Smart Home]
tags: [Alexa Developers, JavaScript, Asynchronization]
---



Good reference, this [<span style="color:#3ababa">Blog</span>](https://es6.ruanyifeng.com/#docs/promise).

Contents from this book [<span style="color:#3ababa">JavaScript: The Definitive Guide, 7th Edition</span>](https://www.oreilly.com/library/view/javascript-the-definitive/9781491952016/) channel.



<span style="color:#3ababa">Promises</span> are objects that represent the not-yet-available result of an asynchronous operation. The keywords <span style="color:#3ababa">async</span> and <span style="color:#3ababa">await</span> simplify asynchronous programming by allowing you to structure your Promise-based code as if it was synchronous. Finally, the <span style="color:#3ababa">for/await</span> loop allow you to work with streams of asynchronous events using simple loops that appear synchronous.


## 1. Asynchronous Progamming with Callbacks
A callback is a function that you write and then pass to some other function. That other function then invokes your function when some condition is met or some synchronous event occurs.
### Timers
```
setTimeout(checkForUpdates, 60000);
```

### Events
Client-side JavaScript programs are almost universally event driven: rather than running some kind of predetermined computation, they
typically wait for the user to do something and then respond to the user’s actions.

```
let okay = document.querySelector('#confirmUpdateDialogbutton.okay');

// Now register a callback function to be invoked when the user clicks on that button.
okay.addEventListener('click', applyUpdate);
```

### Network Events
Another common source of asynchrony in JavaScript programming is network requests.

<img src="/assets/img/JavaScript/network_callback_example_0.PNG" alt="network callback example" width="400"/> <br />

getCurrentVersionNumber() makes an asynchronous request, it cannot synchronously return the value that the caller is interested in. Instead, the caller passes a callback function, which is invoked when the result is ready or when an error occurs.

### Callbacks and Events in Node
Node.js asynchronous example: reading the contents of a file. fs.readFile() takes a two-parameter callback as its last argument.

<img src="/assets/img/JavaScript/network_callback_example_1.PNG" alt="network callback example" width="400"/> <br />


Example to request for the contents of a URL in Node. Node uses <span style="color:#3ababa">on()</span> method to register event listeners instead of <span style="color:#3ababa">addEventListener()</span>.

<img src="/assets/img/JavaScript/network_callback_example_2.PNG" alt="network callback example" width="400"/> 
<img src="/assets/img/JavaScript/network_callback_example_3.PNG" alt="network callback example" width="400"/> <br />


## 2. Promises
所谓Promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。

A Promise is an object that represents the result of an asynchronous computation. That result may or may not be ready yet, and the Promise API is intentionally vague about this: there is no way to synchronously get the value of a Promise; you can only ask the Promise to call a callback function when the value is ready. <br />

To make it Promise-based, omit the callback argument, and instead return a Promise object. The caller can then register one or more callbacks on this Promise object, and they will be invoked when the asynchronous computation is done. <br />

callbacks drawbacks:
- nested callback, not clean
- difficult error handlings

### Using Promises
The function you pass to <span style="color:#3ababa">then()</span> is invoked asynchronously, even if the asynchronous computation is already complete when you call <span style="color:#3ababa">then()</span>.

```
getJSON(url).then(jsonData => {
    // This is a callback function that will be asynchronously
    // invoked with the parsed JSON value when it becomes available.
});
```

<img src="/assets/img/JavaScript/promise_example_0.PNG" alt="promise example" width="400"/> 

*Handling Errors with Promises* <br />
Callback vs. Promise <br />
When something goes wrong in a synchronous computation, it throws an exception that propagates up the call stack until there is a catch
clause to handle it. When an asynchronous computation runs, its caller is no longer on the stack, so if something goes wrong, it is simply not possible to throw an exception back to the caller.

Asynchronous operations can fail in a number of ways, and robust code has to be written to handle the errors that will inevitably occur. 
When a Promise-based asynchronous computation completes normally, it passes its result to the function that is the first argument to <span style="color:#3ababa">then()</span>. For Promises, we can do this by passing a second function to the <span style="color:#3ababa">then()</span> method for error handling.

```
getJSON("/api/user/profile").then(displayUserProfile, handleProfileError);
```

<img src="/assets/img/JavaScript/promise_example_1.PNG" alt="promise example" width="400"/> 

*Promise Terminology*
we say that the Promise has been <span style="color:#3ababa">fulfilled</span> if and when the first callback is called; we say that the Promise has been <span style="color:#3ababa">rejected</span> if and when the second callback is called; if a Promise is neither fulfilled nor rejected, then it is <span style="color:#3ababa">pending</span>. And once a promise is fulfilled or rejected, we say that it is <span style="color:#3ababa">settled</span>. 


### Chaining Promises
Promise provides a natural way to express a sequence of asynchronous operations as a linear chain of <span style="color:#3ababa">then()</span> method invocations, without having to nest each operation within the callback of the previous one. <br />


Example to illustrate how a chain of Promises can make it easy to express a sequence of asynchronous operations

<img src="/assets/img/JavaScript/promise_example_2.PNG" alt="promise example" width="400"/> 


Method chain example:
```
fetch().then().then()
```

We know that the <span style="color:#3ababa">fetch()</span> function returns a Promise object, and we can see that the first <span style="color:#3ababa">.then()</span> in this chain invokes a method on that returned Promise object. But there is a second <span style="color:#3ababa">.then()</span> in the chain, which means that the first invocation of the then() method must itself return a Promise. 

```
fetch(theURL)           // task 1; returns promise 1
    .then(callback1)    // task 2; returns promise 2
    .then(callback2);   // task 3; returns promise 3
```

But just because task 2 begins when c1 is invoked, it does not mean that task 2 must end when c1 returns. Promises are about managing asynchronous tasks, after all, and if task 2 is asynchronous (which it is, in this case), then that task will not be complete by the time the callback returns.

<img src="/assets/img/JavaScript/promise_example_4.PNG" alt="promise example" width="400"/> 

About <span style="color:#3ababa">resolved</span>:

<img src="/assets/img/JavaScript/promise_example_3.PNG" alt="promise example" width="400"/> 


### Promises in Parallel
Sometimes we want to execute a number of asynchronous operations on parallel.

### Making Promises
This section shows how you can create your own Promise-based APIs.

#### Promises Based On Other Promises
Given a Promise, you can always create a new one by calling <span style="color:#3ababa">.then()</span>.
```
//Promise returned by getJSON() resolves to the Promise returned by response.json(). 
function getJSON(url) {
    return fetch(url).then(response => response.json());
}
```

#### Promises Based On Synchronous Values
Sometimes, you may need to implement an existing Promise-based API and return a Promise from a function, even though the computation to be performed does not actually require any asynchronous operations. <br />

<span style="color:#3ababa">Promise.resolve()</span> and <span style="color:#3ababa">Promise.reject()</span>

Promise.resolve() takes a value as its single argument and returns a Promise that will immediately(but asynchronously) be fulfilled to that value. 

*To be clear*: <br />
The Promises returned by these static methods are not already fulfilled or rejected when they are returned, but they will fulfill or reject immediately after the current synchronous chunk of code has finished running. Typically, this happens within a few milliseconds unless there are many pending asynchronous tasks waiting to run. <br />

It is fairly common to have synchronous special cases within an asynchronous function, and you can handle these special cases with Promise.resolve() and Promise.reject(). 

#### Promises from Scratch
what about writing a Promise-returning function when you can't use another Promise-returning function as the starting point? In that case, you use the <span style="color:#3ababa">Promise()</span> constructor to create a new Promise object that you have complete control over. 

Alexa skill example:

<img src="/assets/img/JavaScript/promise_example_5.PNG" alt="promise example" width="500"/> 

Pay attension:

<img src="/assets/img/JavaScript/promise_example_8.PNG" alt="promise example" width="400"/> 

The constructor synchronously calls your function with function arguments for the <span style="color:#3ababa">resolve</span> and <span style="color:#3ababa">reject</span> parameters. After calling your function, the <span style="color:#3ababa">Promise()</span> constructors returns the newly created Promise. That returned Promise is under the control of the function you passed to the contructor.

That function should perform some asynchronous operation and then call the <span style="color:#3ababa">resolve</span> function to resolve or fulfill the returned Promise or call the <span style="color:#3ababa">reject</span> function to reject the returned Promise. 

```
function wait(duration) {
    // Create and return a new Promise
    return new Promise((resolve, reject) => {   // These control the Promise
                                                // If the argument is invalid, reject the Promise
        if (duration < 0) {
            reject(new Error("Time travel not yet implemented"));
        }

        // Otherwise, wait asynchronously and then resolve the Promise.
        // setTimeout will invoke resolve() with no arguments, which means
        // that the Promise will fulfill with the undefined value.
        setTimeout(resolve, duration);
    });
}
```

This getJSON() example illustrates how we can implement Promise-based APIs on top of other styles of asynchronous programming.

<img src="/assets/img/JavaScript/promise_example_6.PNG" alt="promise example" width="500"/> 
<img src="/assets/img/JavaScript/promise_example_7.PNG" alt="promise example" width="500"/> 

### Promises in Sequence


## 3. async and await
These new keywords dramatically simplify the use of Promises and allow us to write Promise-based, asynchronous code that look like synchronous code that blocks while waiting for network response or other asynchronous events.

### await Expressions
The <span style="color:#3ababa">await</span> keyword takes a Promise and turns it back into a return value or a thrown exception. Given a Promise object p, the <span style="color:#3ababa">await p</span> waits until p settels. If p fulfills, then the value of <span style="color:#3ababa">await p</span> is the fulfillment value of p. If p is rejected, then the <span style="color:#3ababa">await p</span> throws the rejection value of p. <br />

It literally does nothing until the specified Promise settles. The code remains asynchronous, and the await simply disguises this fact. This means that any code that uses await is itself asynchronous.

```
let response = await fetch("/api/user/profile");
let profile = await response.json();
```

### async Functions
Because any code that uses <span style="color:#3ababa">await</span> is asynchronous, there is one critical rule: you can only use the <span style="color:#3ababa">await</span> keyword within functions that have been declared with the <span style="color:#3ababa">async</span> keyword. <br />

Declaring a function <span style="color:#3ababa">async</span> means that the return value of the function will be a <span style="color:#3ababa">Promise</span> even if no Promise-related code appears in the body of the function.

```
async function getHighScore() {
    let response = await fetch("/api/user/profile");
    let profile = await response.json();
    return profile.highScore;
}
```

The getHighScore() function is declared async, so it returns a Promise. And because it returns a Promise, we can use the await keyword with it:
```
displayHighScore(await getHighScore());
```

### Awaiting Multiple Promises
Given this getJSON() function:
```
async function getJSON(url) {
    let response = await fetch(url);
    let body = await response.json();
    return body;
}
```

Now suppose we want to fetch two JSON values with this function:
```
let value1 = await getJSON(url1);
let value2 = await getJSON(url2);
```

*Issue*: <br />
It is unnecessarily sequential: the fetch of the second URL will not begin until the first fetch is complete. If the second URL does not depend on the value obtained from the first URL, then we should probably try to fetch the two values at the same time. <br />

*Solution*: <br />
In order to await a set of concurrently executing <span style="color:#3ababa">async</span> functions, we use <span style="color:#3ababa">Promise.all()</span> just as we would if working with Promises directly:
```
let [value1, value2] = await Promise.all([getJSON(url1), getJSON(url2)]);
```

### Implementation details
To think about what is going on under the hood. Suppose you write an <span style="color:#3ababa">async</span> like this:
```
async function f(x) { /* body */ }
```
You can think of this as a Promise-returning function wrapped around the body of your original function:
```
function f(x) {
    return new Promise(function(resolve, reject) {
        try {
            resolve((function(x) { /* body */ })(x));
        }
        catch(e) {
            reject(e);
        }
    });
}
```

## 4. Asynchronous iteration
Promises are useful for single-shot asynchronous computations but were not suitable for use with resources of repetitive asynchronous events, such as <span style="color:#3ababa">setInterval()</span>, the "click" event in a web browser, or the "data" event on a Node stream. Because single Promises do not work for sequences of asynchronous events, we also cannot use regular <span style="color:#3ababa">async</span> functions and the <span style="color:#3ababa">await</span> statements for these things. 

### The for/await Loop








