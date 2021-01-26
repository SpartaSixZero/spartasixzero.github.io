---
layout: post
title: Closures in JavaScript
---

Functions are a key part of JavaScript. And closures are a key part of functions.

According to MDN:

_"A closure is the combination of a function bundled together (enclosed) with references to its surrounding state (the lexical environment). In other words, a closure gives you access to an outer function’s scope from an inner function. In JavaScript, closures are created every time a function is created, at function creation time."_

This concept is very powerful and allows creation of constructs like higher order functions.

## Higher-order function

According to JavaScript, The Definitive Guide:

_"A higher-order function is a function that operates on functions, taking one or more functions as arguments and returning a new function."_

Below is a simple example demonstrating the power of closure:

```javascript
function makeAdder(x) {
  return function (y) {
    return x + y;
  };
}

var add5 = makeAdder(5);
var add10 = makeAdder(10);

console.log(add5(2)); // 7
console.log(add10(2)); // 12
```

Let's dissect this code. First, we declared a function called makeAdder that takes a parameter `x`. This function returns an anonymous function which takes a parameter `y`. In the body of the anonymous function, we simply return `x + y`

When we first invoke `makeAdder(5)`, this is what happens:

1. We declare a variable named `x` and assigned it a value of `5` in the function scope of makeAdder.
1. We then create and return an anonymous function which takes in a parameter `y`. When this anonymous function is created, a closure is created and it has the lexical environment of the parent function.
1. Inside this anonymous function, `x` exists and has a value of `5` because we were able to find it in the lexical environment of the parent function.

When we then invoke `makeAdder(10)`, the same three steps happen but this time, `x` has a value of `10` inside the outer function scope.

When `add5(2)` gets invoked, our anonymous function that was returned from `makerAdder(5)` is executed. `2` gets passed in and becomes the value for `y`. Here, the anonymous function remembers that `x` was set to `5` so the returned value becomes `7`

When `add10(2)` gets invoked, `2` gets passed in and becomes the value for `y`. Here, `add10` has a different lexical environment than `add5` and it remembers the `x` was set to `10` so the returned value becomes `12`

## Summary

Closures are a fundamental concept to learn if you really want to master JavaScript. Closures enable higher-order functions which are used in Redux-Thunks, a popular middleware library for performing asynchronous actions. Don't worry if you don't know Redux or Redux-Thunks right now. I plan to write blog posts in the future on them.
