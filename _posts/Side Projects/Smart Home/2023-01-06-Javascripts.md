---
layout: post
title: "[JavaScript for Alexa Skill Development] Asynchronous features"
date: 2023-01-06 15:40:00 +0800
categories: [Side Projects, Smart Home]
tags: [Alexa Developers, JavaScript, Asynchronization]
---

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





