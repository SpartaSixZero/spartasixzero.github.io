---
layout: post
title: Asynchronous JavaScript With setTimeout
---

I remembered when I first came across [setTimeout()](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setTimeout) I didn't think much about it. At the time, my naive mind was like "oh, so it does an action after a certain time, that's not so hard". For a long time, I kept seeing `setTimeout()` used in examples where asynchronous JavaScript was involved, whether it's old school asynchronous callbacks or [Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise). I never really thought deeply about why they were used until I came across this eye opening tech talk on youtube: [What the heck is the event loop anyway](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) by Philip Roberts.

JavaScript Event Loop is a key concept to understand if you want to learn asynchronous JavaScript. To help explain the event loop, I'll use setTimeout() in a simple example:

```javascript
function doThisLater() {
  setTimeout(() => {
    console.log("This goes last");
  }, 0);
}

console.log("This goes first");
doThisLater();
console.log("This goes second");

// Output:
// This goes first
// This goes second
// This goes last
```

If you want to run this for yourself, you can [go to the code pen here](https://codepen.io/sunmark14/pen/NWRMXWj)

You might wonder why the message outputted in the function doThisLater() is last even though it is invoked before `console.log("This goes second")`

The MDN page on setTimeout() says this about the delay time, which for our example was 0: (The delay time is the 2nd parameter to setTimeout() for our example, the 1st parameter being the callback function.)

_'The time, in milliseconds (thousandths of a second), the timer should wait before the specified function or code is executed. If this parameter is omitted, a value of 0 is used, meaning execute "immediately", or more accurately, the next event cycle.'_ - MDN

The last sentence gives us a hint, "or more accurately, the next event cycle".

Down the rabbit hole we go! To understand more, we have to first know what this event cycle is.

Below is the picture that I took from MDN's [page on JavaScript Event Loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop#Event_loop)

![JavaScript Event Loop](https://media.prod.mdn.mozit.cloud/attachments/2020/02/23/17124/7cbd04169bbb5be13ede088ff0833882/The_Javascript_Runtime_Environment_Example.svg)

We'll just focus on Stack and Queue and ignore Heap for now. You can think of the Stack as a stack of execution contexts, you can also think of this as a call stack.

For our example, starting from top down, the interpreter first encounters our function declaration, so it declares a variable named doThisLater in our global scope and assigns the function to that variable. After this is done, our stack is empty.

Next, it encounters the line `console.log("This goes first");` and adds to to the stack.

Since it can just execute this, it proceeds to output "This goes first" on the console. Again, our call stack is now empty.

After that, we now are at place where we invoke `doThisLater()`. The interpreter then goes to find the reference to doThisLater and invokes it. It sees the setTimeout() function, immediately, what happens next is that the callback function inside the setTimeout() is now placed inside a message or event queue. The reason is that JavaScript doesn't want asynchronous actions to block the main stack, otherwise, users will experience slow or even unresponsive UI and will get frustated.

Items inside this event queue will basically wait until the current stack is empty before it is placed back into the stack.

Now, with that item in our queue, we move on to the next line of code which is `console.log("This goes second");`. Since the interpreter can run that right away, it does so and we see "This goes second" on our console.

Finally, because our stack is now empty, we take the item from the event queue and places it in the stack. Since the delay is 0, we simply execute the callback function which then outputs "This goes last" onto the console. Now we're done.

Asynchronous Javascript is often used for time consuming actions for example, making a network call using Fetch API to grab a resource. Because of the possibility of being time consuming, we don't want this to block the current stack and cause slowness or even unresponsiveness of the UI. setTimeout() is a great way to demonstrate how actions that are potentially time consuming is placed into an event queue so that they don't block the main stack.
