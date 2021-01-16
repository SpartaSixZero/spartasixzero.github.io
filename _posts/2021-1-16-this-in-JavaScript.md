---
layout: post
title: this in JavaScript
---

`this` in JavaScript has confused many developers who started learning JavaScript. The most common reason is that it doesn't behave the way you would expect if you come from an object-oriented language like Java or C#.

I had some knowledge of `this` before I started reading the book series [You Don't Know JS by Kyle Simpson](https://github.com/getify/You-Dont-Know-JS). After reading it, I would highly recommend anyone who wants a deeper understanding of JavaScript, specially the `this` keyword.

## What is `this?`

Kyle states inside his book, You don't know JS:

_"When a function is invoked, an activation record, otherwise known as an execution context, is created. This record contains information about where the function was called from (the call-stack), how the function was invoked, what parameters were passed, etc. One of the properties of this record is the `this` reference which will be used for the duration of the function's execution."_

The key takeaway from the above quote is that `this` is determined when the function is invoked, not when it is declared.

In order to help us understand `this`, we need to first understand the concept of a call-site. _A call-site is the location where a function is called_. We need to inspect the call-site to help figure out what `this` is.

He then goes to mention 4 different types of possible binding for `this`.

## Binding Types

### Default Binding

This is the rule that serves as a default catch-all when none of the other rules apply. Below is an example:

```javascript
function foo() {
  console.log(this.a);
}

var a = 2;

foo(); // 2
```

Here, the call-site is simply the global scope, and we are just invoking foo as a function. In this case, `this` refers to the global object.
Note: When strict mode is turned on, `this` is instead set to `undefined`.

### Implicit Binding

For implicit binding, we ask the question: "does the call-site have a context object. A context object is also referred to as a owning or containing object. Below is an example:

```javascript
function foo() {
  console.log(this.a);
}

var obj = {
  a: 2,
  foo: foo,
};

obj.foo(); // 2
```

In this example, we can see that `obj` has a property called `foo`. This `foo` is then set to reference the function `foo()`. In this case, when `foo()` was invoked, we do have a context object `obj`. So `obj` will be used as the value for `this`. Since `obj` has a prop named `a`, `foo()` then is able to log the value of `a` into the console to show 2.

### Explicit Binding

Every JavaScript function has access to `call()` and `apply()` methods. They are similar in functionality. For both of them, the first parameter is an object to use for `this`. This object will then be used when the function is invoked. Below is an example:

```javascript
function foo() {
  console.log(this.a);
}

var obj = {
  a: 2,
};

foo.call(obj); // 2
```

In this example, foo is invoked using the `call()` method. What is different is that we pass in the object `obj` as the first parameter to `call()`. Then inside `foo()`, `this` is now referring to `obj`.
That's why we are able to see an output of 2 in the console.

#### Hard Binding

If you used class based components with React, you'll often bind yourself using the `bind()` method on functions. This is because in React's class based components, functions do not automatically get `this` to be the class instance.

Example:

```jsx
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0,
    };
    // This next line is necessary so that onButtonClick
    // method can use the correct "this". Otherwise,
    // "this" would be undefined
    this.onButtonClick = this.onButtonClick.bind(this);
  }

  onButtonClick() {
    this.setState({ count: this.state.count + 1 });
  }

  render() {
    return (
      <div>
        <button onClick={this.onButtonClick}>Add 1</button>
        <div>count is {this.state.count}</div>
      </div>
    );
  }
}
```

`Function.prototype.bind(..)` creates a new wrapper function that is hard coded to ignore its own `this` binding, and use the manual one that we provide.

Hard binding is a form of explicit binding.

### `new` Binding

Last but certainly not least, we have the `new` binding. This is when we invoke a JavaScript function with the `new` keyword in front of it.

When we invoke a JavaScript function with a `new` keyword in front of it, several things happen automatically:

1. A brand new object is created
1. The newly constructed object is [[prototype]] - linked
1. The newly constructed object is set as the `this` for that function call.
1. Unless function explicitly returns its own alternative object, the `new` - invoked function call will automatically return the newly constructed object implicitly.

For example:

```javascript
function foo(a) {
  this.a = a;
}

var bar = new foo(2);
console.log(bar.a); // 2
```

In our example, we invoke function `foo()` and pass in 2. What happens next is that a new object is created and the `this` is now set to that object. 2 which is passed in to `foo()` now gets assigned to a property called `a` on that newly created object. Because `foo()` does not explicitly use a `return` statement, that newly created object is implicitly returned and set as the value of `bar`. Later on, we can get the property called `a` and console.log that value which ends up being 2.

## Order of predecence

Armed with these types of bindings, we then have to know the order of precedence. To do this, you have to ask these questions in order:

1. Is the function called with `new`? If so, use rules for `new` binding.
1. Is the function called with `call()` or `apply()`. Is `bind()` used? If so, use rules for explicit binding.
1. Is the function called with a context or owning or containing object? If so, use rules for implicit binding.
1. Otherwise, use default binding. If in strict mode, `this` becomes undefined, else it becomes the global object.

## Summary

`this` is definitely a concept that should be mastered if you want to have a solid understanding of JavaScript. The key to understanding is that `this` is determined at function invocation time and there are 4 bindings with simple rules to consider and memorize.
