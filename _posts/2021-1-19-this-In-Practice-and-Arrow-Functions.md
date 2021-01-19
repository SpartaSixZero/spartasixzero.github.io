---
layout: post
title: this in Practice and Arrow Functions
---

Armed with the 4 binding rules for `this` from the last blog post, let's go through an example together and apply the rules to this example. This example is taken from the book, [JavaScript The Definitive Guide by David Flanagan 7th edition](https://www.amazon.com/JavaScript-Definitive-Most-Used-Programming-Language/dp/1491952024/ref=pd_sbs_1?pd_rd_w=bHWUv&pf_rd_p=3ec6a47e-bf65-49f8-80f7-0d7c7c7ce2ca&pf_rd_r=78F07B4XFYD3G8J58249&pd_rd_r=b1c808af-d1de-4c5d-a800-c7ae0a2ff58e&pd_rd_wg=rEe9k&pd_rd_i=1491952024&psc=1)

```javascript
let o = {
  m: function () {
    let self = this;
    console.log(this === o); // true
    f();

    function f() {
      console.log(this === self); // false
    }
  },
};

o.m();
```

In this example, we declared a variable called `o` and assigned it a JavaScript object using object literal notation. This JavaScript object then has 1 property called `m`. `m` is set to a function expression.

A very common mistake is assuming that inside function `f`, `this` is equal to `self`. Here, we created a variable called `self` and set it equal to `this` in order to preserve the value of `this`. Remember, whenever a function (that is not an Arrow function) gets invoked, it's `this` is determined at the call-site. The call-site is where that function is invoked. The 4 bindings for `this` does not apply to Arrow functions, which I will talk about later on in this post.

Now, let's run through the 4 bindings to figure out what `this` is in the code above. We need to ask the questions in this order for the invocation of `f()`

1. Is the function invoked with `new`? No
1. Is the function invoked explicitly via `call()`, `apply()`, or `bind()`? No
1. Is the function invoked with a reference object? No, `f()` is invoked by itself with no referencing or owning object.
1. Since we get "No" for all three rules above, we end up with the default binding. Remember, that means `this` gets set to global object or `window` on browsers or in strict mode, `undefined`

## Arrow Functions

Most of us have seen Arrow functions after it was introduced in ES6. To put it simply, it's a short hand syntax for writing functions.

Below is a simple function expression without using Arrow functions.

```javascript
const sum = function (a, b) {
  return a + b;
};
```

Now with Arrow functions.

```javascript
const sum = (a, b) => a + b;
```

A key thing to remember is that Arrow functions do not participate in the 4 binding rules for `this` when invoked.

```javascript
let o = {
  m: function () {
    let self = this;
    console.log(this === o); // true

    const f = () => {
      console.log(this === self); // true
    };

    f();
  },
};

o.m();
```

According to JavaScript, The Definitive Guide:

_"Arrow functions differ from functions defined in other ways in one critical way: they inherit the value of the `this` keyword from the environment in which they are defined rather than defining their own invocation context as functions defined in other ways do."_

If we look at the code above, the Arrow function was defined in the scope of the function what we set to `m`. But what is the `this` inside `m`? If we remember are 4 binding rules, we know that we are using `implicit binding` because `m()` was invoked with a referencing or containing object, in our case, `o`. That is why `o` is used as `this` inside the function body for `m`.

Since Arrow function inherits the value of `this` from its parent scope. `this` inside the function body for `f` is also now `o`.

## Summary

In summary, whenever we talk about `this` for JavaScript functions, there are really only 2 big points to remember:

1. Is it an Arrow function? If so, then you can get `this` from the parent scope where the arrow function was defined.
1. If it is not an Arrow function, then simply use the [4 binding rules for `this`](https://spartasixzero.github.io/this-in-JavaScript/) (try to memorize them) in the correct order of precedence.
